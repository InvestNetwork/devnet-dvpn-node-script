# InvestNet dVPN Node Setup Script

A streamlined management script for deploying and operating an InvestNet dVPN node on **Raspberry Pi (ARM64)** systems. This script simplifies the installation of dependencies, binary management, and service orchestration.

## Features

- **Automated Dependency Management**: Installs WireGuard packages and required tools.
- **Architecture-Aware**: Automatically downloads the optimized ARM64 binary for Raspberry Pi.
- **Service Orchestration**: Integrates with `systemd` for reliable background operation and auto-restart.
- **Network Optimization**: Handles IP forwarding, NAT (iptables/nftables), and dynamic egress interface detection.
- **One-Command Management**: Simple interface for initialization, starting, stopping, and status monitoring.

## Prerequisites

- **Hardware**: Raspberry Pi (ARM64 architecture).
- **OS**: Linux (Debian/Ubuntu recommended).
- **Access**: Root or `sudo` privileges are required.
- **Connectivity**: Stable internet connection.

## Quick Start

1.  **Download and Prepare the Script**:
    ```bash
    chmod +x investnet-node.sh
    ```

2.  **Initialize the Node**:
    ```bash
    sudo ./investnet-node.sh init
    ```
    *This will install WireGuard, download the binary, and prompt for configuration (moniker, pricing, and keys).*

3.  **Start the Node**:
    ```bash
    sudo ./investnet-node.sh start
    ```

## CLI Commands

The script follows a simple command structure: `sudo ./investnet-node.sh <command>`

| Command      | Description                                                                 |
| :----------- | :-------------------------------------------------------------------------- |
| `init`       | Installs dependencies, downloads binary, and interactive node setup.       |
| `start`      | Configures networking and starts the node as a `systemd` service.           |
| `stop`       | Stops the node service and cleans up WireGuard interfaces.                  |
| `restart`    | Restarts the node service.                                                  |
| `status`     | Shows service status, logs, WireGuard peers, and NAT rules.                 |
| `uninstall`  | **Reversible**: Stops node, removes service, data, and binary.              |

## Configuration Details

- **Binary Port**: 18133 (TCP)
- **WireGuard Port**: 51820 (UDP)
- **Data Directory**: `${HOME}/.investnet-dvpnx`
- **Systemd Service**: `investnet-dvpn-node.service`

> [!IMPORTANT]
> **Backup Your Mnemonic!** During the `init` phase, you will be given a mnemonic. Store this securely; if lost, your node's identity and potential earnings cannot be recovered.

## Troubleshooting

Use the `status` command to diagnose issues:
```bash
sudo ./investnet-node.sh status
```
This command provides a comprehensive view of:
- Service health (`systemctl status`)
- Recent logs (`journalctl`)
- WireGuard interface state (`wg show`)
- IP Forwarding status
- NAT/Masquerade rules
