# VPS Connection Guide for Dyad

This guide explains how to set up and use Dyad on a VPS (Virtual Private Server) with remote development capabilities.

## Overview

Dyad can be developed and run on a VPS using various remote development tools. This allows you to:

- Develop on a powerful remote machine
- Access your development environment from anywhere
- Share resources across multiple development machines
- Keep your local machine lightweight

## Prerequisites

- A VPS with Ubuntu 20.04+ or similar Linux distribution
- SSH access to your VPS
- VS Code installed locally (for remote development)

## Setup Options

### Option 1: VS Code Remote - SSH

1. **Install VS Code Remote - SSH Extension**
   ```bash
   code --install-extension ms-vscode-remote.remote-ssh
   ```

2. **Configure SSH Connection**
   
   Add to your `~/.ssh/config`:
   ```
   Host my-dyad-vps
       HostName your-vps-ip-address
       User your-username
       IdentityFile ~/.ssh/your-private-key
   ```

3. **Connect to VPS**
   - Open VS Code
   - Press F1 and type "Remote-SSH: Connect to Host"
   - Select your VPS from the list
   - VS Code will connect and open a remote window

4. **Clone and Setup Dyad**
   ```bash
   git clone https://github.com/wasalstor-web/dyad.git
   cd dyad
   npm install
   ```

### Option 2: Dev Containers on VPS

The repository includes a `.devcontainer/devcontainer.json` configuration for development containers.

1. **Install Docker on VPS**
   ```bash
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   sudo usermod -aG docker $USER
   ```

2. **Install Dev Containers Extension**
   ```bash
   code --install-extension ms-vscode-remote.remote-containers
   ```

3. **Connect via SSH and Open in Container**
   - Connect to your VPS via Remote-SSH (see Option 1)
   - Open the Dyad repository
   - Press F1 and select "Dev Containers: Reopen in Container"

### Option 3: Direct VPS Development

1. **SSH into VPS**
   ```bash
   ssh your-username@your-vps-ip
   ```

2. **Install Node.js**
   ```bash
   curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```

3. **Clone and Setup**
   ```bash
   git clone https://github.com/wasalstor-web/dyad.git
   cd dyad
   npm install
   npm run build
   ```

## VS Code Settings

The repository includes `.vscode/settings.json` for consistent development experience. These settings will automatically apply when you open the project.

## Development Container Configuration

The `.devcontainer/devcontainer.json` file provides:

- Pre-configured development environment
- Consistent dependencies across team members
- Isolated development environment

Current configuration:
```json
{
  "image": "mcr.microsoft.com/devcontainers/universal:2",
  "features": {}
}
```

## Running Dyad on VPS

### Development Mode

```bash
npm run start
```

### Production Build

```bash
npm run build
npm run package
```

## Port Forwarding

To access the Dyad UI running on your VPS:

1. **SSH Port Forwarding**
   ```bash
   ssh -L 3000:localhost:3000 your-username@your-vps-ip
   ```

2. **VS Code Automatic Port Forwarding**
   - When connected via Remote-SSH, VS Code automatically forwards ports
   - Check the "Ports" tab in VS Code to see forwarded ports

## Security Considerations

1. **Use SSH Keys** - Always use SSH key authentication instead of passwords
2. **Firewall Configuration** - Configure UFW or iptables to only allow necessary ports
3. **Keep System Updated** - Regularly update your VPS packages
   ```bash
   sudo apt update && sudo apt upgrade
   ```
4. **Use Fail2Ban** - Protect against brute force attacks
   ```bash
   sudo apt install fail2ban
   ```

## Troubleshooting

### SSH Connection Issues

```bash
# Test SSH connection
ssh -v your-username@your-vps-ip

# Check SSH service on VPS
sudo systemctl status ssh
```

### Port Forwarding Not Working

```bash
# Check if port is in use
netstat -tuln | grep 3000

# Kill process on port if needed
sudo fuser -k 3000/tcp
```

### Performance Issues

- Increase VPS resources (RAM, CPU)
- Use a VPS in a region closer to you for better latency
- Consider using a dedicated development container

## Additional Resources

- [VS Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview)
- [Dev Containers](https://code.visualstudio.com/docs/devcontainers/containers)
- [SSH Configuration Guide](https://www.ssh.com/academy/ssh/config)

## Support

For issues specific to Dyad, refer to the main [README.md](./README.md) or visit [https://dyad.sh/](https://dyad.sh/)
