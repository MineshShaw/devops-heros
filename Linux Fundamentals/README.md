## 1. Soft Links vs. Hard Links

* Hard Link: Creates a direct pointer to the underlying inode on the disk. Both the original file and the hard link share the exact same inode number and file data. Deleting the original file does not remove the data as long as at least one hard link remains.

    * Limitations: Cannot span across different file systems/partitions and cannot link directories (to prevent recursive loops).

* Soft Link (Symlink): Creates a separate shortcut file that points to the path of the original file (stores the target filename as text). It has a unique inode. If the original file is deleted or moved, the soft link becomes a "broken link."

    * Advantages: Can span across file systems and can link directories.

### Commands:
```bash
Hard Link: ln <target> <link_name>

Soft Link: ln -s <target> <link_name>
```

### Commands & Expected Outputs

```bash
# Create a test file
echo "Hello Linux" > original.txt

# Create a Hard Link
ln original.txt hard_link.txt

# Create a Soft Link
ln -s original.txt soft_link.txt

# Verify inodes and link counts
ls -li original.txt hard_link.txt soft_link.txt
```

#### Expected Output

```bash
11821949021872006 -rwxrwxrwx 2 scaler-190nml3 scaler-190nml3 12 Aug 31 16:57 hard_link.txt
11821949021872006 -rwxrwxrwx 2 scaler-190nml3 scaler-190nml3 12 Aug 31 16:57 original.txt
2814749767131731 lrwxrwxrwx 1 scaler-190nml3 scaler-190nml3 12 Aug 31 16:57 soft_link.txt -> original.txt
```

---

## 2. adduser vs. useradd

* **useradd:** A low-level, native system utility used to add users. It does not automatically create a home directory, set up a user group, or prompt for a password unless specific flags are passed (e.g., ```useradd -m -s /bin/bash username```).

* **adduser:** A high-level, user-friendly Perl script wrapper around ```useradd``` (commonly found on Debian and Ubuntu systems). It interactively prompts for a password, automatically creates a home directory with default configuration files from ```/etc/skel```, and sets up a dedicated user group.

* **Recommendation:** ```adduser``` is the preferred choice on Ubuntu/Debian for administrators due to its automation and ease of use.

### Commands & Expected Outputs

```bash
# Create a test user using the recommended adduser command
sudo adduser testuser
```

#### Expected Output

```bash
[sudo: authenticate] Password:
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for testuser
Enter the new value, or press ENTER for the default
        Full Name []: testuser
        Room Number []: 123
        Work Phone []: 123
        Home Phone []: 123
        Other []: 123
Is the information correct? [Y/n] Y
```

---

## 3. journalctl

* ```journalctl``` is the query and display tool for ```systemd-journald```, the service responsible for collecting and storing logging data in binary format on modern Linux distributions. It aggregates kernel logs, system daemon outputs, and standard error/output from services managed by systemd.

* Key Usage:

    * View all logs: ```journalctl```
    * View logs for a specific service (e.g., SSH): ```journalctl -u ssh```
    * Follow logs in real-time: ```journalctl -f```
    * View logs since boot: ```journalctl -b```

### Commands & Expected Output
```bash
# View recent system logs
journalctl -n 20

# View real-time logs for the ssh service
sudo journalctl -u ssh -f
```

#### Expected Output:
```bash
Aug 31 17:01:01 scaler-190NML3 adduser[1204]: Adding new user `testuser' to supplemental / extra groups `users' ...
Aug 31 17:01:01 scaler-190NML3 adduser[1204]: Adding user `testuser' to group `users' ...
Aug 31 17:01:01 scaler-190NML3 gpasswd[1263]: members of group users set by root to scaler-190nml3,testuser
Aug 31 17:01:01 scaler-190NML3 sudo[1200]: pam_unix(sudo:session): session closed for user root
Aug 31 17:01:04 scaler-190NML3 wsl-pro-service[1227]: WARNING Daemon: could not connect to Windows Agent: could not get address: could not read age>
Aug 31 17:01:36 scaler-190NML3 wsl-pro-service[1227]: WARNING Daemon: could not connect to Windows Agent: could not get address: could not read age>
Aug 31 17:01:39 scaler-190NML3 sudo[1288]: pam_unix(sudo:session): session opened for user root(uid=0) by (uid=1000)
Aug 31 17:01:39 scaler-190NML3 sudo[1288]: scaler-190nml3 : TTY=/dev/pts/2 ; PWD=/mnt/c/Users/mines/OneDrive/Desktop/linux_fundamentals ; USER=root>
Aug 31 17:01:39 scaler-190NML3 deluser[1291]: Looking for files to backup/remove ...
Aug 31 17:01:39 scaler-190NML3 deluser[1291]: Removing files ...
Aug 31 17:01:39 scaler-190NML3 deluser[1291]: Removing crontab ...
Aug 31 17:01:39 scaler-190NML3 crontab[1295]: (root) LIST (testuser)
Aug 31 17:01:39 scaler-190NML3 deluser[1291]: Removing user `testuser' ...
Aug 31 17:01:39 scaler-190NML3 userdel[1297]: delete user 'testuser'
Aug 31 17:01:39 scaler-190NML3 userdel[1297]: delete 'testuser' from group 'users'
Aug 31 17:01:39 scaler-190NML3 userdel[1297]: removed group 'testuser' owned by 'testuser'
Aug 31 17:01:39 scaler-190NML3 userdel[1297]: removed shadow group 'testuser' owned by 'testuser'
Aug 31 17:01:39 scaler-190NML3 userdel[1297]: delete 'testuser' from shadow group 'users'
Aug 31 17:01:39 scaler-190NML3 sudo[1288]: pam_unix(sudo:session): session closed for user root
Aug 31 17:02:36 scaler-190NML3 wsl-pro-service[1227]: WARNING Daemon: could not connect to Windows Agent: could not get address: could not read age>
lines 1-20/20 (END)
```

---

## 4. Linux Command Cheat Sheet

| Command          | Description                                           |
| ---------------- | ----------------------------------------------------- |
| ls               |List directory contents.                               |  
|cd                |Change directory.                                      |  
|pwd               | Print working directory.                              |  
|mkdir             |Make new directory.                                    |
|rm                |Remove files/directories.                              |
|touch             | Create a new file.                                    |
|cp                | Copy files.                                           |
|mv                | Move or rename files.                                 |
|cat               | View file content.                                    |
|less / more       | View large files.                                     |
|tail              | View end of file.                                     |
|head              | View top of file.                                     |
|grep              | Search inside files.                                  |
|ps                | Show processes.                                       |
|top / htop        | System resource usage.                                |
|kill              | Kill process by PID.                                  |
|systemctl status  | Check service status.                                 |
|systemctl restart | Restart a service.                                    |
|ping              | Check connectivity.                                   |
|ip a / ifconfig   | Show IP/network config.                               |
|netstat           | Show network connections.                             |
|curl              | Fetch URL data.                                       |
|wget              | Download file.                                        |
|chmod             | Change file permissions.                              |
|chown             | Change file owner.                                    |
|apt               | Install packages (Ubuntu/Debian).                     |
|yum               | Install packages (RHEL/CentOS).                       |
|df                | Show disk usage.                                      |
|du                | Show file/folder size.                                |
| crontab -e       | Edit cron jobs.                                       |
|nohup             | Run command in background.                            | 
|adduser           | Add a new user.                                       |
|useradd           | Create user (non-interactive).                        |
|usermod           | Modify user account.                                  |
|passwd            | Change user password.                                 |
|id                | Display UID, GID, and groups.                         |
|groups            | Show groups user belongs to.                          |
|deluser / userdel | Delete a user.                                        |
|who               | List logged-in users.                                 |
|w                 | Show who is logged in and what they are doing.        |
|last              | Show login history.                                   |
|uname -a          | Kernel & system info.                                 |
|hostname          | Show system hostname.                                 |
|uptime            | Show system uptime.                                   |
|whoami            | Current logged-in username.                           |
|history           | Show command history.                                 |
|date              | Current system date/time.                             |
|clear             | Clear terminal screen.                                |
|!! / !n           | Run last command again / Run nth command from history.|
|Ctrl+C            | Cancel running command.                               |
|Ctrl+L            | Clear terminal screen.                                |  

### Commands & Expected Output
```bash
# File, Directory, and Info Operations
pwd
mkdir -p /tmp/devops_practice
touch /tmp/devops_practice/index.html
ls -l /tmp/devops_practice
whoami
date
```

#### Expected Output:
```bash
/mnt/c/Users/mines/OneDrive/Desktop/linux_fundamentals
total 0
-rw-r--r-- 1 scaler-190nml3 scaler-190nml3 0 Aug 31 17:05 index.html
scaler-190nml3
Mon Aug 31 17:05:03 UTC 2026
```