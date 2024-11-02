# Cisco Packet Tracer

## Switch and End Device Configuration

### IOS CLI Main Command Modes

Switch to privileged mode
```
enable
```
<br>Switch from privileged to user mode
```
disable
```
<br>Enter global configuration mode
```
configure terminal
```
<br>Exit global configuration mode
```
exit
```
<br>Enter line sub-configuration mode
```
line console 0
# line LINE_TYPE NUMBER_OR_RANGE
```
-   **LINE TYPE**: Defines the type of access you want to configure. Common values are:
    
    -   `console`: For direct access via the console port.
    -   `aux`: For access via the auxiliary port.
    -   `vty`: For remote access via SSH or Telnet.
-   **LINE NUMBER | RANGE**: Specifies the line number or range of lines you want to configure.
    
    -   For example, `console 0`, `aux 0`, or `vty 0 4`.

<br>Return to enable mode
```
end
```
<br>Access contextual help
```
?
```
<br>View the clock
```
show clock
```
<br>Set the clock
```
clock set 16:20:00 22 Feb 2030
```
<br>Assign a hostname
```
hostname NAME
```
<br>Set a Console User Password
```
line console 0
password PASSWORD
login
```
```
line vty 0 15
password PASSWORD
login
```
<br>Set a Privileged Console Password
```
enable secret PASSWORD

```
**`enable secret`** stores the password in hashed form, offering more security than **`enable password`**, which stores it in plain text.
<br>
> [!TIP]
 The `startup-config` and `running-config` files display stored passwords in plain text, which is a security risk. To encrypt them, use the command `service password-encryption`.

<br>Set a warning message
```
configure terminal
banner motd #Authorized Access Only#
```

<br>There are two system files that store device configuration:
* `Startup-config`: configuration saved in NVRAM used at startup.
* `Running-config`: active configuration in RAM, lost upon power-off.

To save the current configuration to the startup file, run `copy running-config startup-config` in privileged mode.

<br>Restart the router or switch.
```
reload
```
<br>Erase the saved configuration to start with the default settings.
```
erase startup-config
```
<br>View the status of interfaces
```
Router# show ip interface brief

Interface              IP-Address      OK? Method Status     Protocol
GigabitEthernet0/0     192.168.1.1     YES manual up         up
GigabitEthernet0/1     unassigned      YES unset  administratively down down
Serial0/0              10.1.1.1        YES manual up         up
Serial0/1              unassigned      YES unset  down       down
Loopback0              192.168.0.1     YES manual up         up
```

### Hot Keys and Shortcuts

![01](https://github.com/user-attachments/assets/738969c3-a4ca-402d-8de7-b7ad96da962d)
![02](https://github.com/user-attachments/assets/d5630434-1f0e-4e92-8eb9-9b84f99b246b)


### Configure an SVI on a Switch

```
configure terminal
interface vlan 1
ip address 192.168.1.20 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.1.1
```
