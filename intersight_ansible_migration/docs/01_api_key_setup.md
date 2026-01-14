# Cisco Intersight API Key Setup

This guide walks you through generating and securely storing your Intersight API credentials for use with Ansible.

---

## Understanding Intersight Authentication

Ansible connects to Intersight using **API Key authentication**, which consists of two parts:

| Component | Description | Visibility |
|-----------|-------------|------------|
| **API Key ID** | Public identifier (like a username) | Visible in Intersight UI anytime |
| **Secret Key** | Private RSA key (like a password) | **Shown only once at creation!** |

> [!CAUTION]
> The Secret Key is displayed **only once** when you generate it. If you lose it, you must generate a new API key pair.

---

## Step 1: Log into Cisco Intersight

1. Navigate to [https://intersight.com](https://intersight.com)
2. Sign in with your Cisco CCO credentials
3. Select the appropriate account/organization

---

## Step 2: Navigate to API Keys

1. Click the **gear icon** (⚙️) in the top-right corner
2. Select **Settings**
3. In the left navigation, click **API Keys** under "API"

---

## Step 3: Generate a New API Key

1. Click **Generate API Key** button
2. Fill in the form:
   - **Description**: `Ansible Automation - [Your Name]`
   - **API Key Purpose**: Select `OpenAPI Schema Version 2`
   
3. Click **Generate**

---

## Step 4: Save Your Credentials

After generation, you'll see a screen with your credentials:

### API Key ID
Copy this value - it looks like:
```
1234567890abcdef12345678/1234567890abcdef12345678/1234567890abcdef12345678
```

### Secret Key (RSA Private Key)
Click **Download** to save the `SecretKey.txt` file.

The secret key looks like:
```
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA... (many lines of encoded data)
-----END RSA PRIVATE KEY-----
```

---

## Step 5: Secure Storage Setup

### Create a Secure Directory

```bash
# Create a directory outside your project for secrets
mkdir -p ~/.intersight
chmod 700 ~/.intersight

# Move your downloaded secret key there
mv ~/Downloads/SecretKey.txt ~/.intersight/

# Secure the file permissions
chmod 600 ~/.intersight/SecretKey.txt
```

### Create Your Ansible Variables File

```bash
# Navigate to your project
cd /path/to/intersight_ansible_migration

# Create the group_vars file
cat > group_vars/all.yml << 'EOF'
---
# Cisco Intersight API Configuration
# =============================================================================
# IMPORTANT: This file should be encrypted with Ansible Vault before committing
# Run: ansible-vault encrypt group_vars/all.yml
# =============================================================================

# Your Intersight API Key ID (the long string from the UI)
intersight_api_key_id: "YOUR_API_KEY_ID_HERE"

# Path to your Secret Key file (keep this outside the project!)
intersight_api_private_key: "{{ lookup('file', '~/.intersight/SecretKey.txt') }}"

# Intersight API base URL (use this for production)
intersight_api_uri: "https://intersight.com/api/v1"

# Optional: Your organization name in Intersight
intersight_organization: "default"
EOF
```

### Update with Your Actual API Key ID

Open `group_vars/all.yml` and replace `YOUR_API_KEY_ID_HERE` with your actual API Key ID.

---

## Step 6: Encrypt with Ansible Vault

Never store unencrypted API credentials in version control!

```bash
# Encrypt the file (you'll be prompted to create a password)
ansible-vault encrypt group_vars/all.yml

# View the encrypted file (requires password)
ansible-vault view group_vars/all.yml

# Edit the encrypted file (requires password)
ansible-vault edit group_vars/all.yml
```

### Running Playbooks with Vault

```bash
# Option 1: Prompt for password each time
ansible-playbook playbooks/crawl/01_test_connection.yml --ask-vault-pass

# Option 2: Use a password file (more convenient for automation)
echo "your-vault-password" > ~/.vault_pass
chmod 600 ~/.vault_pass
ansible-playbook playbooks/crawl/01_test_connection.yml --vault-password-file ~/.vault_pass

# Option 3: Set environment variable
export ANSIBLE_VAULT_PASSWORD_FILE=~/.vault_pass
ansible-playbook playbooks/crawl/01_test_connection.yml
```

> [!WARNING]
> Never commit `.vault_pass` or any plain-text password files to version control!

---

## Step 7: Verify Your Configuration

Test that your credentials work:

```bash
# Create a quick test playbook
cat > /tmp/test_api.yml << 'EOF'
---
- name: Test Intersight API Connection
  hosts: localhost
  gather_facts: false
  tasks:
    - name: Get API user info
      cisco.intersight.intersight_rest_api:
        api_private_key: "{{ intersight_api_private_key }}"
        api_key_id: "{{ intersight_api_key_id }}"
        api_uri: "{{ intersight_api_uri }}"
        resource_path: /iam/Users
        query_params:
          $top: 1
      register: user_info
      
    - name: Display result
      debug:
        msg: "Successfully connected! Found {{ user_info.api_response.Count | default(0) }} users."
EOF

# Run the test
ansible-playbook /tmp/test_api.yml --ask-vault-pass
```

Expected output:
```
TASK [Display result] *********************************************************
ok: [localhost] => {
    "msg": "Successfully connected! Found 1 users."
}
```

---

## Security Best Practices Checklist

- [ ] Secret Key stored outside project directory (`~/.intersight/`)
- [ ] Secret Key file permissions set to 600
- [ ] `group_vars/all.yml` encrypted with Ansible Vault
- [ ] Vault password stored securely (not in project)
- [ ] `.gitignore` includes sensitive file patterns
- [ ] API key description identifies its purpose

### Recommended .gitignore Entries

Add these to your project's `.gitignore`:

```gitignore
# Ansible Vault password files
.vault_pass
*vault_pass*

# Secret keys (as backup protection)
SecretKey.txt
*.pem
*.key

# Python virtual environment
venv/
.venv/

# IDE files
.idea/
.vscode/
*.swp
```

---

## Rotating API Keys

Cisco recommends rotating API keys every 90 days:

1. Generate a new API key in Intersight
2. Update `group_vars/all.yml` with the new key ID
3. Replace `~/.intersight/SecretKey.txt` with the new key
4. Test connectivity
5. Revoke the old API key in Intersight

---

## Troubleshooting

### Error: "Invalid API Key"
- Verify the API Key ID is copied exactly (no leading/trailing spaces)
- Check the key hasn't been revoked in Intersight

### Error: "Invalid signature"
- Ensure `SecretKey.txt` contains the complete key including headers
- Verify file permissions allow Ansible to read it
- Check the key matches the API Key ID (they're generated together)

### Error: "Vault password file not found"
- Verify `~/.vault_pass` exists
- Check file permissions: `chmod 600 ~/.vault_pass`

---

## What's Next?

Your API credentials are now securely configured. Continue to:

1. **Learn Ansible Basics** → [02_ansible_basics.md](02_ansible_basics.md)
2. **Start the Crawl Phase** → [03_crawl_phase.md](03_crawl_phase.md)
