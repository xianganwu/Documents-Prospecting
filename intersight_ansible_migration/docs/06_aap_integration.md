# Red Hat Ansible Automation Platform (Cloud) Integration

This guide covers integrating your Intersight playbooks with Red Hat Ansible Automation Platform (AAP) hosted in the cloud.

---

## 🎯 Why AAP?

| Feature | Ansible CLI | Red Hat AAP |
|---------|-------------|-------------|
| **Scheduling** | Cron jobs | Built-in scheduler |
| **Access Control** | OS-level | Role-based access control |
| **Audit Trail** | Manual logging | Complete audit history |
| **Approvals** | Manual | Workflow approvals |
| **Credentials** | Vault files | Secure credential store |
| **API** | None | Full REST API |
| **Dashboards** | None | Visual dashboards |
| **Support** | Community | Enterprise support |

---

## 📋 Prerequisites

Before starting AAP integration:
- [ ] Red Hat AAP subscription (cloud-hosted)
- [ ] AAP Controller access URL
- [ ] Admin or appropriate user credentials
- [ ] Git repository for playbook storage
- [ ] Tested playbooks from Crawl/Walk/Run phases

---

## Step 1: Prepare Your Git Repository

AAP pulls playbooks from Git. Structure your repository:

```
your-repo/
├── README.md
├── collections/
│   └── requirements.yml         # Collection dependencies
├── inventory/
│   └── intersight.yml
├── group_vars/
│   └── all.yml                  # Non-sensitive variables
├── playbooks/
│   ├── crawl/
│   ├── walk/
│   └── run/
└── roles/
    └── intersight_common/
```

### Create collections/requirements.yml

```yaml
---
collections:
  - name: cisco.intersight
    version: ">=2.0.0"
```

### Push to Git

```bash
cd /path/to/intersight_ansible_migration
git init
git add .
git commit -m "Initial Intersight automation playbooks"
git remote add origin https://github.com/your-org/intersight-ansible.git
git push -u origin main
```

---

## Step 2: Access AAP Console

1. Navigate to your AAP cloud URL (provided by Red Hat)
2. Log in with your organizational credentials
3. You'll see the AAP Dashboard

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    ANSIBLE AUTOMATION PLATFORM                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  📊 Dashboard    📁 Projects    📋 Templates    🏃 Jobs    ⚙️ Settings       │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Step 3: Create Credentials

### 3.1 Git Credentials (for pulling playbooks)

1. Navigate to **Resources → Credentials**
2. Click **Add**
3. Configure:
   - **Name**: `GitHub - Intersight Playbooks`
   - **Credential Type**: `Source Control`
   - **Username**: Your GitHub username
   - **Password/Token**: GitHub Personal Access Token

### 3.2 Intersight API Credentials

1. Navigate to **Resources → Credentials**
2. Click **Add**
3. Configure:
   - **Name**: `Intersight API Credentials`
   - **Credential Type**: `Machine` (or create custom type)
   - **Variables** (in Extra Variables):
   ```yaml
   intersight_api_key_id: "YOUR_API_KEY_ID"
   intersight_api_private_key: |
     -----BEGIN RSA PRIVATE KEY-----
     YOUR_SECRET_KEY_HERE
     -----END RSA PRIVATE KEY-----
   intersight_api_uri: "https://intersight.com/api/v1"
   ```

### 3.3 Create Custom Credential Type (Recommended)

For better security, create a custom Intersight credential type:

1. Navigate to **Administration → Credential Types**
2. Click **Add**
3. Configure:

**Name**: `Cisco Intersight`

**Input Configuration**:
```yaml
fields:
  - id: api_key_id
    type: string
    label: API Key ID
  - id: api_private_key
    type: string
    label: API Private Key (SecretKey.txt contents)
    secret: true
    multiline: true
  - id: api_uri
    type: string
    label: API URI
    default: "https://intersight.com/api/v1"
required:
  - api_key_id
  - api_private_key
```

**Injector Configuration**:
```yaml
extra_vars:
  intersight_api_key_id: '{{ api_key_id }}'
  intersight_api_private_key: '{{ api_private_key }}'
  intersight_api_uri: '{{ api_uri }}'
```

---

## Step 4: Create Project

1. Navigate to **Resources → Projects**
2. Click **Add**
3. Configure:
   - **Name**: `Intersight Automation`
   - **Organization**: Your organization
   - **Source Control Type**: `Git`
   - **Source Control URL**: `https://github.com/your-org/intersight-ansible.git`
   - **Source Control Branch**: `main`
   - **Source Control Credential**: `GitHub - Intersight Playbooks`
   - **Options**: ☑️ Clean, ☑️ Update Revision on Launch

4. Click **Save**
5. Click **Sync** to pull the playbooks

---

## Step 5: Create Inventory

For localhost-based API calls:

1. Navigate to **Resources → Inventories**
2. Click **Add → Add inventory**
3. Configure:
   - **Name**: `Intersight Localhost`
   - **Organization**: Your organization

4. After saving, click **Hosts** tab
5. Click **Add**
6. Configure:
   - **Name**: `localhost`
   - **Variables**:
   ```yaml
   ansible_connection: local
   ansible_python_interpreter: "{{ ansible_playbook_python }}"
   ```

---

## Step 6: Create Job Templates

### 6.1 Server Inventory Report Template

1. Navigate to **Resources → Templates**
2. Click **Add → Add job template**
3. Configure:
   - **Name**: `Intersight - Collect Server Inventory`
   - **Job Type**: `Run`
   - **Inventory**: `Intersight Localhost`
   - **Project**: `Intersight Automation`
   - **Playbook**: `playbooks/walk/01_collect_inventory.yml`
   - **Credentials**: `Intersight API Credentials`
   - **Options**: ☑️ Enable Privilege Escalation (if needed)

4. Click **Save**

### 6.2 Boot Policy Template

1. Create another template for boot policy management
2. Add **Survey** for dynamic input:
   - **Prompt**: `Policy Name`
   - **Variable**: `policy_name`
   - **Default**: `Ansible-Boot-LocalDisk`
   - **Required**: Yes

### 6.3 Firmware Update Template

1. Create template for firmware updates
2. Add **Survey** for firmware version selection
3. Set **Job Slice Count** for parallel execution

---

## Step 7: Create Workflows

For complex operations, create workflows that chain templates:

### Server Provisioning Workflow

1. Navigate to **Resources → Templates**
2. Click **Add → Add workflow template**
3. Configure:
   - **Name**: `Intersight - Full Server Provisioning`
   - **Organization**: Your organization

4. Click **Visualizer** to design the workflow:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         PROVISIONING WORKFLOW                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   ┌──────────────┐    ┌──────────────┐    ┌──────────────┐                  │
│   │ Create Boot  │───▶│ Create BIOS  │───▶│   Create     │                  │
│   │   Policy     │    │   Policy     │    │ Storage Pol  │                  │
│   └──────────────┘    └──────────────┘    └──────────────┘                  │
│          │                   │                   │                           │
│          └───────────────────┴───────────────────┘                           │
│                              │                                               │
│                              ▼                                               │
│                    ┌──────────────┐    ┌──────────────┐                     │
│                    │    Create    │───▶│    Verify    │                     │
│                    │   Profile    │    │  Deployment  │                     │
│                    └──────────────┘    └──────────────┘                     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Step 8: Set Up Schedules

### Daily Inventory Collection

1. Navigate to your template
2. Click **Schedules** tab
3. Click **Add**
4. Configure:
   - **Name**: `Daily Inventory Collection`
   - **Start date/time**: Tomorrow, 6:00 AM
   - **Repeat frequency**: `Day`
   - **Every**: `1`

### Monthly Firmware Audit

1. Create schedule for firmware reports
2. Set to run monthly
3. Enable email notifications

---

## Step 9: Configure Notifications

1. Navigate to **Administration → Notifications**
2. Click **Add**
3. Configure for your preferred channel:
   - **Slack**: Webhook URL
   - **Email**: SMTP settings
   - **Microsoft Teams**: Webhook URL

4. Associate with templates for job status alerts

---

## Step 10: Role-Based Access Control

### Create Team Roles

1. **Intersight Operators**: Can run templates, view results
2. **Intersight Admins**: Can modify templates, create schedules
3. **Intersight Auditors**: Read-only access to job history

### Assign Permissions

1. Navigate to **Access → Teams**
2. Create teams
3. Assign templates with appropriate roles

---

## Best Practices for AAP

### 1. Use Surveys for Variables

Instead of hardcoding, use surveys:
```yaml
# In template survey
- question_name: "Target Organization"
  variable: target_organization
  type: multiplechoice
  choices:
    - Production
    - Staging
    - Development
```

### 2. Chain with Approval Nodes

For production operations, add approval steps:
```
[Create Policy] → [APPROVAL: Ops Manager] → [Deploy to Production]
```

### 3. Use Instance Groups

For high availability:
- Create multiple execution environments
- Use instance groups for load balancing

### 4. Implement Logging

- Enable activity stream
- Export to SIEM (Splunk, ELK)
- Retain job output for compliance

---

## Monitoring and Reporting

### Dashboard Widgets

Customize your AAP dashboard with:
- Job success/failure rates
- Most frequently run templates
- Failed hosts by template
- Pending workflow approvals

### API Integration

Query AAP API for custom reporting:

```bash
# Get recent job runs
curl -H "Authorization: Bearer YOUR_TOKEN" \
  https://your-aap-host/api/v2/jobs/?page_size=10

# Get template statistics
curl -H "Authorization: Bearer YOUR_TOKEN" \
  https://your-aap-host/api/v2/job_templates/ID/jobs/?status=successful
```

---

## Troubleshooting AAP

### Job Fails with "Module Not Found"

**Solution**: Add collection to requirements.yml:
```yaml
collections:
  - name: cisco.intersight
```

### Credential Not Injected

**Solution**: Verify credential type injector configuration matches variable names in playbooks.

### Slow Project Sync

**Solution**: Enable "Clean" option to reset between syncs.

---

## Migration Checklist

- [ ] Git repository created and accessible
- [ ] Credentials configured in AAP
- [ ] Project synced successfully
- [ ] Inventory with localhost created
- [ ] At least one job template working
- [ ] Schedules configured for recurring tasks
- [ ] Notifications set up for critical jobs
- [ ] RBAC configured for team access
- [ ] Documentation updated for operators

---

## Congratulations! 🎉

You have completed the full migration from Cisco UCS Intersight GUI to Ansible Automation Platform!

**What you've achieved:**
- 🐢 **Crawl**: Learned Ansible fundamentals and API connectivity
- 🚶 **Walk**: Created policies and profiles as code
- 🏃 **Run**: Built enterprise-scale automation
- 🚀 **AAP**: Integrated with Red Hat Ansible Automation Platform

**Your new capabilities:**
- Infrastructure as Code for UCS management
- Automated, scheduled operations
- Role-based access control
- Complete audit trail
- Integration with enterprise workflows
