# Network Switch Management Guide

This guide covers the network switch management capabilities added to the SSH-MCP server, including support for Cisco and Aruba switches via both SSH and USB-to-Serial connections.

## Overview

The SSH-MCP server now includes comprehensive network switch management tools that support:

- **Cisco IOS/IOS-XE switches** - Full command support for Catalyst, Nexus, and other Cisco switches
- **Aruba switches** - Support for ArubaOS-Switch (formerly ProCurve) devices
- **SSH connections** - Standard network SSH access to switches
- **USB-to-Serial console** - Direct console access via USB-to-Serial adapters
- **Device discovery** - Automatic detection of switch types and capabilities
- **Configuration management** - Backup, restore, and configuration tools
- **Network diagnostics** - Built-in ping, traceroute, and connectivity testing

## New Tools Available

### SSH-Based Switch Tools

#### `switch_discover_device`
Automatically discover and identify the type of network switch connected via SSH.

**Parameters:**
- `connectionId` (required) - ID of active SSH connection
- `enablePassword` (optional) - Enable password for privileged mode

**Example:**
```
Connect to my Cisco switch at 192.168.1.10 and discover what type of device it is
```

#### `switch_show_interfaces`
Display interface status and configuration on network switches.

**Parameters:**
- `connectionId` (required) - ID of active SSH connection
- `interfaceType` (optional) - Type of interfaces to show (all, ethernet, gigabit)
- `enablePassword` (optional) - Enable password for privileged mode

**Example:**
```
Show all interface status on the connected switch
```

#### `switch_show_vlans`
Display VLAN configuration and status.

**Parameters:**
- `connectionId` (required) - ID of active SSH connection
- `enablePassword` (optional) - Enable password for privileged mode

**Example:**
```
Show all VLANs configured on the switch
```

#### `switch_backup_config`
Backup switch configuration (running or startup config).

**Parameters:**
- `connectionId` (required) - ID of active SSH connection
- `configType` (optional) - Type of config to backup (running, startup)
- `enablePassword` (optional) - Enable password for privileged mode

**Example:**
```
Backup the running configuration from the switch
```

#### `switch_network_diagnostics`
Run network diagnostics from the switch (ping, traceroute).

**Parameters:**
- `connectionId` (required) - ID of active SSH connection
- `target` (required) - Target IP address or hostname
- `diagnosticType` (optional) - Type of diagnostic (ping, traceroute)
- `enablePassword` (optional) - Enable password for privileged mode

**Example:**
```
Ping 8.8.8.8 from the switch to test internet connectivity
```

#### `switch_show_mac_table`
Display MAC address table on the switch.

**Parameters:**
- `connectionId` (required) - ID of active SSH connection
- `vlan` (optional) - Specific VLAN to show MAC addresses for
- `enablePassword` (optional) - Enable password for privileged mode

**Example:**
```
Show the MAC address table for VLAN 10
```

### USB-to-Serial Console Tools

#### `serial_list_ports`
List all available USB-to-Serial ports on the system.

**Example:**
```
Show me all available serial ports for console connections
```

#### `serial_connect`
Connect to a network device via USB-to-Serial console port.

**Parameters:**
- `port` (required) - Serial port name (e.g., COM3, /dev/ttyUSB0)
- `baudRate` (optional) - Baud rate (default: 9600)
- `dataBits` (optional) - Data bits (default: 8)
- `stopBits` (optional) - Stop bits (default: 1)
- `parity` (optional) - Parity (none, even, odd - default: none)
- `flowControl` (optional) - Enable flow control (default: false)
- `connectionId` (optional) - Unique identifier for connection
- `deviceType` (optional) - Device type for optimal settings (cisco, aruba, generic)

**Example:**
```
Connect to my Cisco switch via console cable on COM3
```

#### `serial_send_command`
Send a command to network device via serial connection.

**Parameters:**
- `connectionId` (required) - ID of active serial connection
- `command` (required) - Command to send to device
- `waitForResponse` (optional) - Wait for device response (default: true)
- `timeout` (optional) - Response timeout in milliseconds

**Example:**
```
Send "show version" command to the switch via console connection
```

#### `serial_discover_device`
Discover device type and capabilities via serial connection.

**Parameters:**
- `connectionId` (required) - ID of active serial connection

**Example:**
```
Discover what type of device is connected to the console port
```

#### `serial_list_connections`
List all active serial connections.

**Example:**
```
Show me all active console connections
```

#### `serial_disconnect`
Disconnect from a serial port.

**Parameters:**
- `connectionId` (required) - ID of active serial connection

**Example:**
```
Disconnect from the console connection
```

## Supported Switch Types

### Cisco IOS/IOS-XE
- **Catalyst switches** (2960, 3560, 3750, 3850, 9300, 9400 series)
- **Nexus switches** (Basic support)
- **ISR routers** with switching modules
- **Commands supported:**
  - `show version`, `show running-config`, `show startup-config`
  - `show interfaces status`, `show vlan brief`
  - `show mac address-table`, `show arp`
  - `show cdp neighbors detail`, `show lldp neighbors detail`
  - `show inventory`, `show power`, `show environment`
  - `ping`, `traceroute`

### Aruba Switches (ArubaOS-Switch)
- **2530 series** (formerly ProCurve 2530)
- **2540 series** (formerly ProCurve 2540)
- **2930F/M series**
- **3810M series**
- **Commands supported:**
  - `show version`, `show running-config`, `show config`
  - `show interfaces brief`, `show vlans`
  - `show mac-address-table`, `show arp`
  - `show lldp info remote-device`
  - `show system`, `show power-consumption`
  - `ping`, `traceroute`

## Connection Methods

### SSH Connection
1. Use standard `ssh_connect` tool to establish SSH connection
2. Use switch-specific tools with the connection ID
3. Most modern switches support SSH out of the box

**Example workflow:**
```
1. "Connect to my switch at 192.168.1.10 with username admin"
2. "Discover what type of switch this is"
3. "Show all interfaces on the switch"
4. "Backup the running configuration"
```

### USB-to-Serial Console Connection
1. Connect USB-to-Serial adapter to switch console port
2. Use `serial_list_ports` to find available ports
3. Use `serial_connect` to establish console connection
4. Use `serial_send_command` for direct command execution

**Example workflow:**
```
1. "Show me available serial ports"
2. "Connect to COM3 for a Cisco switch console"
3. "Send 'enable' command to enter privileged mode"
4. "Send 'show version' to get device information"
```

## Common Use Cases

### Initial Switch Setup
When setting up a new switch that doesn't have SSH configured:

1. Connect via USB-to-Serial console
2. Discover device type
3. Configure basic network settings
4. Enable SSH access
5. Switch to SSH connection for ongoing management

### Network Troubleshooting
For diagnosing network issues:

1. Connect to switch (SSH or console)
2. Check interface status
3. Verify VLAN configuration
4. Review MAC address table
5. Run network diagnostics (ping/traceroute)

### Configuration Management
For backing up and managing switch configurations:

1. Connect to switch
2. Backup running configuration
3. Make configuration changes
4. Backup updated configuration
5. Compare configurations if needed

### Network Discovery
For mapping network topology:

1. Connect to each switch
2. Discover device type and model
3. Check CDP/LLDP neighbors
4. Document interface connections
5. Map VLAN assignments

## Hardware Requirements

### USB-to-Serial Adapters
- **FTDI-based adapters** (recommended) - FT232R, FT232H chipsets
- **Prolific PL2303** adapters (common, good compatibility)
- **CP2102/CP2104** Silicon Labs adapters
- **Generic CH340** adapters (budget option)

### Console Cables
- **Cisco console cable** - RJ45 to DB9 or USB
- **Aruba/HP console cable** - RJ45 to DB9 or USB
- **Universal console cable** - Works with most devices

### Driver Installation
- **Windows:** Install manufacturer drivers (FTDI, Prolific, etc.)
- **macOS:** Usually plug-and-play, may need drivers for some adapters
- **Linux:** Most adapters work out-of-the-box with kernel drivers

## Troubleshooting

### Serial Connection Issues
- **Port not found:** Check driver installation and device manager
- **No response:** Verify baud rate (usually 9600 for switches)
- **Garbled text:** Check data bits, stop bits, and parity settings
- **Connection timeout:** Ensure console cable is properly connected

### SSH Connection Issues
- **Connection refused:** Verify SSH is enabled on switch
- **Authentication failed:** Check username/password or SSH keys
- **Command timeout:** Some commands take longer, increase timeout
- **Permission denied:** May need enable password for privileged commands

### Device Detection Issues
- **Unknown device type:** Device may use non-standard command syntax
- **Partial command support:** Some older devices have limited command sets
- **Privilege level:** Some commands require enable mode or admin privileges

## Security Considerations

### SSH Connections
- Use strong passwords or SSH key authentication
- Limit SSH access to management VLANs
- Consider using non-standard SSH ports
- Enable SSH logging and monitoring

### Console Connections
- Secure physical access to console ports
- Use console port security features if available
- Monitor console access and commands
- Consider console server for remote console access

### Best Practices
- Always backup configurations before making changes
- Use read-only accounts when possible
- Log all configuration changes
- Implement change management procedures
- Test changes in lab environment first

## Future Enhancements

Planned improvements for network switch management:

- **Real serialport integration** - Replace mock implementation with actual serialport library
- **Configuration templates** - Pre-built configurations for common scenarios
- **Bulk operations** - Manage multiple switches simultaneously
- **Firmware management** - Upload and install switch firmware updates
- **Performance monitoring** - Real-time interface statistics and utilization
- **SNMP integration** - Additional monitoring and management via SNMP
- **Network mapping** - Automatic topology discovery and visualization
- **Configuration comparison** - Compare configurations between devices or versions

## Examples

### Complete Cisco Switch Setup
```
1. "List available serial ports"
2. "Connect to COM3 for Cisco switch console"
3. "Send 'enable' command"
4. "Send 'configure terminal' command"
5. "Send 'interface vlan 1' command"
6. "Send 'ip address 192.168.1.10 255.255.255.0' command"
7. "Send 'no shutdown' command"
8. "Send 'exit' command"
9. "Send 'ip ssh version 2' command"
10. "Send 'username admin privilege 15 secret mypassword' command"
11. "Send 'copy running-config startup-config' command"
12. "Disconnect from console"
13. "Connect to switch at 192.168.1.10 via SSH"
14. "Backup the running configuration"
```

### Network Troubleshooting Workflow
```
1. "Connect to my core switch at 192.168.1.1"
2. "Show all interface status"
3. "Show VLAN configuration"
4. "Show MAC address table for VLAN 100"
5. "Ping 192.168.100.1 from the switch"
6. "Traceroute to 8.8.8.8 from the switch"
7. "Backup current configuration for documentation"
```

This comprehensive network switch management capability transforms your SSH-MCP server into a powerful network administration tool suitable for both initial device setup and ongoing network management tasks.