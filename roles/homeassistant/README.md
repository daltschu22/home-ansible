# Home Assistant Ansible Role

This Ansible role installs and configures Home Assistant as a Docker container managed by systemd on Ubuntu servers.

## Overview

The role:
- Installs Docker and required Python dependencies
- Creates a dedicated configuration directory for Home Assistant
- Sets up Home Assistant as a systemd service for easy management
- Configures the container with proper privileges and network settings

## Prerequisites

- Ubuntu target system
- Ansible 2.9 or newer
- Internet connectivity on the target host to pull the Docker image

## Role Variables

The following variables are defined in `vars/main.yaml` and can be overridden:

| Variable | Default | Description |
|----------|---------|-------------|
| `homeassistant_config_path` | `/opt/homeassistant/config` | Path where Home Assistant configuration will be stored |
| `homeassistant_timezone` | `America/New_York` | Timezone for Home Assistant |
| `homeassistant_image` | `ghcr.io/home-assistant/home-assistant:stable` | Docker image to use |

## Usage

### Adding the Role to Your Playbook

```yaml
- name: Configure Home Assistant
  hosts: homeassistant
  roles:
    - homeassistant
```

### Running the Role

To run the full site playbook:
```bash
ansible-playbook site.yaml
```

To run only the Home Assistant role:
```bash
ansible-playbook site.yaml --limit homeassistant
```

Or using the dedicated playbook if created:
```bash
ansible-playbook homeassistant.yaml
```

### Inventory Setup

Ensure your inventory file includes the Home Assistant host(s):

```ini
[homeassistant]
homeassistant.example.com  # or IP address
```

## Systemd Service

The role creates a systemd service that:
- Starts automatically on boot
- Manages the Docker container lifecycle
- Can be controlled with standard systemd commands:

```bash
# Check status
systemctl status homeassistant

# Restart Home Assistant
systemctl restart homeassistant

# View logs
journalctl -fu homeassistant
```

## Initial Setup

After running the role, access the Home Assistant web interface at:
```
http://your-server-ip:8123
```

Follow the on-screen instructions to complete the initial setup.

## Customization

### Custom Configuration

The Home Assistant configuration is stored at the path specified by `homeassistant_config_path` (default: `/opt/homeassistant/config`).

To make configuration changes:
1. Edit files in this directory
2. Restart the service: `systemctl restart homeassistant`

### Updating Home Assistant

The role can be run again with an updated image variable to upgrade:

```yaml
homeassistant_image: "ghcr.io/home-assistant/home-assistant:2023.6.0"  # Specific version
```

## Troubleshooting

### Service Won't Start

Check logs:
```bash
journalctl -fu homeassistant
```

Verify Docker is running:
```bash
systemctl status docker
```

### Container Issues

Inspect the container:
```bash
docker inspect homeassistant
```

View container logs:
```bash
docker logs homeassistant
``` 
