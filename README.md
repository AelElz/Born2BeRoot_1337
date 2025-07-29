# Born2beroot

Born2beroot is a system administration and security project from 42 school. The goal is to set up and harden a Debian-based virtual machine by applying strict security rules and best practices. You’ll configure user accounts, manage permissions, secure SSH access, enforce strong password policies, configure a firewall, and automate system monitoring with cron jobs. The project teaches you how to build a more secure and maintainable Linux server environment — essential skills for any system administrator or DevOps engineer.

---

## 🛠️ Project Goals

- Install and configure a minimal Debian system
- Harden the system using user, group, and password policies
- Configure and secure SSH access
- Set up and manage a firewall with UFW
- Automate monitoring via cron jobs
- Secure sudo access with custom rules and logging

---

## 📦 Installation

### Step 1: OS Installation

1. Download the [Debian 10 Netinst ISO](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/).
2. Create a new Virtual Machine using VirtualBox.
3. Use the ISO to install Debian with default settings.
4. Set hostname as `your_login42`.

---

## ⚙️ Configuration Steps

### Configuration

```bash
su -
apt-get update && apt-get upgrade -y
apt install sudo -y
usermod -aG sudo your_username

- Edit /etc/sudoers: your_username ALL=(ALL) ALL

- Essential Tools: sudo apt-get install git wget vim -y

- SSH Setup:
    sudo apt install openssh-server
    sudo nano /etc/ssh/sshd_config
    # Don't forget to Change Port 22 to 4242 #
    sudo service ssh restart
- UFW (Firewall):
    sudo apt install ufw
    sudo ufw allow ssh
    sudo ufw allow 4242
    sudo ufw enable

- Password Policies:
    sudo apt-get install libpam-pwquality

- Edit /etc/pam.d/common-password:
    password requisite pam_pwquality.so retry=3 minlen=10 lcredit=-1 ucredit=-1 dcredit=-1 maxrepeat=3 usercheck=0 difok=7 enforce_for_root

- Edit /etc/login.defs:
    PASS_MAX_DAYS 30
    PASS_MIN_DAYS 2
    PASS_WARN_AGE 7

- Users & Groups:
    sudo groupadd user42
    sudo groupadd evaluating
    sudo adduser new_username
    sudo usermod -aG user42 your_username
    sudo usermod -aG evaluating new_username

- Sudoers Restrictions:
    Defaults        passwd_tries=3
    Defaults        badpass_message="Password is wrong, please try again!"
    Defaults        logfile="/var/log/sudo/sudo.log"
    Defaults        log_input,log_output
    Defaults        requiretty
    Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

- Hostname:
    hostnamectl set-hostname new_hostname
    sudo nano /etc/hosts
    # Replace old hostname with new_hostname #
    sudo reboot

- Monitoring Script:
+ Place this script in /usr/local/bin/monitoring.sh and make it executable:
  sudo chmod +x /usr/local/bin/monitoring.sh

+ Allow sudo to run it without password:
    sudo visudo
    # Add:
    your_username ALL=(ALL) NOPASSWD: /usr/local/bin/monitoring.sh
+ Add a cron job to run every 10 minutes:
    sudo crontab -u root -e
    # Add:
    */10 * * * * /usr/local/bin/monitoring.sh

