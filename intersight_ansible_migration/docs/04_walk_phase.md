# Walk Phase: Configuration & Policy Management

Welcome to the **Walk Phase** – where you start making real configuration changes. These playbooks create and manage policies, profiles, and server configurations.

---

## ⚠️ Important Safety Note

> [!CAUTION]
> Unlike Crawl phase playbooks, these make **actual changes** to your Intersight environment. Always:
> 1. Test in a non-production organization first
> 2. Use `--check` mode for dry runs
> 3. Review changes before applying to production

---

## 🎯 Phase Objectives

By the end of this phase, you will:
- ✅ Collect complete server inventory
- ✅ Create and manage boot policies
- ✅ Configure BIOS policies
- ✅ Set up storage policies
- ✅ Build server profiles

---

## 📋 Prerequisites Checklist

Before starting, ensure you have:
- [ ] Completed all [Crawl Phase](03_crawl_phase.md) playbooks
- [ ] Identified your test organization Moid
- [ ] Backed up existing configurations (if any)

---

## 🚶 Playbooks in This Phase

| Playbook | Purpose | Risk Level |
|----------|---------|------------|
| `01_collect_inventory.yml` | Full inventory collection | 🟢 Read-only |
| `02_boot_policy.yml` | Create/manage boot policies | 🟡 Creates resources |
| `03_bios_policy.yml` | BIOS configuration | 🟡 Creates resources |
| `04_storage_policy.yml` | Storage configuration | 🟡 Creates resources |
| `05_server_profile.yml` | Server profile management | 🟡 Creates resources |

---

## Playbook 1: Collect Full Inventory

**File**: `playbooks/walk/01_collect_inventory.yml`

This advanced inventory playbook collects comprehensive data about your environment.

### What It Collects
- All physical servers with detailed specs
- All policies and their configurations
- All server profiles and their states
- Network and storage information

### How to Run

```bash
ansible-playbook playbooks/walk/01_collect_inventory.yml --ask-vault-pass
```

### Output Files
The playbook creates JSON files in the `output/` directory:
- `output/servers.json` - Server inventory
- `output/policies.json` - All policies
- `output/profiles.json` - Server profiles

---

## Playbook 2: Boot Policy Management

**File**: `playbooks/walk/02_boot_policy.yml`

This playbook demonstrates the full lifecycle of Intersight policies.

### Operations
- **Create**: New boot policy with local disk boot order
- **Update**: Modify existing boot policy
- **Delete**: Remove boot policy (optional, disabled by default)

### How to Run

```bash
# Create a boot policy
ansible-playbook playbooks/walk/02_boot_policy.yml --ask-vault-pass

# With custom policy name
ansible-playbook playbooks/walk/02_boot_policy.yml --ask-vault-pass \
  -e "policy_name='MyBootPolicy'"
  
# Dry run (check mode)
ansible-playbook playbooks/walk/02_boot_policy.yml --ask-vault-pass --check
```

### Policy-as-Code Concept

Instead of clicking through the GUI, you define policies in YAML:

```yaml
# This is equivalent to manually creating a boot policy in Intersight UI
boot_policy:
  name: "Ansible-Boot-Local"
  description: "Created by Ansible - Local disk boot"
  boot_devices:
    - name: "LocalDisk"
      type: "boot.LocalDisk"
      enabled: true
```

---

## Playbook 3: BIOS Policy Management

**File**: `playbooks/walk/03_bios_policy.yml`

Configure BIOS settings across your server fleet consistently.

### Common BIOS Settings
- CPU performance profiles
- Memory configuration
- Virtualization settings
- Power management

### How to Run

```bash
ansible-playbook playbooks/walk/03_bios_policy.yml --ask-vault-pass
```

---

## Playbook 4: Storage Policy Management

**File**: `playbooks/walk/04_storage_policy.yml`

Define storage configurations including RAID levels and virtual drives.

### What It Configures
- RAID controller settings
- Virtual drive creation
- Hot spare configuration
- Drive group definitions

---

## Playbook 5: Server Profile Management

**File**: `playbooks/walk/05_server_profile.yml`

The capstone playbook that ties policies together into deployable server configurations.

### What It Does
1. Creates a server profile
2. Associates policies (boot, BIOS, storage)
3. Assigns to a server (optional)
4. Deploys the profile

### Profile Workflow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     SERVER PROFILE LIFECYCLE                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   1. CREATE          2. CONFIGURE        3. ASSIGN         4. DEPLOY        │
│   ─────────          ───────────         ────────          ────────         │
│   Profile name       Attach policies     Select server     Apply config     │
│   Organization       Boot, BIOS, etc     From inventory    Reboot if needed │
│                                                                              │
│   ┌───────────┐      ┌───────────┐      ┌───────────┐      ┌───────────┐    │
│   │  Profile  │ ──▶ │ + Policies│ ──▶ │ + Server  │ ──▶ │  Running  │    │
│   │  (Empty)  │      │ (Ready)   │      │ (Assigned)│      │ (Active)  │    │
│   └───────────┘      └───────────┘      └───────────┘      └───────────┘    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 📝 Key Concepts Introduced

### 1. Idempotency

Ansible playbooks are **idempotent** – running them multiple times produces the same result:

```yaml
# Running this twice:
# 1st run: Creates "MyPolicy" → Changed
# 2nd run: "MyPolicy" exists → No change (OK)

- name: Create boot policy
  cisco.intersight.intersight_rest_api:
    resource_path: /boot/PrecisionPolicies
    api_body:
      Name: "MyPolicy"
    state: present  # 'present' creates if missing, updates if different
```

### 2. State Management

```yaml
state: present   # Create or update the resource
state: absent    # Delete the resource if it exists
```

### 3. Organization Scoping

All resources in Intersight belong to an organization:

```yaml
api_body:
  Name: "MyPolicy"
  Organization:
    - Moid: "{{ organization_moid }}"
      ObjectType: organization.Organization
```

### 4. Tags for Tracking

Tag resources created by Ansible for easy identification:

```yaml
api_body:
  Tags:
    - Key: "CreatedBy"
      Value: "Ansible"
    - Key: "Environment"
      Value: "Production"
```

---

## 🔄 Development Workflow

```bash
# 1. Check syntax first
ansible-playbook playbooks/walk/02_boot_policy.yml --syntax-check

# 2. Dry run to see what would change
ansible-playbook playbooks/walk/02_boot_policy.yml --check --ask-vault-pass

# 3. Run with verbose output for debugging
ansible-playbook playbooks/walk/02_boot_policy.yml -v --ask-vault-pass

# 4. Run for real
ansible-playbook playbooks/walk/02_boot_policy.yml --ask-vault-pass
```

---

## ✅ Phase Completion Checklist

Before moving to the Run Phase:

- [ ] Successfully created a test boot policy
- [ ] Successfully created a test BIOS policy
- [ ] Successfully created a test server profile
- [ ] Verified resources appear in Intersight UI
- [ ] Practiced deleting test resources
- [ ] Understand idempotency concept

---

## 🚀 Next Steps

You've mastered individual operations. Ready for enterprise-scale automation?

**Proceed to Run Phase**: [05_run_phase.md](05_run_phase.md)
