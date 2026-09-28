Kali Linux Operating System Hardening & Administration Lab
Executive Summary
This lab demonstrates fundamental Linux system administration, service auditing, and host-based security hardening techniques performed on a Kali Linux environment running within Oracle VirtualBox. The primary goal was to inspect active system services, configure administrative access according to privilege control policies, enforce packet filtering via the Uncomplicated Firewall (UFW), and minimize the attack surface by disabling unnecessary background daemons.

Lab Environment & Information
Operating System: Kali Linux (Rolling Release)

Kernel Version: 5.7.0-kali1-amd64

Virtualization Platform: Oracle VirtualBox

Primary User: praise-jah

Technical Execution & Workflow
1. Network Sockets & Port Auditing
To assess potential exposure points on the system, active listening sockets were inspected using socket statistics:

Bash


ss -tulpn
ip a
Result: Confirmed active TCP/UDP ports and bound interfaces (such as 127.0.0.1:36061 listening locally).

2. User & Access Control Management
Enforced privilege separation by creating a restricted standard account and assigning elevated operational privileges through group membership:

Bash


# Create standard user account
sudo adduser student

# Add user to the privilege escalation group
sudo usermod -aG sudo student

# Verify group membership
groups student
Result: User account student was created with home directory allocation and successfully added to the sudo group.

3. Package Index Refresh
Synchronized local package lists with remote Kali Linux security repositories:

Bash


sudo apt update
Result: Handled repository index updates and audited available package upgrades.

4. Host-Based Firewall Enforcement (UFW)
Configured host security rules using the Uncomplicated Firewall (UFW) to enforce strict network access controls:

Bash


# Enable the host firewall service on boot
sudo ufw enable

# Audit firewall status and policy rules
sudo ufw status verbose
Result: Firewall turned active and enabled on system startup. Verified incoming and outgoing default policy rules along with specific port restrictions.

5. Service Auditing & Attack Surface Reduction
Audited running system services and container daemons to ensure unnecessary background processes are terminated:

Bash


# Audit Docker container service status
systemctl status docker

# Stop active socket triggers and disable auto-start on boot
sudo systemctl stop docker.socket
sudo systemctl disable docker

# Verify daemon status
systemctl status docker.socket
Result: Stopped active Docker triggers (docker.socket) and disabled the container service from launching automatically during boot cycles, reducing background system overhead and exposure.

6. File Integrity & Verification Check
Verified local storage operations by creating and checking a sample text artifact:

Bash


# Create sample file
nano sample.txt

# Inspect file metadata and content encoding
ls -l sample.txt
file sample.txt
cat sample.txt
Result: Successfully created sample.txt and verified its file type as ASCII text.

Lab Demonstration Video

https://github.com/user-attachments/assets/7be32436-b917-4d1e-8eb0-b1e041093db5



Security Takeaways
Minimization of Attack Surface: Disabling unneeded container sockets (docker.socket) prevents unauthorized background process invocation and saves system resources.

Network Defense-in-Depth: Implementing host firewalling with UFW provides explicit filtering against unauthorized inbound socket connections.

Privilege Control: Admin access was structured using targeted sudo permissions instead of operating directly from the root account.
