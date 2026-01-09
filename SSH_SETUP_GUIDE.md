# SSH Setup Guide - Console to SSH Transition

This guide covers the new SSH setup capabilities that allow you to automatically configure SSH access on network switches via console connection, eliminating the need for ongoing USB-to-Serial management.

## Overview

The SSH setup tools provide a complete workflow to transition from console-based management to SSH-based management:

1. **Connect via console** - Use USB-to-Serial adapter for initial access
2. **Automated SSH configuration** - Apply network and SSH settings automatically
3. **Test connectivity** - Verify SSH is working properly
4. **Switch to SSH** - Disconnect console cable and use SSH for all future management

## New SSH Setup Tools

### `switch_generate_ssh_config`
Generate SSH configuration template for network switch.

**Parameters:**
- `hostname` (required) - Switch hostname
- `ip_address` (required) - Management IP address  
- `gateway` (required) - Default gateway IP address
- `username` (required) - SSH username
- `password` (required) - SSH password
- `deviceType` (optional) - Device type (cisco, aruba) - default: cisco
- `securityLevel` (optional) - Security level (basic, secure) - default: basic
- `subnet_mask` (optional) - Subnet mask - default: 255.255.255.0
- `enable_password` (optional) - Enable password - defaults to SSH password

### `switch_apply_ssh_config`
Apply SSH configuration to switch via serial console connection.

**Parameters:**
- `serialConnectionId` (required) - ID of active serial console connection
- `hostname` (required) - Switch hostname
- `ip_address` (required) - Management IP address
- `gateway` (required) - Default gateway IP address
- `username` (required) - SSH username
- `password` (required) - SSH password
- `confirmApply` (required) - Must be set to `true` for safety
- `deviceType` (optional) - Device type (cisco, aruba)
- `securityLevel` (optional) - Security level (basic, secure)

### `switch_test_ssh_connection`
Test SSH connection to newly configured switch.

**Parameters:**
- `ip_address` (required) - Switch IP address
- `username` (required) - SSH username
- `password` (required) - SSH password
- `port` (optional) - SSH port - default: 22
- `timeout` (optional) - Connection timeout in milliseconds - default: 10000

### `switch_complete_ssh_setup`
Complete end-to-end SSH setup workflow via console connection.

**Parameters:**
- `serialConnectionId` (required) - ID of active serial console connection
- `hostname` (required) - Switch hostname
- `ip_address` (required) - Management IP address
- `gateway` (required) - Default gateway IP address
- `username` (required) - SSH username
- `password` (required) - SSH password
- `confirmSetup` (required) - Must be set to `true` for safety
- `deviceType` (optional) - Device type (cisco, aruba)
- `securityLevel` (optional) - Security level (basic, secure)
- `testConnection` (optional) - Test SSH after setup - default: true

## Security Levels

### Basic Security
- Standard SSH configuration
- Basic authentication
- Default timeouts
- Minimal hardening

### Secure Security  
- Enhanced SSH configuration
- Stronger encryption settings
- Shorter timeouts
- Additional hardening:
  - Disables HTTP/HTTPS management
  - Enables password encryption
  - Sets login banners
  - Configures exec timeouts

## Complete Workflow Examples

### Automated Setup (Recommended)
```
1. "List available serial ports"
2. "Connect to COM3 for Cisco switch console"
3. "Complete SSH setup workflow for my switch with IP 192.168.1.10, gateway 192.168.1.1, username admin, password SecurePass123, hostname CoreSwitch01, deviceType cisco, securityLevel secure, confirmSetup=true"
4. "Disconnect from console connection"
5. "Connect to switch at 192.168.1.10 via SSH with username admin"
6. "Show all interface status on the switch"
7. "Backup the running configuration"
```

### Step-by-Step Setup
```
1. "List available serial ports"
2. "Connect to COM3 for Cisco switch console"
3. "Generate SSH configuration for Cisco switch with IP 192.168.1.10, gateway 192.168.1.1, username admin, password SecurePass123, hostname CoreSwitch01, securityLevel secure"
4. "Apply SSH configuration to switch via console connection with confirmApply=true"
5. "Test SSH connection to 192.168.1.10 with username admin and password SecurePass123"
6. "Disconnect from console connection"
7. "Connect to switch at 192.168.1.10 via SSH"
```

## Device-Specific Configuration

### Cisco IOS/IOS-XE Switches
**Basic Configuration:**
- Sets hostname and domain name
- Generates RSA keys (2048-bit)
- Configures SSH version 2
- Creates privileged user account
- Configures management VLAN interface
- Sets default gateway
- Saves configuration

**Secure Configuration:**
- All basic features plus:
- 4096-bit RSA keys
- Shorter SSH timeouts
- Disables HTTP services
- Enables password encryption
- Sets login banner
- Configures exec timeouts

### Aruba Switches (ArubaOS-Switch)
**Basic Configuration:**
- Sets hostname
- Configures management VLAN
- Generates SSH keys (2048-bit)
- Enables SSH service
- Creates user accounts
- Sets default gateway
- Saves configuration

**Secure Configuration:**
- All basic features plus:
- 4096-bit SSH keys
- Disables web management
- Disables telnet service
- Sets login banner
- Enhanced timeouts

## Network Requirements

### IP Configuration
- **Management IP:** Must be in same subnet as gateway
- **Subnet Mask:** Typically 255.255.255.0 (/24)
- **Gateway:** Must be reachable from management network
- **DNS:** Optional, uses domain name if specified

### Network Connectivity
- Switch must have physical connection to management network
- Gateway must be configured and reachable
- No IP conflicts with existing devices
- Firewall must allow SSH traffic (port 22)

## Troubleshooting

### Configuration Issues
- **Invalid IP format:** Verify IP addresses are in correct format
- **Network unreachable:** Check physical connections and VLAN configuration
- **Key generation fails:** May take 30-60 seconds, be patient
- **Configuration not saved:** Ensure copy/write commands complete successfully

### SSH Connection Issues
- **Connection refused:** Wait 30 seconds after configuration for SSH service to start
- **Authentication failed:** Verify username/password are correct
- **Network unreachable:** Check IP configuration and routing
- **Timeout:** Increase timeout value or check network connectivity

### Recovery Options
- **SSH setup failed:** Console connection remains active for troubleshooting
- **Wrong IP configured:** Use console to reconfigure network settings
- **SSH not working:** Use console to verify and fix SSH configuration
- **Complete failure:** Factory reset switch and start over

## Security Best Practices

### Password Requirements
- **Minimum 8 characters** for SSH passwords
- **Mix of letters, numbers, symbols** recommended
- **Avoid common passwords** or dictionary words
- **Different passwords** for SSH and enable access

### Network Security
- **Management VLAN:** Use dedicated VLAN for switch management
- **Access Control:** Limit SSH access to authorized networks
- **Monitoring:** Log SSH access and configuration changes
- **Regular Updates:** Keep switch firmware updated

### Physical Security
- **Console Port:** Secure physical access to console ports
- **Cable Management:** Properly secure and label console cables
- **Documentation:** Document switch locations and access methods

## Benefits of SSH Setup Tools

### Automation Benefits
- **Consistent Configuration:** Eliminates manual configuration errors
- **Time Savings:** Complete setup in minutes instead of manual step-by-step
- **Standardization:** Ensures all switches follow same security standards
- **Reduced Errors:** Automated validation prevents common mistakes

### Operational Benefits
- **Remote Management:** No need for physical console access after setup
- **Scalability:** Easy to configure multiple switches consistently
- **Security:** Stronger security configurations than manual setup
- **Documentation:** Automatic configuration templates for reference

### Maintenance Benefits
- **No Console Cables:** Eliminate need for USB-to-Serial adapters
- **Remote Access:** Manage switches from anywhere on network
- **Backup Integration:** Easy integration with configuration backup tools
- **Monitoring:** Better integration with network monitoring systems

This SSH setup capability transforms the initial switch configuration process from a manual, error-prone task into an automated, reliable workflow that gets your switches ready for production network management quickly and securely.
