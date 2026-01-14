# Ansible Basics for Beginners

This tutorial covers the fundamental Ansible concepts you need before working with Intersight automation.

---

## What is Ansible?

Ansible is an **agentless automation tool** that:
- Runs on a **control node** (your laptop/server)
- Connects to **managed nodes** via SSH or APIs
- Executes **tasks** defined in **playbooks** (YAML files)

For Intersight automation, Ansible connects directly to the Intersight API (no agent on your UCS servers).

```
┌─────────────────┐         HTTPS/API        ┌─────────────────────────┐
│  Control Node   │ ──────────────────────▶ │   Cisco Intersight      │
│  (Your Laptop)  │                          │   Cloud Platform        │
│                 │ ◀────────────────────── │                         │
│  - Ansible      │        JSON Response     │   - Manages UCS Servers │
│  - Playbooks    │                          │   - Policies & Profiles │
└─────────────────┘                          └─────────────────────────┘
```

---

## Core Concepts

### 1. Playbooks

A **playbook** is a YAML file containing automation tasks. Here's the anatomy:

```yaml
---
# Playbook header
- name: My First Playbook               # Descriptive name
  hosts: localhost                      # Where to run (localhost for API calls)
  gather_facts: false                   # Skip system facts (faster for API ops)
  
  # Variables section (optional)
  vars:
    my_variable: "some_value"
  
  # Tasks to execute
  tasks:
    - name: First task description      # What this task does
      debug:                            # Module to use
        msg: "Hello, Ansible!"          # Module parameters
        
    - name: Second task
      debug:
        msg: "Variable value is: {{ my_variable }}"
```

### 2. Tasks

A **task** is a single action that Ansible performs. Each task uses a **module**:

```yaml
tasks:
  - name: Get server information        # Human-readable description
    cisco.intersight.intersight_rest_api:  # Fully-qualified module name
      api_key_id: "{{ api_key }}"       # Module parameters
      api_private_key: "{{ api_key }}"
      resource_path: /compute/PhysicalSummaries
    register: servers                    # Store result in variable
```

### 3. Modules

**Modules** are the building blocks. For Intersight, we primarily use:

| Module | Purpose | Example Use |
|--------|---------|-------------|
| `cisco.intersight.intersight_rest_api` | All CRUD operations | Create policies, profiles |
| `cisco.intersight.intersight_info` | Query information | Get server details |
| `debug` | Display information | Print variables |
| `set_fact` | Create variables | Process API responses |

### 4. Variables

Variables make playbooks reusable:

```yaml
# Inline variables
vars:
  server_name: "my-server-01"
  
# Using variables (Jinja2 syntax)
- name: Show server
  debug:
    msg: "Server is: {{ server_name }}"

# Variables from files (group_vars/all.yml)
# Automatically loaded - no need to specify in playbook
```

### 5. Inventory

For Intersight automation, we typically use `localhost` since we're making API calls:

```ini
# inventory/hosts
[local]
localhost ansible_connection=local
```

Or use the Intersight dynamic inventory plugin (covered in Walk phase).

---

## Playbook Execution Flow

```
┌─────────────────────────────────────────────────────────────────────┐
│                      PLAYBOOK EXECUTION                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. PARSE           2. LOAD VARS        3. EXECUTE TASKS            │
│  ────────────       ────────────        ────────────────            │
│  Read YAML          Load inventory      Run each task               │
│  Validate syntax    Load group_vars     in order                    │
│                     Load host_vars                                   │
│                                                                      │
│  ansible-playbook   ──▶  Variables  ──▶  Task 1 ──▶ Task 2 ──▶ Done │
│  playbook.yml           merged          (serial)                    │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Essential Command-Line Options

```bash
# Basic execution
ansible-playbook playbook.yml

# With inventory file
ansible-playbook -i inventory/hosts playbook.yml

# With Ansible Vault password
ansible-playbook playbook.yml --ask-vault-pass

# Check mode (dry run - shows what would change)
ansible-playbook playbook.yml --check

# Verbose output (add more v's for more detail)
ansible-playbook playbook.yml -v
ansible-playbook playbook.yml -vvv

# Limit to specific hosts
ansible-playbook playbook.yml --limit webservers

# Pass extra variables
ansible-playbook playbook.yml -e "server_name=server01"

# Syntax check (no execution)
ansible-playbook playbook.yml --syntax-check

# List tasks (no execution)
ansible-playbook playbook.yml --list-tasks
```

---

## YAML Essentials

YAML is whitespace-sensitive. Common patterns:

### Key-Value Pairs
```yaml
name: "Server01"
port: 443
enabled: true
```

### Lists
```yaml
servers:
  - server01
  - server02
  - server03

# Or inline
servers: [server01, server02, server03]
```

### Dictionaries (Maps)
```yaml
server:
  name: "Server01"
  ip: "192.168.1.10"
  port: 443
```

### Multi-line Strings
```yaml
# Literal block (preserves newlines)
description: |
  This is line 1
  This is line 2

# Folded block (newlines become spaces)
description: >
  This is a very long description
  that spans multiple lines
  but becomes one line.
```

---

## Jinja2 Templating Basics

Ansible uses Jinja2 for variable interpolation:

### Variable Substitution
```yaml
- debug:
    msg: "Server {{ server_name }} has IP {{ server_ip }}"
```

### Conditionals
```yaml
- name: Only run if condition is true
  debug:
    msg: "Production server"
  when: environment == "production"
```

### Loops
```yaml
- name: Process each server
  debug:
    msg: "Processing {{ item }}"
  loop:
    - server01
    - server02
    - server03
```

### Filters
```yaml
# Convert to JSON
{{ my_dict | to_json }}

# Default value if undefined
{{ my_var | default("fallback") }}

# Get length
{{ my_list | length }}

# Extract values
{{ servers | map(attribute='name') | list }}
```

---

## Registering Results

Capture task output for later use:

```yaml
- name: Query servers
  cisco.intersight.intersight_rest_api:
    resource_path: /compute/PhysicalSummaries
  register: server_response  # Store the result

- name: Show server count
  debug:
    msg: "Found {{ server_response.api_response.Results | length }} servers"
    
- name: Loop through results
  debug:
    msg: "Server: {{ item.Name }}"
  loop: "{{ server_response.api_response.Results }}"
```

---

## Error Handling

```yaml
# Ignore errors and continue
- name: This might fail
  command: /possibly/missing/command
  ignore_errors: true

# Fail with custom message
- name: Check condition
  fail:
    msg: "No servers found!"
  when: server_count == 0

# Block with rescue (try/catch)
- block:
    - name: Try this
      command: /risky/command
  rescue:
    - name: If it fails, do this
      debug:
        msg: "The risky command failed, recovering..."
```

---

## Hands-On Exercise

Create this file as `exercise_01.yml` and run it:

```yaml
---
- name: Ansible Basics Exercise
  hosts: localhost
  gather_facts: false
  
  vars:
    my_name: "Ansible Learner"
    servers:
      - name: "server01"
        role: "web"
      - name: "server02"
        role: "db"
      - name: "server03"
        role: "web"
  
  tasks:
    - name: Step 1 - Simple greeting
      debug:
        msg: "Hello, {{ my_name }}!"

    - name: Step 2 - Loop through servers
      debug:
        msg: "Server {{ item.name }} has role {{ item.role }}"
      loop: "{{ servers }}"
      
    - name: Step 3 - Filter web servers only
      debug:
        msg: "Web server found: {{ item.name }}"
      loop: "{{ servers }}"
      when: item.role == "web"
      
    - name: Step 4 - Count servers
      debug:
        msg: "Total servers: {{ servers | length }}"
```

Run it:
```bash
ansible-playbook exercise_01.yml
```

---

## Common Mistakes to Avoid

| Mistake | Problem | Solution |
|---------|---------|----------|
| Wrong indentation | YAML parse errors | Use 2 spaces, no tabs |
| Missing quotes | Special characters break parsing | Quote strings with `:`, `{`, `[` |
| Undefined variables | Playbook fails | Use `{{ var \| default('') }}` |
| Wrong module name | Module not found | Use fully-qualified names |
| Forgetting `register` | Can't access results | Always register API responses |

---

## Quick Reference Card

```yaml
# Standard Intersight task pattern
- name: Descriptive name of what this does
  cisco.intersight.intersight_rest_api:
    api_private_key: "{{ intersight_api_private_key }}"
    api_key_id: "{{ intersight_api_key_id }}"
    api_uri: "{{ intersight_api_uri }}"
    resource_path: /api/endpoint/here
    query_params:                        # For GET requests
      $filter: "Name eq 'something'"
      $top: 10
    api_body:                            # For POST/PATCH requests
      Name: "MyResource"
      Description: "Created by Ansible"
  register: result                       # Always capture response
  
- name: Always show what happened
  debug:
    var: result
```

---

## What's Next?

You now have the Ansible fundamentals. Time to apply them:

1. **Start the Crawl Phase** → [03_crawl_phase.md](03_crawl_phase.md)
2. **Reference the playbooks** → `playbooks/crawl/`
