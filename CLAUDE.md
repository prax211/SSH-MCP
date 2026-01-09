# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an SSH-MCP (Model Context Protocol) Server that provides SSH and serial console access to remote servers and network devices. The server enables AI assistants to manage Linux servers, network switches, and other infrastructure devices through both SSH connections and USB-to-Serial console connections.

## Key Features

- **SSH Connection Management** - Secure SSH connections with password or key-based authentication
- **USB-to-Serial Console Access** - Direct console connections to network devices via USB adapters
- **Network Switch Management** - Support for Cisco IOS/IOS-XE and Aruba switches
- **Automated SSH Setup** - Configure SSH on switches via console, then transition to SSH management
- **Ubuntu Server Tools** - Nginx, SSL, package management, and firewall tools
- **Network Diagnostics** - Built-in ping, traceroute, and connectivity testing

## Key Commands

### Build & Development
- `npm run build` - Compile TypeScript to JavaScript (outputs to `build/`)
- `npm run dev` - Development mode with ts-node
- `npm start` - Start the MCP server (requires built files)
- `npm test` - Run tests (not yet configured)

### Installation
- `npm install` - Install dependencies
- `npm install -g .` - Install globally for system-wide access

## Architecture

### Core Components

- **Main Entry Point**: `src/index.ts` - MCP server initialization and tool registration
- **Network Switch Tools**: `src/network-switch-tools.ts` - SSH-based switch management
- **Serial Connection Tools**: `src/serial-connection-tools.ts` - USB-to-Serial console access
- **SSH Setup Tools**: `src/ssh-setup-tools.ts` - Automated SSH configuration for switches
- **Console Transition Tools**: `src/console-transition-tools.ts` - Complete console-to-SSH workflow
- **Ubuntu Website Tools**: `src/ubuntu-website-tools.ts` - Ubuntu server management
- **Switch Firmware Tools**: `src/switch-firmware-tools.ts` - Firmware management (future)

### Key Tool Categories

#### SSH Connection Tools
- `ssh_connect` - Establish SSH connection to remote server
- `ssh_exec` - Execute commands on remote server
- `ssh_upload_file` - Upload files via SFTP
- `ssh_download_file` - Download files via SFTP
- `ssh_list_files` - List directory contents
- `ssh_disconnect` - Close SSH connection

#### Serial Console Tools
- `serial_list_ports` - List available USB-to-Serial ports
- `serial_connect` - Connect to device via console port
- `serial_send_command` - Send commands to device
- `serial_discover_device` - Auto-detect device type
- `serial_list_connections` - List active console connections
- `serial_disconnect` - Close console connection

#### Network Switch Tools (SSH)
- `switch_discover_device` - Identify switch type and capabilities
- `switch_show_interfaces` - Display interface status
- `switch_show_vlans` - Show VLAN configuration
- `switch_backup_config` - Backup switch configuration
- `switch_network_diagnostics` - Run ping/traceroute
- `switch_show_mac_table` - Display MAC address table

#### SSH Setup Tools (Console-based)
- `switch_generate_ssh_config` - Generate SSH configuration template
- `switch_apply_ssh_config` - Apply SSH config via console
- `switch_test_ssh_connection` - Test SSH connectivity
- `switch_complete_ssh_setup` - Complete automated setup workflow
- `console_to_ssh_transition` - Full transition from console to SSH

#### Ubuntu Server Tools
- `ubuntu_nginx_control` - Web server control
- `ubuntu_update_packages` - System updates
- `ubuntu_ssl_certificate` - Let's Encrypt SSL management
- `ubuntu_website_deployment` - Website deployment with backup
- `ubuntu_ufw_firewall` - Firewall management

## Development Notes

### TypeScript Configuration
- Target: ES2020
- Module: ES2020
- Output directory: `build/`
- Strict mode enabled

### Dependencies
- **@modelcontextprotocol/sdk** - MCP protocol implementation
- **ssh2** - SSH client for Node.js
- **serialport** - USB-to-Serial communication
- **@serialport/parser-readline** - Serial data parsing

### MCP Integration
The server runs as a Model Context Protocol server that communicates via stdio. Configure in Claude Desktop or other MCP clients by pointing to the built `build/index.js` file.

Example configuration:
```json
{
  "mcpServers": {
    "ssh-server": {
      "command": "node",
      "args": ["/path/to/SSH-MCP/build/index.js"]
    }
  }
}
```

## Supported Devices

### Network Switches
- **Cisco IOS/IOS-XE** - Catalyst, Nexus series
- **Aruba ArubaOS-Switch** - 2530, 2540, 2930, 3810 series

### USB-to-Serial Adapters
- FTDI-based adapters (FT232R, FT232H)
- Prolific PL2303 adapters
- Silicon Labs CP2102/CP2104
- CH340 chipset adapters

## Common Workflows

### Initial Switch Setup via Console
1. List available serial ports
2. Connect to console port
3. Run complete SSH setup workflow
4. Test SSH connection
5. Disconnect console
6. Use SSH for ongoing management

### Network Switch Management via SSH
1. Connect to switch via SSH
2. Discover device type
3. View interfaces and VLANs
4. Backup configuration
5. Run diagnostics as needed

### Ubuntu Server Management
1. Connect to server via SSH
2. Update packages
3. Manage Nginx web server
4. Configure SSL certificates
5. Deploy website files

## Security Considerations

- Use SSH key-based authentication when possible
- Secure physical access to console ports
- Use dedicated management VLANs for switch access
- Keep switch firmware and server packages updated
- Use strong passwords (minimum 8 characters, mixed types)
- Limit SSH access to authorized networks
- Monitor and log all configuration changes

## Real-World Testing

This tool has been successfully tested in production network environments:
- Discovered previously unknown fluttering ports on switches
- Successfully managed multiple switches without causing network disruption
- Automated SSH setup reduced configuration time significantly

## Future Enhancements

- Enhanced serial port integration
- Firmware management capabilities
- Bulk switch operations
- Network topology mapping
- Configuration templates
- SNMP integration
- Real-time performance monitoring
