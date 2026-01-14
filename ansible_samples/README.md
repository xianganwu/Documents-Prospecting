# Ansible Beginner Playbooks

This directory contains a set of sample Ansible Playbooks designed to teach the basics of Ansible.

## Prerequisites

- Ansible must be installed on your control node.
- You need SSH access to the target hosts (or use `localhost` for testing).

## Files Overview

- **inventory.ini**: Defines the hosts Ansible will manage. Currently set to `localhost`.
- **01_hello_world.yml**: Checks connectivity using the `ping` module.
- **02_install_packages.yml**: Installs packages (`git`, `tree`) using the generic `package` module.
- **03_manage_services.yml**: Manages services (e.g., `nginx`). **Note**: Ensure the service is installed first.
- **04_file_manipulation.yml**: Demonstrates creating directories, copying files, and changing permissions.

## How to Run

1.  **Check Connectivity**:
    ```bash
    ansible-playbook -i inventory.ini 01_hello_world.yml
    ```

2.  **Run other playbooks**:
    ```bash
    ansible-playbook -i inventory.ini 02_install_packages.yml --ask-become-pass
    ```
    *Note: `--ask-become-pass` (or `-K`) prompts for your `sudo` password, which is usually required for installing packages or managing services.*

## Tips for Beginners

- Always check syntax before running complex playbooks:
  ```bash
  ansible-playbook --syntax-check <playbook_name.yml>
  ```
- Use `-v` or `-vv` for more verbose output if something goes wrong.
