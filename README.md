# MCP SSH Server

A Model Context Protocol (MCP) server that provides comprehensive SSH and serial console access to remote servers and network devices. Enables AI tools like Claude Desktop to securely manage Linux servers, network switches, and infrastructure devices through both network SSH connections and direct USB-to-Serial console access.

## Features

### Core SSH Capabilities
- SSH connection management with password or key-based authentication
- Remote command execution with timeout handling
- File upload and download via SFTP
- Directory listing and file operations
- Secure connection handling

### Network Device Management
- **USB-to-Serial Console Access** - Direct console connections via FTDI and other USB adapters
- **Network Switch Management** - Full support for Cisco IOS/IOS-XE and Aruba switches
- **Device Discovery** - Automatic detection of switch types and capabilities
- **Configuration Management** - Backup, restore, and automated configuration
- **Network Diagnostics** - Built-in ping, traceroute, and connectivity testing
- **Console-to-SSH Transition** - Automated SSH setup via console connection
- **Firmware Management** - Upload, verify, install, and rollback switch firmware

### Server Management
- Ubuntu server management tools (Nginx, SSL, packages, firewall)
- Compatible with Claude Desktop, VS Code, and other MCP-compatible clients