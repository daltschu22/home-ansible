# Home Ansible Configuration

This repository contains Ansible playbooks and roles for managing home infrastructure servers and services.

## Overview

This Ansible project is designed to manage:
- Common configurations for all hosts
- DNS services (dnsmasq)
- UPS monitoring (NUT)
- Home Assistant home automation system
- And more

## Structure

- `site.yaml`: Main playbook that includes all roles
- `homeassistant.yaml`: Dedicated playbook for Home Assistant
- `roles/`: Directory containing all available roles
  - `common/`: Base configurations for all servers
  - `dnsmasq/`: DNS service configuration
  - `nut/`: Network UPS Tools configuration
  - `homeassistant/`: Home Assistant Docker deployment

## Prerequisites

- Ansible 2.9 or newer
- SSH access to target hosts
- Inventory file with host groups defined

## Usage

### Running all configurations

```bash
ansible-playbook site.yaml
```

### Running specific roles

To target specific host groups:

```bash
ansible-playbook site.yaml --limit homeassistant
ansible-playbook site.yaml --limit dc
ansible-playbook site.yaml --limit nut
```

Or use dedicated playbooks if available:

```bash
ansible-playbook homeassistant.yaml
```

### Testing changes

To perform a dry run without making changes:

```bash
ansible-playbook site.yaml --check
```

## Roles

### Common

Basic configurations applied to all servers:
- System packages
- User setup
- Security configurations

### dnsmasq

DNS server configuration for local network.

### NUT (Network UPS Tools)

UPS monitoring and management.

### Home Assistant

Home automation system deployed via Docker and configured as a systemd service.

- Features: Docker-based deployment, systemd management, automatic startup
- Documentation: See [roles/homeassistant/README.md](roles/homeassistant/README.md)

## Adding New Hosts

1. Add the host to your inventory file under the appropriate group
2. Run the playbook with the `--limit` option for that host
3. Or run the full playbook to apply all applicable roles

## Customization

Each role may have its own variables that can be customized:
- Role default variables are in `roles/<role_name>/vars/main.yaml`
- Override these in your inventory host variables or group variables

## Troubleshooting

If you encounter issues:

1. Use `--check` to see what changes would be made
2. Increase verbosity with `-v`, `-vv`, or `-vvv`
3. Check individual role READMEs for role-specific troubleshooting
