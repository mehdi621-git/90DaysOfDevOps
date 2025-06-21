# 🚀 Project: DevOps Linux Server Monitoring & Automation

Imagine you're managing a **Linux-based production server** and need to ensure that **users, logs, and processes** are well-managed. This project showcases real-world DevOps tasks including **log analysis**, **volume management**, and **automation**, helping to boost core Linux and DevOps skills.

---

## 📌 Tasks & Solutions

---

### 🔐 1️⃣ User & Group Management

**Learned Concepts:**

* Linux users, groups, and permissions (`/etc/passwd`, `/etc/group`)
* SSH access restrictions
* Granting sudo privileges

#### ✅ Commands:

```bash
# Create a user with Bash shell and home directory
sudo useradd -s /bin/bash -m devops_user

# Set password
sudo passwd devops_user

# Create a group
sudo groupadd devops_team

# Add user to the group
sudo usermod -a -G devops_team devops_user

# Grant sudo access to the user
sudo usermod -a -G sudo devops_user
```

#### 🔐 SSH Login Restriction:

```bash
# Edit sshd_config to restrict SSH access
cd /etc/ssh
sudo chmod 666 sshd_config
vim sshd_config
# Add the following line to allow only specific users
AllowUsers devops_user

# Restart SSH service
sudo systemctl restart ssh
```

#### 🔑 Setup SSH Key-Based Login:

```bash
# On client machine
ssh-keygen

# Add public key to the User "devops_user" /home/devops_user/.ssh/authorized_keys 
```

---

### 📁 2️⃣ File & Directory Permissions

**Task:**

* Create a directory `/devops_workspace` and file `project_notes.txt`
* Set permissions: Owner can edit, group can read, others no access

#### ✅ Commands:

```bash
# Create directory and file
mkdir ~/devops_workspace
cd ~/devops_workspace
touch project_notes.txt


# Set permissions: rw for owner, r for group, none for others
chmod 640 project_notes.txt

# Verify
ls -l project_notes.txt
```

---

### 📊 3️⃣ Log File Analysis with AWK, Grep & Sed

**Log Source:**
[LogHub Linux Logs (Linux\_2k.log)](https://github.com/logpai/loghub/blob/master/Linux/Linux_2k.log)

#### ✅ Commands:

```bash
# Extract authentication failures
grep -i "authentication_failure" Linux_2k.log > grepUsed

# Extract timestamps and log levels using AWK
awk '{print $3, $6, $7}' Linux_2k.log

# Replace IPs with [REDACTED] using SED
grep -i "connection from" Linux_2k.log | sed -r 's/(\b[0-9]{1,3}\.){3}[0-9]{1,3}\b/REDACTED/' > sedDone
```

#### 🔍 Bonus: Most Frequent Log Entries

```bash
cat Linux_2k.log | sort | uniq -c | sort -nr | head -10
```

---

### 📍 4️⃣ Volume Management & Disk Usage

**Task:**

* Mount a volume (or loop device)
* Verify disk usage

#### ✅ Commands:

```bash
#create new Vokumn from EBS and attack to the instance i-e /dev/xvdf
# Create mount point
sudo su
mkdir /mnt/devops

# Mount a loop device
mkfs -t ext4 /dev/xvdf
mount /dev/xvdf /mnt/devops

# Verify mount and usage
df -h
```

---

### 🧠 5️⃣ Process Management & Monitoring

**Task:**

* Run and monitor a background process
* Use tools like `ps`, `top`, and `htop`

#### ✅ Commands:

```bash
# Start background process
ping google.com > ping_test.log &

# Check processes
ps -A           # View all processes
top             # Basic Text based real-time process viewer
```

#### Install & Use htop:

```bash
# Install htop (if not installed)
sudo apt-get install htop

# Run htop
htop  # A color and imteractive for real time process viewer
```
---

### 🗂️ 6️⃣ Automate Backups with Shell Scripting

**Task:**

* Create backup script for `/devops_workspace`
* Save as `backup_YYYY-MM-DD.tar.gz` in `/backups`
* Schedule it with cron
* Show success message in green

#### ✅ Shell Script: `backup.sh`

```bash
#!/bin/bash

GREEN='\033[32;3m'

if [ -d "/home/ubuntu/Day1/devops_workspace/Backups" ];
then
        echo "Directory exists"
else
        echo -e "${GREEN}Creating Backup directory"
mkdir /home/ubuntu/Day1/devops_workspace/Backups

fi

timestamp=$(date +%F-%H-%M)

echo "Creating Backups"
des="/home/ubuntu/Day1/devops_workspace/Backups/backup-$timestamp.tar.gz"
src='/home/ubuntu/Day1/devops_workspace'
zip $des $src
if [ $? == 0 ];then
        echo "SuccessFully Backup"
else
        echo "Error while backing up"
fi
```

#### ✅ Make Script Executable:

```bash
chmod 770 backup.sh
```

#### ✅ Setup Cron Job:

```bash
crontab -e
```

Add this line to run the script every 2 minutes:

```cron
*/2 * * * * /path/to/backup.sh
```
---

### 📌 Author

**Muhammad Mehdi Hassan**
*DevOps & Full-Stack Developer*
