# EVPN/VXLAN Data Center Fabric with Ansible & CI/CD

This project showcases an advanced, automated approach to configuring a Cisco Nexus EVPN/VXLAN fabric using Infrastructure-as-Code principles. It demonstrates how a Network Engineer can leverage Ansible, templates, YAML inventories, and a CI/CD pipeline (via GitHub Actions) to deliver consistent, validated, and quickly deployable network configurations.

## Key Highlights

- **Modern Network Design:** Implements a leaf-spine topology with Cisco Nexus switches running BGP EVPN for VXLAN overlays.
- **Ansible-Driven Automation:** Uses Ansible and Jinja2 templates to generate device configurations dynamically from YAML variables.
- **Group Vars & Templates:** Separates fabric-wide parameters from device-specific details.
- **Continuous Integration (CI):** A GitHub Actions workflow runs lint checks (e.g., YAML linting, Ansible syntax checks) on every commit to ensure configuration integrity.
- **Scalability & Extensibility:** Easily add more leaves/spines, adjust overlay settings, or integrate additional services.

## Topology Overview

The topology consists of:
- **2 Spine Switches (Nexus 9000):** Provide underlay and EVPN route-reflector functionality.
- **2 Leaf Switches (Nexus 9000):** Connect to servers and form the VXLAN fabric overlays.
- **Ansible Control Node:** Runs Ansible playbooks and manages inventory, pushing configs to all devices.

## Files & Directories

- `inventory.yml`: Defines the groups (spines, leafs) and hosts with IP/credentials.
- `group_vars/spines.yml` and `group_vars/leafs.yml`: Store variables such as BGP AS numbers, VLANs, and interface assignments.
- `roles/evpn_fabric`: Contains tasks and templates to configure BGP EVPN, NVE interfaces, and VLANs.
- `.github/workflows/ci.yml`: Defines CI pipeline steps to lint YAML and test the Ansible playbook syntax on every push.

## Prerequisites

- **Ansible Installed**: `pip install ansible ansible-lint yamllint`
- **Network Connectivity**: The Ansible control node must reach the management interfaces of the Nexus switches.
- **Cisco Nexus Devices**: Running NX-OS with API and SSH enabled.
- **Credentials**: Update `inventory.yml` or use Ansible Vault if required.

## How to Use

1. **Edit Inventory & Vars**:  
   Update `inventory.yml` with your device IPs and credentials.  
   Adjust `group_vars/leafs.yml` and `group_vars/spines.yml` for ASNs, VLAN IDs, and loopback addresses.

2. **Check Syntax & Lint**:  
   Run locally:
   ```bash
   ansible-playbook --syntax-check playbook.yml
   ansible-lint playbook.yml
   yamllint .
