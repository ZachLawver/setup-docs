Cisco I - Setup Documentation

## 1. Accessing Cisco IOS

Common access methods:

- Console: Direct physical connection, often used for initial configuration.

- SSH: Encrypted remote management (preferred).
  
- Telnet: Unencrypted remote management (not recommended).

### Cisco IOS modes

User EXEC -- `Switch>` -- Basic monitoring 

Privileged EXEC -- `Switch#` -- Advanced commands

Global configuration -- `Switch(config)#` -- Device-wide settings

Line configuration -- `Switch(config-line)#` -- Console and VTY access

Interface configuration -- `Switch(config-if)#` Interface settings


Switch> enable -- (User Exec to Privileged Exec)

Switch# configure terminal -- (Privileged Exec to Global Config)

Switch(config)# exit -- (Leave Global Config)

Switch# exit -- (Return to User Exec)

Switch>


Useful navigation commands: `enable`, `configure terminal`, `exit`, `end`, and `?` for context-sensitive help.



## 2. Basic Switch Configuration

### Step 1: Enter configuration mode

Switch> enable

Switch# configure terminal


### Step 2: Set the hostname

Switch(config)# hostname *SW1*

SW1(config)#


### Step 3: Protect privileged EXEC mode

SW1(config)# enable secret *Class123!*

`enable secret` stores a hashed secret for privileged access.


### Step 4: Configure console login

SW1(config)# line console 0

SW1(config-line)# password *Console123!*

SW1(config-line)# login

SW1(config-line)# exit


### Step 5: Configure VTY lines (basic classroom example)

SW1(config)# line vty 0 15

SW1(config-line)# password *Remote123!*

SW1(config-line)# login

SW1(config-line)# exit

VTY lines provide remote terminal access. This alone does **not** establish secure SSH access. Production configurations should use SSH and strong authentication.


### Step 6: Encrypt plaintext passwords

SW1(config)# service password-encryption

**Important:** This uses weak, reversible obfuscation for certain passwords; it is not strong encryption. I decrypted this with 7 lines of code. 


### Step 7: Add a message-of-the-day banner

SW1(config)# banner motd *#Authorized Access Only#*

**Note:** Mandatory legally in order to prosecute unauthorized access


### Step 8: Prevent unwanted DNS Lookups

SW1(config)# no ip domain-lookup 



## 3. Saving Configuration

Running configuration -- RAM -- Active settings

Startup configuration -- NVRAM -- Settings loaded on boot

SW1# copy running-config startup-config (Copies RAM to NVRAM)

Verification commands:

show running-config

show startup-config

show version

show interface *f0/18*

Unsaved changes can be lost after reboot.



## 4. Configuring IP Addresses

A typical IPv4 setup uses an IP address, subnet mask, and (when needed) default gateway.

### Switch management SVI (Switch Virtual Interface)

SW1(config)# interface vlan 1

SW1(config-if)# ip address *192.168.1.2 255.255.255.0*

SW1(config-if)# no shutdown (**Important** Command must be used to run VLAN)

SW1(config-if)# exit

SW1(config)# ip default-gateway *192.168.1.1* 

An SVI typically requires an active switch port in the corresponding VLAN to become operational. The default gateway address requires a router at that address to work.



## 5. Verifying Connectivity

From PC1:

ping *192.168.1.11*

On the switch:

Switch# show ip interface brief

Switch# show running-config

Switch# show interfaces

**Note:** Commands can also be run in global config mode.



## 6. Complete Basic Switch Setup

enable

configure terminal

hostname *SW1*

enable secret *Class123!*

service password-encryption

banner motd *#Authorized Access Only#*

line console 0

password *Console123!*

login

exit

line vty 0 15

password *Remote123!*

login

exit

interface vlan 1

ip address *192.168.1.2 255.255.255.0*

no shutdown

exit

ip default-gateway *192.168.1.1*

end

copy running-config startup-config



## 7. Command Reference

`enable` -- Enter privileged EXEC mode

`configure terminal` -- Enter global configuration

`hostname SW1` -- Set device name

`enable secret` -- Set privileged access secret

`line console 0` -- Configure console access

`line vty 0 15` -- Configure remote terminal lines

`service password-encryption` -- Obfuscate certain plaintext passwords

`banner motd` -- Set login banner

`interface vlan 1` -- Select management SVI

`ip address` -- Assign IPv4 address

`no shutdown` -- Administratively enable interface

`ip default-gateway` -- Set management default gateway

`show ip interface brief` -- Check interface status and IPs

`copy running-config startup-config` -- Save configuration

`ping` -- Test IP connectivity
