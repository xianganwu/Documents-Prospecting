# Crawl Phase: Building Your Foundation

Welcome to the **Crawl Phase** – your first steps with Intersight automation using Ansible. These playbooks are completely **read-only** and safe to run against your production environment.

---

## 🎯 Phase Objectives

By the end of this phase, you will:
- ✅ Verify API connectivity to Intersight
- ✅ Query basic server information
- ✅ List organizations and understand Intersight's structure
- ✅ Feel confident running Ansible playbooks

---

## 📋 Prerequisites Checklist

Before starting, ensure you have completed:
- [ ] [Environment Setup](00_prerequisites.md)
- [ ] [API Key Configuration](01_api_key_setup.md)
- [ ] [Ansible Basics](02_ansible_basics.md) (recommended)

---

## 🐢 Playbooks in This Phase

| Playbook | Purpose | Risk Level |
|----------|---------|------------|
| `01_test_connection.yml` | Verify API connectivity | 🟢 None |
| `02_get_server_info.yml` | Query server information | 🟢 None |
| `03_list_organizations.yml` | List all organizations | 🟢 None |

---

## Playbook 1: Test Connection

**File**: `playbooks/crawl/01_test_connection.yml`

This playbook verifies that your API credentials are working correctly.

### What It Does
1. Connects to Intersight API
2. Queries the IAM Users endpoint
3. Displays connection status

### How to Run

```bash
cd /path/to/intersight_ansible_migration
ansible-playbook playbooks/crawl/01_test_connection.yml --ask-vault-pass
```

### Expected Output

```
PLAY [Test Intersight API Connection] ******************************************

TASK [Verify API credentials are loaded] ***************************************
ok: [localhost] => {
    "msg": "Testing connection with API Key ID: 1234567890..."
}

TASK [Query Intersight API] ****************************************************
ok: [localhost]

TASK [Display connection status] ***********************************************
ok: [localhost] => {
    "msg": "✅ SUCCESS: Connected to Intersight. API is responding."
}

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0
```

### Troubleshooting

| Error | Cause | Solution |
|-------|-------|----------|
| `401 Unauthorized` | Invalid API credentials | Regenerate API keys |
| `Connection timeout` | Network issues | Check proxy settings |
| `Vault password required` | Forgot --ask-vault-pass | Add the flag |

---

## Playbook 2: Get Server Information

**File**: `playbooks/crawl/02_get_server_info.yml`

This playbook retrieves basic information about all servers managed by Intersight.

### What It Does
1. Queries the Physical Server Summaries endpoint
2. Displays a summary of each server
3. Shows server names, serials, and status

### How to Run

```bash
ansible-playbook playbooks/crawl/02_get_server_info.yml --ask-vault-pass
```

### Expected Output

```
TASK [Display server summary] **************************************************
ok: [localhost] => {
    "msg": [
        "=== Server Inventory Summary ===",
        "Total servers found: 5",
        "",
        "Server: UCS-SERVER-01 | Serial: FCH12345678 | Status: InService",
        "Server: UCS-SERVER-02 | Serial: FCH12345679 | Status: InService",
        "Server: UCS-SERVER-03 | Serial: FCH12345680 | Status: Maintenance",
    ]
}
```

### Understanding the Output

| Field | Description |
|-------|-------------|
| Name | Server hostname in Intersight |
| Serial | Physical server serial number |
| Status | Current operational status |

---

## Playbook 3: List Organizations

**File**: `playbooks/crawl/03_list_organizations.yml`

This playbook lists all organizations in your Intersight account.

### What It Does
1. Queries the Organization endpoint
2. Lists all organizations and their descriptions
3. Shows resource groups (if any)

### How to Run

```bash
ansible-playbook playbooks/crawl/03_list_organizations.yml --ask-vault-pass
```

### Why Organizations Matter

Intersight uses organizations to:
- **Segment resources** by business unit, environment, or region
- **Control access** through role-based permissions
- **Organize policies** and profiles logically

```
Organizations in Intersight
├── default                    # Default org for all accounts
├── Production                 # Production servers
├── Development               # Dev/test environment
└── DR-Site                   # Disaster recovery
```

---

## 📝 Key Learnings from Crawl Phase

### 1. The Standard Intersight Task Pattern

Every Intersight playbook follows this pattern:

```yaml
- name: Descriptive task name
  cisco.intersight.intersight_rest_api:
    api_private_key: "{{ intersight_api_private_key }}"
    api_key_id: "{{ intersight_api_key_id }}"
    api_uri: "{{ intersight_api_uri }}"
    resource_path: /endpoint/here
  register: response

- name: Process the response
  debug:
    var: response.api_response
```

### 2. Understanding API Responses

Intersight API responses have a consistent structure:

```json
{
  "api_response": {
    "ObjectType": "compute.PhysicalSummary.List",
    "Count": 5,
    "Results": [
      { "Name": "Server01", "Serial": "FCH123" },
      { "Name": "Server02", "Serial": "FCH456" }
    ]
  }
}
```

Access data with Jinja2:
```yaml
# Get the count
{{ response.api_response.Count }}

# Loop through results
loop: "{{ response.api_response.Results }}"

# Access specific fields
{{ item.Name }}
{{ item.Serial }}
```

### 3. Common Query Parameters

```yaml
resource_path: /compute/PhysicalSummaries
query_params:
  $filter: "Name eq 'Server01'"      # Filter by name
  $top: 10                            # Limit results
  $skip: 0                            # Pagination offset
  $orderby: "Name asc"                # Sort order
  $select: "Name,Serial,Status"       # Return only these fields
```

---

## ✅ Phase Completion Checklist

Before moving to the Walk Phase, ensure:

- [ ] `01_test_connection.yml` runs successfully
- [ ] `02_get_server_info.yml` returns your servers
- [ ] `03_list_organizations.yml` shows your orgs
- [ ] You understand the standard task pattern
- [ ] You can interpret API responses

---

## 🚀 Next Steps

Congratulations on completing the Crawl Phase! You've proven that:
- Your environment is configured correctly
- Your API credentials work
- You can run Ansible playbooks successfully

**Ready for configuration changes?** → [Walk Phase Guide](04_walk_phase.md)
