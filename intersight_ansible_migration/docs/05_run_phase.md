# Run Phase: Enterprise Automation at Scale

Welcome to the **Run Phase** – production-grade automation for enterprise operations. These playbooks handle bulk operations, complex workflows, and integration with Red Hat Ansible Automation Platform (AAP).

---

## ⚠️ Production Warning

> [!CAUTION]
> These playbooks perform operations that can affect multiple servers simultaneously. 
> **Always test in a staging environment first!**

---

## 🎯 Phase Objectives

By the end of this phase, you will:
- ✅ Provision multiple servers in bulk
- ✅ Orchestrate firmware updates across your fleet
- ✅ Automate complete server lifecycle management
- ✅ Integrate with Red Hat AAP (Hosted)

---

## 📋 Prerequisites

Before starting:
- [ ] Completed all Walk Phase playbooks
- [ ] Have a tested, working set of policies
- [ ] Identified staging/test servers for validation
- [ ] (Optional) Red Hat AAP subscription for cloud integration

---

## 🏃 Playbooks in This Phase

| Playbook | Purpose | Risk Level |
|----------|---------|------------|
| `01_bulk_provision.yml` | Provision multiple servers | 🔴 High |
| `02_firmware_update.yml` | Fleet firmware updates | 🔴 High |
| `03_lifecycle_management.yml` | Full server lifecycle | 🔴 High |

---

## Playbook 1: Bulk Server Provisioning

**File**: `playbooks/run/01_bulk_provision.yml`

Provision multiple servers simultaneously using profile templates.

### Features
- Read server list from YAML or CSV
- Create profiles from templates
- Parallel deployment support
- Progress tracking and reporting
- Rollback capabilities

### How to Run

```bash
# Dry run to see what would happen
ansible-playbook playbooks/run/01_bulk_provision.yml --ask-vault-pass --check

# Full execution
ansible-playbook playbooks/run/01_bulk_provision.yml --ask-vault-pass
```

---

## Playbook 2: Firmware Update Orchestration

**File**: `playbooks/run/02_firmware_update.yml`

Orchestrate firmware updates across your server fleet with safety controls.

### Safety Features
- Pre-flight health checks
- Rolling updates (one server at a time option)
- Backup current settings
- Automatic rollback on failure
- Post-update verification

### How to Run

```bash
# Check current firmware versions
ansible-playbook playbooks/run/02_firmware_update.yml --ask-vault-pass \
  -e "operation=report_only"

# Perform updates (after review)
ansible-playbook playbooks/run/02_firmware_update.yml --ask-vault-pass \
  -e "operation=update"
```

---

## Playbook 3: Complete Lifecycle Management

**File**: `playbooks/run/03_lifecycle_management.yml`

Automate the complete server lifecycle from provisioning to decommissioning.

### Lifecycle Stages
1. **Provision**: Create profiles, assign policies
2. **Deploy**: Apply configuration to hardware
3. **Operate**: Monitor, backup, update
4. **Decommission**: Clean removal and policy cleanup

---

## 📊 Best Practices for Production

### 1. Use Change Windows

```yaml
# Add to playbooks for maintenance window enforcement
vars:
  maintenance_windows:
    weekday: [0, 6]  # Sunday, Saturday
    hours: [22, 23, 0, 1, 2, 3, 4, 5]  # 10 PM - 5 AM
    
tasks:
  - name: Check if within maintenance window
    fail:
      msg: "This operation can only run during maintenance windows"
    when: 
      - not ansible_date_time.weekday | int in maintenance_windows.weekday
      - not ansible_date_time.hour | int in maintenance_windows.hours
```

### 2. Implement Approvals

```yaml
# Pause for manual approval on critical operations
- name: Require manual approval for firmware update
  pause:
    prompt: |
      
      ⚠️  FIRMWARE UPDATE PENDING
      
      Servers to update: {{ servers_to_update | length }}
      Target firmware: {{ target_firmware_version }}
      
      Press ENTER to continue or Ctrl+C to abort
```

### 3. Log Everything

```yaml
# Comprehensive logging
- name: Log operation to file
  lineinfile:
    path: "/var/log/ansible/intersight_operations.log"
    line: "{{ ansible_date_time.iso8601 }} | {{ operation }} | {{ server_name }} | {{ result }}"
    create: yes
  delegate_to: localhost
```

### 4. Use Ansible Vault for All Secrets

```bash
# Create encrypted secrets file
ansible-vault create secrets.yml

# Contents
intersight_api_key_id: "YOUR_KEY"
intersight_api_private_key: |
  -----BEGIN RSA PRIVATE KEY-----
  ...
  -----END RSA PRIVATE KEY-----
```

---

## 🔄 Error Handling Patterns

```yaml
# Block with rescue for rollback
- block:
    - name: Apply new configuration
      include_tasks: apply_config.yml
      
    - name: Verify configuration
      include_tasks: verify_config.yml
      
  rescue:
    - name: Configuration failed - rolling back
      include_tasks: rollback.yml
      
    - name: Notify ops team
      uri:
        url: "{{ slack_webhook }}"
        method: POST
        body_format: json
        body:
          text: "⚠️ Ansible rollback triggered for {{ server_name }}"
```

---

## 🚀 Scaling Considerations

### For 100+ Servers

```yaml
# Use serial execution for safety
- hosts: all
  serial: 
    - 1     # First, one server
    - 5     # Then batches of 5
    - "20%" # Then 20% at a time
```

### For High Throughput

```yaml
# Increase parallelism (use with caution)
- hosts: all
  strategy: free  # Don't wait for slowest host
  throttle: 50    # Max concurrent operations
```

---

## ✅ Phase Completion Checklist

Before considering Run phase complete:

- [ ] Successfully ran bulk provisioning in test
- [ ] Successfully updated firmware on test servers
- [ ] Created rollback procedures
- [ ] Documented runbooks for operations
- [ ] Trained team on playbook usage
- [ ] Integrated with AAP (optional)

---

## 🎓 You've Completed the Migration!

Congratulations! You have successfully migrated from Intersight GUI to Ansible automation.

**What you've achieved:**
- 🐢 **Crawl**: Connected to Intersight and queried your inventory
- 🚶 **Walk**: Created policies and profiles via code
- 🏃 **Run**: Built enterprise-scale automation

**Next steps:**
1. Review [AAP Integration Guide](06_aap_integration.md) for cloud hosting
2. Create organization-specific playbooks
3. Build CI/CD pipelines for policy changes
4. Train your team on the new workflows
