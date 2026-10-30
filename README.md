::: {align="center"}
# 🔐 Cybersecurity Lab Environment Setup

**Building a virtual lab for cybersecurity, penetration-testing, and
ethical-hacking practice**

**Author: K. Pranay Kumar**\
**Program: Cybersecurity at Networkwalks \| Batch B084 \| Week 01**
:::<p align="center">
<img src="https://img.shields.io/badge/Project-Cybersecurity-404040?style=flat-square&labelColor=C00000" />{=html}
<img src="https://img.shields.io/badge/Platform-VirtualBox-0070C0?style=flat-square&labelColor=000000" />{=html}
<img src="https://img.shields.io/badge/OS-Kali%20Linux-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />{=html}
<img src="https://img.shields.io/badge/Networking-NAT%20Network-238F89?style=flat-square&labelColor=000000" />{=html}
<img src="https://img.shields.io/badge/Focus-Ethical%20Hacking-C00000?style=flat-square&labelColor=000000" />{=html}
<img src="https://img.shields.io/badge/Batch-B084-404040?style=flat-square&labelColor=C00000" />{=html}
</p>
📌 Project Overview
This project documents the setup of a virtual cybersecurity lab using
Oracle VirtualBox and Kali Linux as part of the Cybersecurity
Program at Networkwalks, Batch B084.

The aim is to prepare a controlled virtual environment for learning
cybersecurity fundamentals, Linux networking, and authorized
penetration-testing practice. The lab can be extended later by adding
other virtual machines for safe, permission-based testing.
🎯 Objectives
The main objectives of this project are to:
- Set up and launch Kali Linux in a virtual machine.
- Configure a private VirtualBox NAT Network.
- Configure IPv4 settings for the Kali Linux network connection.
- Review the IP address, subnet mask, gateway, and DNS configuration.
- Manage the network connection using NetworkManager commands.
- Test network connectivity and DNS resolution.
- Create a clean virtual-machine snapshot for recovery.
- Document the process using screenshots and clear steps.
🛡️ Purpose of the Lab
A virtual lab provides a convenient environment to learn and practise
cybersecurity without directly testing systems that are not owned by the
learner.

Possible future learning activities include:
- Network reconnaissance in an authorized lab.
- Port scanning of lab machines.
- Network traffic analysis.
- Vulnerability assessment in a controlled environment.
- Learning Linux security tools.
- Practising secure configuration and troubleshooting.
⚠️ Responsible use: Only test systems and networks you own or have
explicit permission to assess. Keep practice within your controlled
lab environment.

🏗️ Lab Architecture
The lab uses Kali Linux as a virtual machine running through VirtualBox.
The virtual network is configured separately from the physical network
and can be expanded with additional lab machines later.
                 Host Computer
                      |
                 VirtualBox
                      |
              Virtual NAT Network
                 10.0.0.0/24
                      |
                Kali Linux VM
                 10.0.0.2/24
                      |
           Gateway / DNS shown in
             the configuration:
                  10.0.0.1
The diagram represents the configuration shown in the provided
screenshot. Confirm the VirtualBox NAT Network settings and actual
connectivity tests before relying on this configuration.

⚙️ Lab Configuration
  Component                        Configuration shown / used
  Virtualization platform          Oracle VirtualBox
Security operating system        Kali Linux
Virtual network mode             NAT Network
IPv4 configuration method        Manual
Kali IPv4 address                10.0.0.2
Prefix length                    /24
Subnet mask                      255.255.255.0
Gateway                          10.0.0.1
DNS server shown in screenshot   10.0.0.1
Network connection name          Wired connection 1

Note: Use the address range, gateway, and DNS server that match your
own VirtualBox network. The values above are taken from the supplied
IPv4 configuration screenshot.
🪜 Lab Setup Procedure
Step 1. Prepare VirtualBox and Kali Linux
VirtualBox is used to run Kali Linux in a virtual machine. Start the
Kali Linux VM and check that the desktop loads correctly.

Evidence: screenshots/01-kali-linux-desktop.png
Step 2. Configure the Virtual Network
Create or select a VirtualBox NAT Network for the lab. Confirm its
network prefix and DHCP settings in VirtualBox before assigning a manual
IP address.

A NAT Network can allow virtual machines attached to the same network to
communicate with each other and, when configured appropriately, access
external networks through NAT.
Step 3. Configure IPv4 Settings
The supplied screenshot shows the following manual IPv4 configuration:
Address:    10.0.0.2
Prefix:     24
Gateway:    10.0.0.1
DNS server: 10.0.0.1
Evidence: screenshots/02-ipv4-network-configuration.png
Step 4. Reactivate the Network Connection
The terminal screenshot shows these commands being used to update and
reactivate the NetworkManager connection:
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
The visible terminal output reports that the connection was deactivated
and then activated successfully.

Evidence: screenshots/03-network-connection-reactivated.png
The successful activation message confirms that the connection was
brought up. It does not, by itself, prove internet connectivity or
successful DNS resolution.

Step 5. Verify Network Connectivity and DNS
Run the following commands in Kali Linux and review their actual output:
ip addr
ip route
ping -c 4 10.0.0.1
ping -c 4 1.1.1.1
getent hosts kali.org
- ip addr displays network interface addresses.
- ip route displays routing information.
- The gateway ping checks whether the configured gateway responds.
- The external IP ping tests external reachability when permitted.
- getent hosts kali.org checks hostname resolution through the
  system's configured name-service setup.
Network policies can block some tests. Record the real output and do not
mark a test as successful unless it passes.

Evidence to add:
screenshots/04-connectivity-and-dns-verification.png
Step 6. Create a Clean Virtual-Machine Snapshot
After verifying the lab configuration, create a VirtualBox snapshot so
the VM can be restored to a known baseline before future experiments.

Suggested snapshot name:
Clean Kali - Network Setup
Evidence to add: screenshots/05-virtualbox-vm-snapshot.png

Only include this step as completed after confirming that the snapshot
exists.
🔎 Lab Verification Checklist
  Test                    Command / action                   What to check
  IP address              ip addr                          Expected IPv4 address
is shown

  Default route           ip route                         Expected
gateway/default route
is present

  Gateway reachability    ping -c 4 10.0.0.1               Replies are received,
if ICMP is allowed

  External connectivity   ping -c 4 1.1.1.1                Replies are received,
if external ICMP is
allowed

  DNS resolution          getent hosts kali.org            A hostname address is
returned

  Network connection      nmcli connection show --active   The intended connection
is active
Snapshot                Check VirtualBox Snapshots         The baseline snapshot
is listed
🐞 Troubleshooting Notes
Network connection does not work after configuration
If the connection fails after changing IPv4 settings:
1. Confirm the IP address and prefix match the NAT Network.
2. Check that the gateway is correct.
3. Check the active connection name using:
   nmcli connection show
4. Review the active connection and route:
   nmcli connection show --active
   ip route
5. Reactivate the correct connection if appropriate:
   sudo nmcli connection down "Wired connection 1"
   sudo nmcli connection up "Wired connection 1"
Connection names may differ on other systems. Use the name shown on your
own machine.
💡 What I Learned
Through this project, I practised and explored:
1. Virtualization
VirtualBox allows an operating system such as Kali Linux to run inside a
virtual machine.
2. NAT Network
A NAT Network provides a way to connect multiple VMs to the same virtual
network while supporting network address translation for outbound
access, depending on its configuration.
3. IPv4 Configuration
I worked with IP addresses, subnet prefixes, gateways, and DNS server
settings.
4. NetworkManager
I used nmcli to modify and reactivate a network connection from the
terminal.
5. Verification and Documentation
I learned the importance of checking command output, saving screenshots,
and documenting both configuration steps and test results.
6. Recovery Planning
A clean VM snapshot can provide a baseline to return to before carrying
out later experiments.
📸 Screenshots
Place your genuine project screenshots in a folder named screenshots
and use descriptive filenames:
  Filename                                     Description
  01-kali-linux-desktop.png                  Kali Linux desktop

  02-ipv4-network-configuration.png          Manual IPv4 settings

  03-network-connection-reactivated.png      Terminal showing connection
activation

  04-connectivity-and-dns-verification.png   Connectivity and DNS test results
--- capture this after testing
05-virtualbox-vm-snapshot.png              VirtualBox snapshot --- capture
this if created
The browser/news screenshot is not recommended as evidence for this
particular lab because it does not show network configuration or
verification.
🔐 Security and Ethical Use
This project is intended for educational purposes and authorized
testing. Always obtain permission before testing any system or network.
Keep future penetration-testing exercises inside your own lab or another
explicitly authorized environment.
🔗 Tools and Resources
- VirtualBox Downloads
- Kali Linux --- Get Kali
- Kali Linux Documentation
👤 Author
K. Pranay Kumar
Cybersecurity Program --- Networkwalks
Batch: B084
Project: Week 01 --- Cybersecurity & Pentesting Lab Setup
📌 Project Information
- Program: Cybersecurity at Networkwalks
- Batch: B084
- Week: 01
- Project: Cybersecurity & Pentesting Lab Setup
- Primary tools: VirtualBox and Kali Linux
