# Linux-Part6
Linux Networking &amp; Logging Cheat Sheet – Quick reference for managing network interfaces, SSH, firewalls, and system logs using nmcli, ip, ssh, rsyslog, and journalctl. Ideal for sysadmins, developers, and security tasks.
# 🐧 Linux Networking & Logging Cheat Sheet

## 🌐 Network Interfaces & Status


ip link show        # 🔹 Show all network interfaces with status (UP/DOWN) and MAC addresses
ifconfig            # 🔹 Show IP, MAC, RX/TX bytes for each interface [deprecated but useful]
ip address show     # 🔹 Display all IP addresses assigned to interfaces
ip -s link show <interface>  # 🔹 Show RX/TX stats, errors, dropped packets
ip -br a s          # 🔹 Briefly display interfaces and IP addresses
nmcli device status # 🔹 Show all network devices and connection status
nmcli connection show  # 🔹 List all saved network connections
nmcli device wifi list  # 🔹 Display nearby Wi-Fi networks
nmcli device wifi connect "SSID" password "PASSWORD"  # 🔹 Connect to Wi-Fi
nmcli device connect eth0   # 🔹 Enable a wired network interface
nmcli device disconnect eth0 # 🔹 Disable a wired network interface
nmcli radio wifi on          # 🔹 Turn Wi-Fi on
nmcli radio wifi off         # 🔹 Turn Wi-Fi off
nmcli connection modify "Wired connection 1" ipv4.method auto
nmcli connection up "Wired connection 1" 

# 🔹 Automatically get IP from DHCP

nmcli connection modify "Wired connection 1" ipv4.addresses 192.168.1.100/24
nmcli connection modify "Wired connection 1" ipv4.gateway 192.168.1.1
nmcli connection modify "Wired connection 1" ipv4.dns "8.8.8.8 8.8.4.4"
nmcli connection modify "Wired connection 1" ipv4.method manual
nmcli connection up "Wired connection 1"

# 🔹 Set static IP, gateway, and DNS manually
ip route            # 🔹 Show routing table with default gateway
cat /etc/resolv.conf  # 🔹 Display current DNS servers
nmcli networking off
nmcli networking on   # 🔹 Restart NetworkManager

hostname            # 🔹 Show current hostname
hostnamectl         # 🔹 Show detailed hostname and system info
hostnamectl set-hostname <new-host>  # 🔹 Change hostname permanently

ping 193.33.33.34   # 🔹 Check if server/IP is reachable
ssh user@host       # 🔹 Connect via SSH
cd ~/.ssh/known_hosts  # 🔹 View stored fingerprints
ssh-keygen              # 🔹 Generate default SSH key pair (RSA)
ssh-keygen -t rsa       # 🔹 Generate RSA key pair
ssh-keygen -t ed25519   # 🔹 Generate ED25519 key pair (modern & secure)
ssh-copy-id user@server # 🔹 Copy public key to server for passwordless login
ssh user@server         # 🔹 Log in without password
sudo nano /etc/ssh/sshd_config  # 🔹 Edit SSH server config
PermitRootLogin yes|no   # 🔹 Allow or deny root login
Port 2222                # 🔹 Change SSH port
sudo systemctl restart ssh  # 🔹 Restart SSH service
sudo ufw allow 2222/tcp  # 🔹 Open new SSH port in firewall
ssh -p 2222 user@IP      # 🔹 Connect to SSH on custom port

sudo vim /etc/rsyslog.conf       # 🔹 Configure syslog: TCP/UDP, priority, facility
logger -p local1.info "check CPU"  # 🔹 Send a test message
man logger                       # 🔹 Manual & options for logger
ls /etc/rsyslog.d/               # 🔹 List additional rsyslog config files
cd /var/log                       # 🔹 Navigate to log directory
tail -f /var/log/auth.log         # 🔹 View authentication logs live
tail -F /var/log/auth.log         # 🔹 Follow auth.log even after rotation

journalctl                 # 🔹 View system logs collected by journald
journalctl -n 5            # 🔹 Show last 5 log entries
journalctl -f              # 🔹 Follow logs in real-time
journalctl -u ssh.service  # 🔹 Show logs for SSH service
journalctl --since yesterday
journalctl --since "-1hour"  # 🔹 Filter logs since yesterday or last hour
sudo vim /etc/systemd/journald.conf  # 🔹 Configure storage: volatile/persistent/none
systemctl restart systemd-journald   # 🔹 Restart journald service
journalctl --flush         # 🔹 Flush journal logs to disk
sudo visudo

Defaults !fqdn   # 🔹 Disable FQDN lookup to speed up sudo
