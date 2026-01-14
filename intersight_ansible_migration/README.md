# Cisco UCS Intersight to Ansible Migration Guide

> **Complete Crawl-Walk-Run Roadmap for Enterprise Automation**

Welcome to your comprehensive guide for migrating from Cisco UCS Intersight GUI-based management to Ansible automation. This guide is designed for teams with limited Ansible experience and progressively builds your skills toward production-ready automation.

---

## 🎯 Why Migrate to Ansible?

| Feature | Intersight GUI | Ansible Automation |
|---------|----------------|-------------------|
| **Repeatability** | Manual clicks each time | Run playbooks anytime |
| **Version Control** | Limited audit trails | Full Git history |
| **Scale** | One operation at a time | Parallel operations across fleet |
| **Integration** | Limited to Intersight APIs | Integrate with any system |
| **Compliance** | Manual verification | Automated enforcement |

---

## 📋 Migration Phases Overview

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         CRAWL-WALK-RUN METHODOLOGY                         │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   🐢 CRAWL (Weeks 1-2)         🚶 WALK (Weeks 3-4)          🏃 RUN (Week 5+) │
│   ─────────────────────        ─────────────────────        ──────────────── │
│   • Learn Ansible basics       • Policy management          • Full automation │
│   • Test API connectivity      • Server profiles            • Firmware updates│
│   • Read-only queries          • Inventory collection       • Lifecycle mgmt  │
│   • Build confidence           • Configuration changes      • AAP integration │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Quick Start (5 Minutes)

### Step 1: Install Prerequisites
```bash
# Verify Python 3.7+
python3 --version

# Install Ansible
pip3 install ansible

# Install the Cisco Intersight collection
ansible-galaxy collection install cisco.intersight
```

### Step 2: Generate API Keys
Follow [01_api_key_setup.md](docs/01_api_key_setup.md) to create your Intersight API credentials.

### Step 3: Run Your First Playbook
```bash
cd playbooks/crawl
ansible-playbook 01_test_connection.yml
```

---

## 📚 Documentation Index

### Setup Guides
| Document | Description | Time to Complete |
|----------|-------------|------------------|
| [Prerequisites](docs/00_prerequisites.md) | System requirements & installation | 15 min |
| [API Key Setup](docs/01_api_key_setup.md) | Generate & secure Intersight credentials | 10 min |
| [Ansible Basics](docs/02_ansible_basics.md) | Core concepts for newcomers | 30 min |

### Phase Guides
| Phase | Document | Playbooks Included |
|-------|----------|-------------------|
| 🐢 Crawl | [Crawl Phase Guide](docs/03_crawl_phase.md) | 3 read-only playbooks |
| 🚶 Walk | [Walk Phase Guide](docs/04_walk_phase.md) | 5 configuration playbooks |
| 🏃 Run | [Run Phase Guide](docs/05_run_phase.md) | 3 advanced playbooks |

### Advanced Topics
| Document | Description |
|----------|-------------|
| [AAP Integration](docs/06_aap_integration.md) | Migrating to Red Hat Ansible Automation Platform (Cloud) |

---

## 📁 Project Structure

```
intersight_ansible_migration/
├── README.md                    # This file
├── docs/                        # Tutorial documentation
├── inventory/
│   └── intersight.yml           # Dynamic inventory config
├── group_vars/
│   └── all.yml                  # Shared variables (vault encrypted)
├── playbooks/
│   ├── crawl/                   # Beginner playbooks (read-only)
│   ├── walk/                    # Intermediate playbooks (configs)
│   └── run/                     # Advanced playbooks (automation)
└── roles/
    └── intersight_common/       # Reusable automation role
```

---

## ⚡ Playbook Quick Reference

### Crawl Phase (Safe to Run Anytime)
```bash
# Test API connectivity
ansible-playbook playbooks/crawl/01_test_connection.yml

# Get server information
ansible-playbook playbooks/crawl/02_get_server_info.yml

# List organizations
ansible-playbook playbooks/crawl/03_list_organizations.yml
```

### Walk Phase (Makes Configuration Changes)
```bash
# Create boot policy
ansible-playbook playbooks/walk/02_boot_policy.yml

# Create server profile
ansible-playbook playbooks/walk/05_server_profile.yml
```

### Run Phase (Production Operations)
```bash
# Bulk provisioning
ansible-playbook playbooks/run/01_bulk_provision.yml

# Firmware updates
ansible-playbook playbooks/run/02_firmware_update.yml
```

---

## 🔐 Security Best Practices

1. **Never commit API keys** to version control
2. **Use Ansible Vault** to encrypt sensitive variables
3. **Store SecretKey.txt** outside the project directory
4. **Rotate API keys** periodically (every 90 days recommended)

```bash
# Encrypt your credentials file
ansible-vault encrypt group_vars/all.yml

# Run playbooks with vault password
ansible-playbook playbooks/crawl/01_test_connection.yml --ask-vault-pass
```

---

## 📞 Getting Help

- **Cisco Intersight Ansible Collection**: [GitHub Repository](https://github.com/CiscoDevNet/intersight-ansible)
- **Ansible Documentation**: [docs.ansible.com](https://docs.ansible.com)
- **Red Hat AAP**: [Red Hat Ansible Automation Platform](https://www.redhat.com/en/technologies/management/ansible)

---

## 📝 Migration Checklist

- [ ] Complete prerequisites installation
- [ ] Generate and secure API keys
- [ ] Complete Ansible basics tutorial
- [ ] Successfully run all Crawl phase playbooks
- [ ] Customize Walk phase playbooks for your environment
- [ ] Test Run phase playbooks in non-production
- [ ] Migrate to Red Hat AAP (optional)
- [ ] Document your organization-specific policies

---

**Ready to begin?** Start with [Prerequisites](docs/00_prerequisites.md) →
