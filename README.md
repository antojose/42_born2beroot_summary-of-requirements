# Summary of Requirements - Born2beRoot @42

Outline:
- [BASICS]()
- [CHOICE OF OS]()
- [CHOICE OF SERVICES]()
- [HOSTNAME]()
- [PARTITIONING]()
- [USERS]()
- [SUDO CONFIG]()
- [PASSWORD POLICY]()
- [SSH CONFIG]()
- [FIREWALL]()
- [MONITORING SCRIPT]()
- [README.md]()
- [SIGNATURE.TXT]()
- [BONUS]()
- [SNAPSHOTS]()
- [USEFUL COMMANDS]()
- [SUBMISSION]()
- [DEFENSE]()


## BASICS:
- Create the VM in VirtualBox.
- Submit README.md and a signature.txt containing the sha1sum of the VDI file.


## CHOICE OF OS:
- Debian STABLE (not TESTING/UNSTABLE), since new to system administration.

## CHOICE OF SERVICES:
- Install only a minimal set of services.
- Do not install any GUI (X.org / Wayland / ...).


## HOSTNAME:
- Initial hostname should be your login ending with 42 (e.g., wil42).
- You will have to modify this hostname during your peer review.

## PARTITIONING:
- Create at least 2 encrypted partitions using LVM. 
- Below is an example of a possible partitioning:  
```
# lsblk
NAME                               MAJ:MIN RM   SIZE RO TYPE   MOUNTPOINTS
sda                                  8:0    0     8G  0 disk   
├─sda1                               8:1    0   487M  0 part   /boot
├─sda2                               8:2    0     1K  0 part   
└─sda5                               8:5    0   7.5G  0 part   
  └─sda5_crypt                     254:0    0   7.5G  0 crypt  
		├─wil--vg-root                 254:1    0   2.8G  0 lvm    /
		├─wil--vg-swap_1               254:2    0   976M  0 lvm    [SWAP]
		└─wil--vg-home                 254:3    0   3.8G  0 lvm    /home
	sr0                                 11:0    1  1024M  0 rom    
```
- BONUS (matters only if everything in the mandatory part is perfect):  
Set up the partitions correctly so that you obtain a structure similar to the one below:
```
# lsblk
NAME                               MAJ:MIN RM   SIZE RO TYPE   MOUNTPOINTS
sda                                  8:0    0  30.8G  0 disk   
├─sda1                               8:1    0   500M  0 part   /boot
├─sda2                               8:2    0     1K  0 part   
└─sda5                               8:5    0  30.3G  0 part   
  └─sda5_crypt                     254:0    0  30.3G  0 crypt  
	├─LVMGroup-root                254:1    0    10G  0 lvm    /
	├─LVMGroup-swap                254:2    0   2.3G  0 lvm    [SWAP]
	├─LVMGroup-home                254:3    0     5G  0 lvm    /home
	├─LVMGroup-var                 254:4    0     3G  0 lvm    /var
	├─LVMGroup-srv                 254:5    0     3G  0 lvm    /srv
	├─LVMGroup-tmp                 254:6    0     3G  0 lvm    /tmp
	└─LVMGroup-var--log            254:7    0     4G  0 lvm    /var/log
sr0                                 11:0    1  1024M  0 rom    
```
The examples show arbitrary disk sizes. You need to determine the appropriate size for each partition to ensure proper operation while avoiding unnecessary disk usage.

## USERS:
- In addition to root, add a user with your intraname as the username.
- Add that user to both user42 and sudo groups.
- During your peer review, you will have to create a new user and assign it to a group.


## SUDO CONFIG:
- Max 3 attempts in the event of an incorrect password when using sudo.
- Show custom message when an incorrect password is entered when using sudo.
- Log (save in /var/log/sudo/ folder) the input and output of each action performed with sudo.
- Limit paths that can be accessed with sudo:  
  `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin`
- Enable TTY mode.


## PASSWORD POLICY:
For root and non-root users:
1) Auto-expire password every 30 days.
Send user a warning message 7 days before their password expires.
2) Minimum number of days between password changes: 2
3) Choice of password:
At least 10 characters long.
Must contain an uppercase letter, a lowercase letter, and a number.
Must not contain more than 3 consecutive identical characters.
Must not include the name of the user.

For non-root users only:
The password must contain at least 7 characters that were not part of the previous password.

After setting up your configuration files, you will have to change all the passwords of the accounts present on the virtual machine, including the root account.


### SSH CONFIG:
Change SSH port to 4242.
Disable SSHing in as root.


### FIREWALL:
Configure ufw firewall.
Launch firewall at VM launch.
Leave only port 4242 open in the VM.


### MONITORING SCRIPT:
Create a bash script for monitoring, 
called monitoring.sh, starting at startup, and 
displaying every 10 minutes (take a look at wall).
The banner is optional. 
No errors should be displayed.
  Displays the following information:
	OS Architecture
	OS Kernel Version
	Number of physical processors.
	Number of virtual processors.
	Available RAM and Utilization Rate (percentage).
	Available Storage and Utilization Rate (percentage).
	CPU utilization rate (percentage).
	Last Reboot: Date and time.
	LVM Status (active/not).
	No. of active connections.
	No. of users using the server.
	The server's IP Address and its MAC address.
	No. of commands executed with sudo.

	Example of how the script is expected to work:
	Broadcast message from root@wil (tty1) (Sun Apr 25 15:45:00 2021):
		#Architecture: Linux wil 4.19.0-16-amd64 #1 SMP Debian 4.19.181-1 (2021-03-19) x86_64 GNU/Linux
		#Physical CPU: 1
		#vCPU: 1
		#Memory Usage: 74/987MB (7.50%)
		#Disk Usage: 1009/2Gb (49%)
		#CPU load: 6.7%
		#Last boot: 2021-04-25 14:45
		#LVM use: yes
		#TCP Connections: 1 ESTABLISHED
		#User log: 1
		#Network: IP 10.0.2.15 (08:00:27:51:9b:a5)
		#Sudo: 42 cmd

Take a look at cron.

You will also have to interrupt it without modifying it.


### README.md:
The very first line must be _italicized_, and read: 
This project has been created as part of the 42 curriculum by <login>.

A "Description" section that clearly presents the project, including its goal and a brief overview.

An "Instructions" section containing any relevant information about compilation, installation, and/or execution.

A "Resources" section listing 
  classic references related to the topic (documentation, articles, tutorials, etc.), 
  as well as a description of how AI was used specifying for which tasks and which parts of the project.

A "Project Description" section
  explaining your choice of operating system (Debian or Rocky), 
    including their respective advantages and disadvantages. 
  Must describe the main design choices made during the setup:
    partitioning,
	security policies,
	user management,
	services installed
  and provide a Comparison between:
    Debian vs Rocky Linux
    AppArmor vs SELinux
    UFW vs firewalld
    VirtualBox vs UTM


### SIGNATURE.TXT
signature.txt file contains only the sha1sum of the VDI file.
  Command: sha1sum filename.vdi
  Example output:
    6e657c4619944be17df3c31faa030c25e43e40a

Please note that your VM's signature will be altered as soon as you start the virtual machine again.
To allow signature verification for all your evaluations, you can either duplicate the virtual machine disk file, or use a snapshots for each evaluation.


### BONUS:
Bonus part will be assessed/evaluated ONLY IF 
  the mandatory part has been FULLY completed and works without any malfunctions.

Create the bonus partitions structure as detailed in the PARTITIONING section above.

Set up a functional WordPress website with the following services: lighttpd, MariaDB, and PHP.

Set up a service of your choice that you think is useful (NGINX / Apache2 excluded!).

Update the firewall (ufw/firewalld) rules to open necessary ports for the extra services installed as part of the BONUS requirements.


### SNAPSHOTS:
The use of snapshots is restricted.
No snapshots may exist at the beginning of each evaluation.
A snapshot dedicated to the defense will then be created and deleted at the end of the defense.
You are encouraged to make tests with the snapshot features before submitting your project.


### USEFUL COMMANDS TO CHECK SOME REQUIREMENTS:
  head -n 2 /etc/os-release
  /usr/sbin/aa-status
  ss -tunlp
  /usr/sbin/ufw status



### SUBMISSION:
Submit only the following files at the root of the git repo:
  the README.md file, and 
  the signature.txt.
Do not add your VM to your Git repository.


### DEFENSE:
Understand what you use.

If sha1sum of your VM's current VDI file is not the same as the one in signature.txt, you will be graded 0.

You will be asked:
  A few questions about the OS you chose:
    For example, 
      the differences between aptitude and apt,
      or what SELinux or AppArmor is. 
  How the monitoring script works.
  To Justify the choice of the extra service you added as part of BONUS, if any.
  To Modify your hostname.
  To Create a new user and add to a group.
  To Demonstrate the use of SSH by setting up a new account.



```
