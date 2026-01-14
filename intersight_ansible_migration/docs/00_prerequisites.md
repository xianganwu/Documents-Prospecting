# Prerequisites & Environment Setup

This guide walks you through setting up your local environment to run Ansible playbooks against Cisco Intersight.

---

## System Requirements

| Requirement | Minimum Version | Check Command |
|-------------|-----------------|---------------|
| Python | 3.7+ | `python3 --version` |
| pip | Latest | `pip3 --version` |
| Ansible | 2.15+ | `ansible --version` |
| Operating System | Linux, macOS, or WSL2 | N/A |

---

## Step 1: Install Python

### macOS
```bash
# Using Homebrew (recommended)
brew install python@3.11

# Verify installation
python3 --version
```

### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv

# Verify installation
python3 --version
```

### Linux (RHEL/CentOS/Rocky)
```bash
sudo dnf install python3 python3-pip

# Verify installation
python3 --version
```

---

## Step 2: Create a Python Virtual Environment

Using a virtual environment keeps your project dependencies isolated.

```bash
# Navigate to the project directory
cd /path/to/intersight_ansible_migration

# Create virtual environment
python3 -m venv venv

# Activate it
# On macOS/Linux:
source venv/bin/activate

# On Windows (WSL2):
source venv/bin/activate

# Your prompt should now show (venv) prefix
```

> **Pro Tip**: Add `source /path/to/intersight_ansible_migration/venv/bin/activate` to your `~/.bashrc` or `~/.zshrc` to auto-activate.

---

## Step 3: Install Ansible

```bash
# Ensure pip is up to date
pip install --upgrade pip

# Install Ansible
pip install ansible

# Verify installation
ansible --version
```

Expected output:
```
ansible [core 2.15.x]
  config file = None
  configured module search path = [...]
  ansible python module location = [...]
  ansible collection location = [...]
  executable location = [...]
  python version = 3.x.x
```

---

## Step 4: Install the Cisco Intersight Ansible Collection

The `cisco.intersight` collection provides all the modules needed to interact with Intersight.

```bash
# Install the collection
ansible-galaxy collection install cisco.intersight

# Verify installation
ansible-galaxy collection list | grep intersight
```

Expected output:
```
cisco.intersight    2.x.x
```

---

## Step 5: Install Additional Python Dependencies

The Intersight collection requires the Intersight SDK:

```bash
# Install the Intersight SDK
pip install intersight

# Verify installation
pip show intersight
```

---

## Step 6: Verify Your Setup

Create a simple test to verify everything is working:

```bash
# Check Ansible can find the collection
ansible-doc -l | grep intersight

# You should see modules like:
# cisco.intersight.intersight_info
# cisco.intersight.intersight_rest_api
```

---

## Directory Structure Check

After setup, your project should look like this:

```
intersight_ansible_migration/
├── venv/                        # Python virtual environment
├── README.md
├── docs/
├── inventory/
├── group_vars/
├── playbooks/
│   ├── crawl/
│   ├── walk/
│   └── run/
└── roles/
```

---

## Common Issues & Troubleshooting

### Issue: "ansible-galaxy: command not found"
**Solution**: Ensure your virtual environment is activated:
```bash
source venv/bin/activate
```

### Issue: "No module named 'intersight'"
**Solution**: Install the Intersight SDK:
```bash
pip install intersight
```

### Issue: SSL Certificate Errors
**Solution**: Update your CA certificates:
```bash
# macOS
brew install ca-certificates

# Linux
sudo apt install ca-certificates
```

### Issue: Python version too old
**Solution**: Use pyenv to manage multiple Python versions:
```bash
# Install pyenv
curl https://pyenv.run | bash

# Install Python 3.11
pyenv install 3.11
pyenv local 3.11
```

---

## What's Next?

Now that your environment is ready:

1. **Generate API Keys** → [01_api_key_setup.md](01_api_key_setup.md)
2. **Learn Ansible Basics** → [02_ansible_basics.md](02_ansible_basics.md)
3. **Start the Crawl Phase** → [03_crawl_phase.md](03_crawl_phase.md)

---

## Environment Checklist

- [ ] Python 3.7+ installed
- [ ] Virtual environment created and activated
- [ ] Ansible 2.15+ installed
- [ ] cisco.intersight collection installed
- [ ] intersight Python SDK installed
- [ ] All verification commands pass
