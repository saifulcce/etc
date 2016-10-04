# Fix Windows entry in grub2 
# https://www.centos.org/forums/viewtopic.php?f=47&t=47476
# Add to /etc/grub2.cfg:
menuentry 'Windows 8 (loader)' {
        insmod part_msdos
        insmod ntfs
        set root='hd0,msrdos1'
        ntldr /bootmgr
}

# Install grub2 after Windows Bootloader replaces it
https://fedoraproject.org/wiki/GRUB_2#Encountering_the_dreaded_GRUB_2_boot_prompt
Examples:
> set root=(lvm,fedora-root)
> linux (hd0,msdos3)/vmlinuz-<version> root=/dev/mapper/fedora-root rhgb quiet selinux=0
> initrd (hd0,msdos3)/initramfs-<version>
> boot

# htop
F4  # Filter
F6  # Sort by
I   # Inverse sorting

# Check open ports
sudo netstat -lptu

# Ping with port
# http://superuser.com/questions/769541/is-it-possible-to-ping-an-addressport
nmap -p 80 onofri.org
telnet 192.168.153.1 1433

# Check which user start a process
pgrep <process-name>
ps -p <pid> -o ruser=

# Look at which process using certain port
netstat -nlp | grep <port-num>

# Check if port on remote server is open
# http://superuser.com/questions/621870/test-if-a-port-on-a-remote-system-is-reachable-without-telnet
nc 127.0.0.1 123 < /dev/null; echo $?  # 0 is open

# Start openssh-daemon (for ssh service)
sudo /etc/init.d/sshd start

# Autostart sshd
chkconfig sshd on

ssh-keygen -t rsa -b 4096 -C "david.heryanto@hotmail.com"
ssh-copy-id -i ~/.ssh/id_rsa.pub remote-host

# Auto start ssh-agent and ssh-add
# http://unix.stackexchange.com/questions/90853/how-can-i-run-ssh-add-automatically-without-password-prompt
vim ~/.bashrc  # Somehow ~/.bash_profile doesn't work
if [ -z "$SSH_AUTH_SOCK" ] ; then
  eval `ssh-agent -s`
fi
ssh-add &> /dev/null
ssh-add ~/.ssh/*.pem &> /dev/null
# Must add this to the top of ~/.bashrc, otherwise scp may work incorrectly (bug in scp)
# https://www.fir3net.com/UNIX/General/why-does-scp-file-transfer-fail-but-there-is-no-error.html
# If not running interactively, don't do anything
[[ $- == *i* ]] || return

# Check which key is loaded by ssh-agent
ssh-add -l

# Edit hostname mapping
sudo vim /etc/hosts

# Firewall
sudo /etc/init.d/iptables save
sudo /etc/init.d/iptables stop

# firewalld - new software to manage firewall in newer distro
# https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7
# http://blog.jyore.com/?p=113
firewall-cmd --list-all  # Get firewall config
firewall-cmd --get-active-zones
sudo firewall-cmd --zone=public --add-port=2434/tcp
sudo firewall-cmd --zone=public --remove-port=2434/tcp

# Dnf install without update metadata, i.e. use cached metadata
sudo dnf -C install <package-name>

# Firewall configuration in Fedora, open port
sudo dnf -y install firewall-config
sudo firewall-config  # Rember to change the Configuration to Permanent
# Alternatively
firewall-cmd --get-active-zones  # Then change the zone below accordingly
# List all rules
sudo firewall-cmd --list-all
sudo firewall-cmd --zone=FedoraWorkstation --add-port=8888/tcp --permanent
sudo firewall-cmd --reload

# Remove with --remove-port

# Change hostname in CentOS
sudo hostnamectl set-hostname <new-hostname>  # modifies /etc/hostname

# Restart network
/etc/init.d/network restart

# Setup static ip. May need to disable NetworkManager.service
sudo vim /etc/sysconfig/network-scripts/ifcfg-<interface-name>
# Modify/Add these lines
DEVICE=<interface-name>
BOOTPROTO=static
IPADDR=192.168.8.248
NETMASK=255.255.255.0
BROADCAST=192.168.8.255

# Change dns server
sudo vim /etc/resolv.conf
  nameserver 4.2.1.1
  nameserver 4.5.1.1

# Monitor network speed
sudo dnf -y install nethogs
sudo nethogs <interface>  # Use 'ip addr' to check

# mkdir: Create directory baz, with parents if not exists
mkdir -p foo/bar/baz

# mpi
mpirun -np 3 --host a,b,c a.out
# mpi installed via module
https://ask.fedoraproject.org/en/question/32864/mpirun-command-not-found/

# Running 32 bit app on linux 64 bit
sudo dnf install glibc.i686

# Move /home to new partition
http://embraceubuntu.com/2006/01/29/move-home-to-its-own-partition/

# Move multiple files to a directory
mv -t DESTINATION file1 file2 file3

# Print middle of file
# http://serverfault.com/questions/133692/how-to-display-certain-lines-from-a-text-file-in-linux
sed -n '100,+20p'  filename  # Line 100-120

# Limit ls
ls -U | head -5  # -U skips the sorting to make ls faster

# ls with absolute / full path
ls -d $PWD/*

# ls sort by size
# http://www.cyberciti.biz/faq/linux-ls-command-sort-by-file-size/
ls --sort=size -l

# inotify tools
https://gist.github.com/mani95lisa/6740671

# Sync changed files
https://gist.github.com/senko/1154509
rsync -av project centos-2:~

# Extended grep, -i: ignore case
egrep 'pattern1|pattern2' <filename>  # OR
egrep 'pattern1.*pattern2' <filename>  # AND fixed order
egrep 'pattern1.*pattern2|pattern2.*pattern1' <filename>  # AND any order

# Find which files in current directory contains string
grep -inr 'search token' ./*

# Check dnf installation path
rpm -qa | grep -i <package-name>  # Find the package name first
rpm -ql <package-name>
dnf list installed
# Remove rpm
rpm -e <package name>

# Gnome: reduce / smaller title bar size
# http://unix.stackexchange.com/questions/257163/reduce-title-bar-height-in-gnome-3-gtk-3
vim /home/davidheryanto/.config/gtk-3.0/gtk.css  # Create if not exists
-----------------------------
.header-bar.default-decoration { padding-top : 1px; padding-bottom : 1px; }
.header-bar.default-decoration .button.titlebutton { padding-top : 1.5px; padding-bottom : 1.5px; }
/* No line below the title bar */
.ssd .titlebar { border-width: 0; box-shadow: none;}
-----------------------------

# Open file with default application
gnome-open <file>

# Open file browser
nautilus --browser ~/some/directory

# Nautilus to always show path: Open dconf-editor
Find org.gnome.nautilus.preferences
check "always_use_location_entry"

# Open file eg pdf 
gvfs-open doc.pdf

# Lock screen
gnome-screensaver-command -l

# For loop examples
for i in {1..10}
do
  echo $i
done
for i in 1 3 5;do echo $i;done

# Remove packages
sudo dnf remove <package name>

# Remove packages without dependencies, e.g php-sqlite2
rpm -qa | grep "php-sqlite2"
rpm -e --nodeps "php-sqlite2"

# Restart
sudo reboot

# Theme/Appeance settings, font - anti alias on but no hinting

# Batch rename using perl rename
# Download from: http://anonscm.debian.org/cgit/perl/perl.git/plain/debian/rename
# Syntax: s/FIND/REPLACE/FLAGS where s/ means '/' is the delimiter
rename 's/_full/_500/' *.jpg  # "a_full.jpg" to "a_500.jpg" current dir
find . -type f -iname "*.jpg" -exec rename 's/_full/_500/' {} \;  # Recursive
# Use 'rename -n' to preview the changes


# Kill process
kill -SIGKILL 1686
# Kill all process by name
pkill <process-name>
# http://stackoverflow.com/questions/6381229/how-to-kill-all-processes-matching-a-name
kill -9 $(pgrep amarok)

# pgrep with pattern -f and listing -l
pgrep -fl .*beav.*

# Find process by name
# Check which user runs the process
ps aux | grep <proceess_name>

# Find process by user
pgrep -u <username> | xargs ps -f -p

# List all users
cut -d: -f1 /etc/passwd

# Find location of app in terminal
which git

# Disable gnome 3 animation
gsettings set org.gnome.desktop.interface enable-animations false  # non-permanent
sudo dnf -y install dconf-editor
-> browse to org.gnome.desktop.interface and set enable-animations=false.

# Restart gnome-shell
Alt+F2 -> r 

# Fedora to install VMware Tools need to install kernel headers
sudo dnf install kernel-devel-`uname -r` kernel-headers-`uname -r`
sudo dnf -y install kernel-headers-$(uname -r)

# VMware patches
https://github.com/rasa/vmware-tools-patches

# When VMWare cause Ctrl key to not working
setxkbmap

# VMware cannot copy paste
1. Go into VM / Settings / Options / Guest Isolation
2. UNCHECK both checkboxes (Enable drag and drop, Enable copy and paste) and click OK.
3. Shut down the guest, and shut down VMware Workstation
4. Reboot the host computer
5. Run VMware Workstation but do not launch the guest yet.
6. Go into VM / Settings / Options / Guest Isolation for the guest, and CHECK both    checkboxes
7. Power On the guest.

# Get hardware and system info
cat /proc/meminfo
cat /proc/cpuinfo
uname -a or uname --help

# Distro
cat /etc/*-release

# Disable dnf repo
sudo dnf-config-manager --disable Dropbox

# Stop dnf from updating kernel (why? eg. cuz nvidia driver works only for specific kernel)
sudo vim /etc/dnf/dnf.conf
exclude=kernel*

# Search for packages in dnf repo
dnf search [search term]
# Search exact match for package name
dnf list <package>  # e.g. dnf list R, dnf list R-dev*

# Find packages location installed from dnf
rpm -ql [package-name]

# Install with dnf without updating i.e. disable dnf update
sudo vim /etc/dnf/dnf.conf
metadata_expire=1209600  # seconds in 2 weeks / 14d

# Remove password
passwd --delete <username>

# Add/Remove user
useradd <username>
userdel <username>
passwd <username>

# Multiprocessor sys monitor
# http://www.cyberciti.biz/tips/top-linux-monitoring-tools.html
mpstat -P ALL

# Put running process to background
# http://stackoverflow.com/questions/625409/how-do-i-put-an-already-running-process-under-nohup
Ctrl + Z; bg; disown -h [job-spec]  # Where %1 is the first running job, use 'jobs' to list jobs

# Run background process with sudo (use -b option)
# http://stackoverflow.com/questions/17475098/getting-sudo-and-nohup-to-work-together
sudo -b ./ascii_loader_script.pl 20070502 ctm_20070502.csv

# Download manager: Axel, usage: axel -avn 50 address (a:alt prog bar)
sudo dnf -y install http://pkgs.repoforge.org/axel/axel-2.4-1.el6.rf.x86_64.rpm

# wget background
http://stackoverflow.com/questions/21365251/how-to-run-wget-in-background-for-an-unattended-download-of-files

# wget batch file (content-disposition so wget uses original output file)
wget --content-disposition -i url-list.txt [--no-check-certificate]

# wget download all files with certain extension from url
# http://stackoverflow.com/questions/8755229/how-to-download-all-files-but-not-html-from-a-website-using-wget
wget -A pdf,jpg -m -p -E -k -K -np http://site/path/
wget -A pdf,jpg -m -p -E -k -K -np -nd http://site/path/  # No dir structure

# List all files with absolute path, starting with aa
find $PWD -maxdepth 1 -name aa\*

# Find ignore permission denied
find . -name 'file_to_found' 2>&1 | grep -v 'Permission denied'
# Find case insensitive
find -iname <file to find>

# Cannot redirect echo even with sudo, use sudo tee
echo 'deb blah # blah' | sudo tee [--append|-a] /etc/apt/sources.list

# Find location of file
locate <filename>
# With wildcard
locate <filename*>  
# Search only names in file i.e. basename
locate -b <filenames>
# Search case insensitive
locate -i <FilENAMe>
# Search, locate using regex
locate -r ^java*
locate -r .sh$ | grep intel  # Find files ends with .sh

# Alternatively, find command
find -iname <file-name>
find -type d -name <directory-name>
# http://stackoverflow.com/questions/6844785/how-to-use-regex-with-find-command
find . -regextype sed -regex ".*\.so"  # output: videoio.so, photo.so ...

# dnf install vim conflict with existing vim-minimal
sudo dnf update vim-minimal
sudo dnf install vim

# Save in vim with sudo
# http://stackoverflow.com/questions/2600783/how-does-the-vim-write-with-sudo-trick-work
:w !sudo tee %

# Setup startup script for everyone
sudo mv script.sh /etc/profile.d/

# Prevent processes from being killed after loggin out of ssh client
nohup program &

# NFS
http://www.server-world.info/en/note?os=CentOS_7&p=nfs
# Check if NFS running
ps aux | grep nfsd
# List mounted folder
showmount -e <server ip>

# Check firewall status
http://www.oracle-base.com/articles/linux/linux-firewall-firewalld.php

# Open port on firewalld, https://fedoraproject.org/wiki/FirewallD
firewall-cmd --get-active-zones
firewall-cmd --zone=dmz --add-port=8080/tcp

# ec2 enable password authentication
http://serverfault.com/questions/295809/how-can-i-login-to-amazon-ec2-without-using-pem-file

# Unrar
sudo dnf -y install unar

# Unzip to a directory
unzip package.zip -d /directory

# Decompress gunzip *.gz
gzip -d <file.gz>

# Parallel compress with pigz
# http://stackoverflow.com/questions/12313242/utilizing-multi-core-for-targzip-bzip-compression-decompression
tar cvf - paths-to-archive | pigz -9 -p 32 > archive.tar.gz

# Compress with gunzip tar.gz (with compr level 9)
tar cvf - /path/to/directory | gzip -9 - > file.tar.gz
tar -zcvf output.tar.gz /home/jerry/folder  # Default

# Extract bz2
bzip2 -d filename.bz2
tar xvjf firefox-31.0.tar.bz2

# Extract to specific directory, -C or --directory
tar -xf archive.tar -C /target/directory

# Zip with password
zip -r --encrypt out.zip folder

# Setup gpg
# https://www.digitalocean.com/community/tutorials/how-to-use-gpg-to-encrypt-and-sign-messages-on-an-ubuntu-12-04-vps
gpg --list-keys
gpg --gen-key

# Encrypt with gpg
# http://www.cyberciti.biz/tips/linux-how-to-encrypt-and-decrypt-files-with-a-password.html
gpg -c <myfile>  # Encrypt with passphrase
# Encrypt without compression
gpg -c --compress-algo none <myfile>

# Shorten bash prompt
http://askubuntu.com/questions/145618/how-can-i-shorten-my-command-line-bash-prompt
export PS1='\u:\W\$ '
export PS1='[\u@\h \W]\$ '  # Default for Fedora

# Clear bash history
> ~/.bash_history && history -c

# Check which repo provides the package
dnf info <package-name>

# Location of repo for dnf
/etc/yum.repos.d/
# To disable a .repo
enabled=0
# Check if repo is installed with rpm and unistall rpm if yes
rpm -qa |grep -i <repo-name>
rpm -e <repo-name>

# Add EPEL repo, contains a lot extra softwares
# http://www.tecmint.com/how-to-enable-epel-repository-for-rhel-centos-6-5/
sudo yum install epel-release
sudo dnf -y install http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm

# Install VLC
sudo dnf -y install http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-23.noarch.rpm http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-23.noarch.rpm
sudo dnf -y install vlc

# Install nux-desktop repo
# http://wiki.centos.org/TipsAndTricks/MultimediaOnCentOS7
sudo dnf -y install http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm

# Add rpmforge repo
# http://www.tecmint.com/enable-rpmforge-repository/
wget http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el7.rf.x86_64.rpm
rpm -Uvh rpmforge-release-0.5.3-1.el7.rf.x86_64.rpm

# Check with repo provides the package or library
dnf provides <package-name>
dnf provides /usr/lib64/libbz2.so

# File permission
----------------------------------------------------
# Make folder permission 755, file permission 644
# http://stackoverflow.com/questions/18817744/change-all-files-and-folders-permissions-of-a-directory-to-644-755
find * -type d -print0 | xargs -0 chmod 0755 # for directories
find . -type f -print0 | xargs -0 chmod 0644 # for files

# Get file permission number of a file
# http://askubuntu.com/questions/152001/how-can-i-get-octal-file-permissions-from-command-line
stat -c "%a %n" *

# Alternative
sudo find /your/location -type f -exec chmod 644 {} 
sudo find /your/location -type d -exec chmod 755 {} 

# http://superuser.com/questions/91935/how-to-chmod-755-all-directories-but-no-file-recursively
# http://superuser.com/questions/264383/how-to-set-file-permissions-so-that-new-files-inherit-same-permissions
chmod -R u+rwX,go+rX,go-w /path

# Make files in folder inherit group ownership
# http://unix.stackexchange.com/questions/75381/keep-same-file-owner-for-newly-created-files
chmod g+s <folder>
-----------------------------------------------------

# Disable SELinux to allow php executed remotely to create file, etc
http://docs.fedoraproject.org/en-US/Fedora/13/html/Security-Enhanced_Linux/sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux.html

sudo vim /etc/selinux/config
SELINUX=disabled
$ getenforce  # should return disabled

# Install latest firefox
http://tecadmin.net/install-firefox-on-linux/

# Disable keyring
https://wiki.archlinux.org/index.php/GNOME_Keyring#Disable_Keyring_daemon

# Disable Fedora Gnome auto update
gsettings set org.gnome.software download-updates false

# Disable PackageKit from update
sudo /usr/bin/gpk-prefs
Select 'Never'

# Disable CPU cores
http://www.absolutelytech.com/2011/08/01/how-to-disable-cpu-cores-in-linux/
http://pundiramit.blogspot.sg/2010/07/how-to-disable-cpu-cores-in-multicore.html
sudo sh -c "echo 0 > /sys/devices/system/cpu/cpu7/online"

# Top for Nvidia GPU
http://stackoverflow.com/questions/8223811/top-command-for-gpus-using-cuda
nvidia-smi -q -g 0 -d UTILIZATION -l
nvidia-smi -l <refresh_in_seconds>

# Clear terminal and previous lines
# http://superuser.com/questions/122911/bash-reset-and-clear-commands
echo -e '\0033\0143'

# Convert RedHat to CentOS
http://jensd.be/?p=32
http://lintut.com/convert-rhel-6-x-to-centos-6-x/

# Clean up existing cache of metadata and packages. Also, erase RedHat subscription manager.
dnf clean all
rpm -e subscription-manager
# Create a directory to keep CentOS packages
mkdir /root/centos
cd /root/centos
# Download the packages
wget http://mirror.centos.org/centos/6.3/os/x86_64/RPM-GPG-KEY-CentOS-6
wget http://mirror.centos.org/centos/6.3/os/x86_64/Packages/centos-release-6-3.el6.centos.9.x86_64.rpm
wget http://mirror.centos.org/centos/6.3/os/x86_64/Packages/dnf-3.2.29-30.el6.centos.noarch.rpm
wget http://mirror.centos.org/centos/6.3/os/x86_64/Packages/dnf-utils-1.1.30-14.el6.noarch.rpm
wget http://mirror.centos.org/centos/6.3/os/x86_64/Packages/dnf-plugin-fastestmirror-1.1.30-14.el6.noarch.rpm
# Next enter the commands below to first import CentOS GPG key, to install the packages and then to upgrade to CentOS 6
rpm –import RPM-GPG-KEY-CentOS-6
rpm -e –nodeps redhat-release-server
rpm -e dnf-rhn-plugin rhn-setup rhn-check rhn-setup-gnome rhnsd
rpm -Uhv –force *.rpm
dnf upgrade
# Reboot the server in order to complete the installation.

# Retrieve command line history
history

# Export environment var
export PATH=$PATH:/usr/local/bin

# Print environment variable nicely
# tr (translate)
# -s squeeze: replace <from> to>
echo $PATH | tr -s ':' '\n'

# Set environment variable for sudo
sudo env "PATH=$PATH" sublime_text

# Check which group I belong to
groups

# Understand sudo and admin (wheel) group
http://www.cyberciti.biz/faq/linux-sudo-allows-people-in-group-admin/
# Check user detail eg which group a user belongs to
id <username>
# Show users belong to certain group
getent group <groupname>

# Add new user
sudo adduser newuser
sudo passwd newuser

# Add user to group: Add david to wheel
usermod -a -G wheel davidheryanto

# Allow a folder to be writable by all users in group
# http://superuser.com/questions/19318/how-can-i-give-write-access-of-a-folder-to-all-users-in-linux
sudo chgrp -R mygroup /var/www
sudo chmod -R g+w /var/www

# Slow password prompt on ssh
http://askubuntu.com/questions/246323/why-ssh-password-prompt-takes-too-long-to-appear
/etc/ssh/sshd_config: 'useDNS no' or 'GSSAPIAuthentication no'

# Update database so 'locate' command will work
sudo ionice -c3 updatedb

# Shrink logical and physical volume for lvm partition
# http://askubuntu.com/questions/252204/how-to-shrink-ubuntu-lvm-logical-and-physical-volumes
lvresize --verbose --resizefs -L -150G /dev/fedora/home
pvs -v --segments /dev/sda5
sudo pvmove --alloc anywhere /dev/sda5:<start>-<end>
# Use gparted to resize the physical volume

# Extend partition
http://www.rootusers.com/how-to-increase-the-size-of-a-linux-lvm-by-expanding-the-virtual-machine-disk/
# If file system is xfs
http://stackoverflow.com/questions/13362910/trying-to-resize2fs-eb-volume-fails

# Share internet connection
https://wiki.archlinux.org/index.php/Internet_sharing

# Red Hat documentation
https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7

# Check all the components
sudo dmidecode
# Get devices location on PCI bus
lspci

# sudo no password. Add this add the LAST line cuz sudo reads sudoes from top to bottom
# > sudo visudo 
davidheryanto ALL=NOPASSWD:ALL

# Where to assign dns server
/etc/resolv.conf

# Change timezone
# http://unix.stackexchange.com/questions/110522/timezone-setting-in-linux/110529
ln -sf /usr/share/zoneinfo/Asia/Singapore /etc/localtime  # Red Hat distros
sudo dpkg-reconfigure tzdata  # Ubuntu/Debian distros
# Verify
date

# Wall to specific user
write <username> [tty]
# Asterisk * cannot expand folders w/o permission even with sudo
http://unix.stackexchange.com/questions/101847/cannot-expand-asterisk-without-proper-permission
sudo sh -c 'ls -l /temp/sit/build/*'

# Mount a new partition
# https://wiki.archlinux.org/index.php/add_new_partitions_to_an_existing_system
Format the new partition using gparted
mount /dev/sda2 /mnt/mountfolder
# Add this entry to /etc/fstab
/dev/sda2 /newdir ext3 defaults 0 2

# If cannot format using gparted
sudo fdisk -l  # Check drives
sudo mkfs -t ext3 /dev/sdf
sudo mount /dev/sdf /mnt/data-store
sudo vim /etc/fstab
/dev/sdf /mnt/data-store ext3 defaults,noatime 1 2

# Mount flash drive
sudo fdisk -l  # Check which drive to mount e.g. /dev/sdb1
sudo mkdir /media/usb  # Mount folder
sudo mount /dev/sdb1 /media/usb
sudo umount /media/usb  # When done

# Mount external hard drive
http://askubuntu.com/questions/177825/how-to-mount-an-external-hdd
sudo fdisk -l  # Check if disk is available
sudo mount -t ntfs /dev/sdb1 /media

# Summary of disk usage, check folder size with ls
http://superuser.com/questions/162749/how-to-get-the-summarized-sizes-of-folders-and-their-subfolders
sudo du -sh /* | sort -h  # And human-numeric-sort
df -h  # Check free disk space

# rsync example
for i in {1..4}
do
	rsync -av --delete $1 ulam$i:$1
done

rsync -av --delete $1 node2:$1
rsync -av --delete $1 node4:$1

rsync -u  # newer mode

# linux symbolic link
ln -s /path/to/file /path/to/symlink

# Move cursor wordwise
Alt+b Alt+f

# CentOS: Add app shortcut to desktop menu
sudo dnf -y install alacarte
# https://developer.gnome.org/integration-guide/stable/desktop-files.html.en
# put *.desktop to /usr/share/applications/
[Desktop Entry]
Type=Application
Encoding=UTF-8
Name=Sample Application Name
Comment=A sample application
Exec=application
Icon=application.png
Terminal=false

# Disable turbo boost
# http://luisjdominguezp.tumblr.com/post/19610447111/disabling-turbo-boost-in-linux
# Replace i in -pi with processor no
sudo dnf install msr
sudo rdmsr -pi 0x1a0 -f 38:38  # Check turbo boost status (0 is on)
sudo wrmsr -pi 0x1a0 0x4000850089

# Enable turbo boost
sudo wrmsr -pi 0x1a0 0x850089

# Sleep
sudo dnf -y install pm-utils
sudo pm-suspend
sudo pm-hibernate
echo mem | sudo tee /sys/power/state
echo disk | sudo tee /sys/power/state

# Sleep at a certain time
echo "sudo pm-suspend" | at hh:mm

# Vertical selection or column selection
Ctrl + Shift + Left Mouse Press and Hold

# Check and limit CPU frequency info
cpupower frequency-info
# Set the max frequency (put a script to /etc/profile.d for auto-startup)
sudo systemctl start cpupower
sudo cpupower frequency-set -u 2.6GHz
# cpupower may conflict with tuned
# https://www.centos.org/forums/viewtopic.php?f=47&t=47982

# Gnome extensions
# Disable touchpad on mouse plugged in
https://github.com/orangeshirt/gnome-shell-extension-touchpad-indicator
# Sleep button
https://github.com/laserb/gnome-shell-extension-suspend-button

# Bash git prompt
# https://github.com/jimeh/git-aware-prompt
mkdir ~/.bash
cd ~/.bash
git clone git://github.com/jimeh/git-aware-prompt.git
vim ~/.bashrc
# Check if shell is interactive first
if ! [ -z "$PS1" ]; then
  export GITAWAREPROMPT=~/.bash/git-aware-prompt
  source $GITAWAREPROMPT/main.sh
  export PS1="[\u@\h \W\[$txtcyn\]\$git_branch\[$txtred\]\$git_dirty\[$txtrst\]]\$ "
fi

# Supress stderr
# http://askubuntu.com/questions/350208/what-does-2-dev-null-mean
2>/dev/null
# Supress both stdout and stderr
&>/dev/null

# Change ownership
chown options owner-user:owner-group file/directory

# .bashrc vs .bash_profile bs .profile
# http://superuser.com/questions/409186/environment-variables-in-bash-profile-or-bashrc
# http://stackoverflow.com/questions/415403/whats-the-difference-between-bashrc-bash-profile-and-environment

# Shebang
#!/usr/bin/env python

# Switch users
su - user2
su -  # Switch to root

# Monitor disk activity
sudo dnf -y install iotop
sudo iotop [-oP]  # Show only processes using IO

# Create bootable usb
sudo fdisk -l # Check which sd* usb drive is
sudo dd if=/home/davidheryanto/Downloads/Fedora-Live-Desktop-x86_64-20-1.iso of=/dev/sdX bs=8M

# Delete MBR (Master Boot Record) on bootable usb
# sudo fdisk -l to check where the thumbdrive is sda/sdb?
sudo dd if=/dev/zero of=/dev/sdb bs=446 count=1

# Add xclip, command to copy to clipboard
sudo dnf -y install xclip
# Add alias so xclip default to selection (can use Ctrl+Shift+V etc)
vim ~/.bash_aliases
alias xclip="xclip -selection c"

# Echo no new line
echo -n "text with no newline"

# Alternatives to set different versions of command
alternatives --list
alternatives --display java
sudo alternatives --install <link> <name> <path> <priority>
sudo alternatives --config java

# Improve font rendering on CentOS
# https://www.dropbox.com/s/73plzdw2d1h3md6/Infinality.tar.gz?dl=0
sudo dnf -y install http://www.infinality.net/fedora/linux/20/noarch/fontconfig-infinality-1-20130104_1.noarch.rpm http://www.infinality.net/fedora/linux/20/x86_64/freetype-infinality-2.4.12-1.20130514_01.fc18.x86_64.rpm
OR
sudo dnf -y install freetype-infinality
# Set style then log out and log in
sudo /etc/fonts/infinality/infctl.sh setstyle

sudo dnf -y install gnome-tweak-tool

# Font: Ubuntu
http://font.ubuntu.com/download/ubuntu-font-family-0.83.zip
# Font: Consolas
https://www.dropbox.com/s/y7jonocw202oxgq/Consolas.ttf?dl=0
# Font: San Fransisco
https://www.dropbox.com/s/ueu7tzseckeggry/SanFransisco.tar.gz?dl=0
# Font: Helvetica
https://www.dropbox.com/s/g080l2afa0tdt2n/Helvetica.zip?dl=0
# Font: Monaco
https://www.dropbox.com/s/vk8uffkot4oilzf/Monaco.zip?dl=0
# Font: Bookerly
https://www.dropbox.com/s/wcrjj8m086agp3f/Bookerly.zip?dl=0

# Install font, assume we have Helvetica folder in current dir
sudo cp -R ./Helvetica /usr/share/fonts/
sudo fc-cache -f /usr/share/fonts/

# Set font to substitute for Arial (default is Liberation Sans in Fedora)
vim /etc/fonts/conf.d/59-liberation-sans.conf
vim /etc/fonts/conf.d/57-dejavu-sans-mono.conf

# Install database GUI: dbeaver
http://dbeaver.jkiss.org/download/

# Share files from command line, easy way: https://transfer.sh
curl --upload-file ./hello.txt http://transfer.sh/hello.txt

# Curl with HTTP basic authentication
curl -u myusername:mypassword http://somesite.com

# Curl POST request
# http://superuser.com/questions/149329/what-is-the-curl-command-line-syntax-to-do-a-post-request
curl --data "param1=value1&param2=value2" https://example.com/resource.cgi
curl --form "fileupload=@my-file.txt" https://example.com/resource.cgi

# Curl POST JSON
# http://stackoverflow.com/questions/7172784/how-to-post-json-data-with-curl-from-terminal-commandline-to-test-spring-rest
curl -X POST -H "Content-Type: application/json" -d '{"key":"val"}' URL

# Remote desktop to Windows
# sudo dnf -y install freerdp
# https://github.com/awakecoding/FreeRDP-Manuals/blob/master/User/FreeRDP-User-Manual.markdown
xfreerdp /u:Administrator /v:192.168.2.65 /size:1200:800 +clipboard +fonts
# Old method
sudo dnf -y install rdesktop
# -z: rdp compression; -g: geometry
# -r: device redirection to share files 
rdesktop -z -g 1280x720 -r disk:share=/home/davidheryanto/Downloads -u Administrator -p - notebook.jumpingcrab.com

# Logout
gnome-session-quit

# Install USB wireless adapter Prolink WN2001 (Need to install kernel-devel first)
lsusb  # To check the device id, in this case 07b8:8179 AboCom Systems Inc
git clone https://github.com/lwfinger/rtl8188eu  # Repository for the driver
# After finish installing
sudo modprobe 8188eu

# Change default text editor to sublime-text
# http://askubuntu.com/questions/396938/how-do-i-make-sublime-text-3-the-default-text-editor
sudo [vim|sublime-text] /usr/share/applications/defaults.list
# Replace all gedit.desktop with sublime-text.desktop

# Change default torrent client
# http://unix.stackexchange.com/questions/75614/set-transmission-as-default-program-when-opening-magnet-links
sudo vim ~/.local/share/applications/mimeapps.list
x-scheme-handler/magnet=qBittorrent.desktop

# Configure and make
./configure --prefix=$HOME/local  # to install at custom dir
--disable-shared

# Limit process by % cpu
cpulimit -p <pid> -l <percent-cpu>

# Monitor CPU temperature
sudo dnf install install lm_sensors
sudo sensors-detect
watch -n 1 sensors

# Watch a sequence of commands
watch -n1 "sensors | grep temp | awk '{ print $2 }'"

# Convert office files (wordx, pptx) to other format
unoconv -f pdf input.pptx

# Printing in SOC
sudo dnf install system-config-printer
samba windows: smb://nusstu/nts27.comp.nus.edu.sg/psts-dx
# Perform operation on bash script
# http://stackoverflow.com/questions/6348902/how-can-i-add-numbers-in-a-bash-script
$((6 + 8))

# Execute remote command  (-f for shell to return immediately, no waiting)
ssh -f "hostname; uptime"

# SSH with X forwarding
ssh -X
ssh -Y  # Enable trused X11 forwarding

# Enable X11 after becoming root
http://blog.mobatek.net/post/how-to-keep-X11-display-after-su-or-sudo/

# Redirect time output to file
{ time sleep 1; } 2> time.txt  # Must have space after and before { }

# Add MTP support 
sudo dnf -y install gvfs-mtp.x86_64

# Run command eg suspend at specific time
# http://askubuntu.com/questions/78907/how-can-i-hibernate-suspend-from-the-command-line-and-do-so-at-a-specific-time
echo pm-hibernate | sudo at now + 30 min
echo pm-suspend | sudo at 11pm (or 15:30)

# Sound an alert
echo -e "\a"  # -e to interpret escape char

# Disable sound (especially for centos minimal)
# http://unix.stackexchange.com/questions/152691/how-to-disable-beep-sound-in-linux-centos-7-command-line
echo "blacklist pcspkr" | sudo tee /etc/modules-load.d/bell.conf
sudo rmmod pcspkr

# Disable sound only on terminal
echo 'set bell-style none' >> ~/.inputrc

# VPN NUS
http://blog.yjwong.name/using-the-new-soc-vpn-on-linux/
http://www.abdelmartinez.com/2013/06/forticlient-ssl-vpn-routing-problem.html  # Fix routing

# Find files with size greater than 
find . -type f -size +10M
find . -maxdepth 1 -type f -size +100M

# Send notifications
notify-send "This is a notification"
# Clear all notifications
sudo killall notify-osd

# Setup SSH Tunnel, e.g. via SOC
ssh -D 8080 -C -N a0083545@sunfire.comp.nus.edu.sg
# Then setup SOCKS Proxy via 127.0.0.1:8080

# http proxy and https proxy
export http_proxy=http://remote.proxy.com:8080
export https_proxy=https://remote.proxy.com:8080

# dnf proxy and certificate check
sudo vim /etc/dnf/dnf.conf
proxy=http://remote.proxy.com:8080
proxy_username=
proxy_password=
sslverify=False  # Disable certificate check

# Convert pdf to epub
sudo dnf -y install calibre
pip install cssutils cssselect
ebook-convert file.pdf file.epub [--enable-heuristics]

# Convert pdf to text
sudo dnf -y install poppler-utils
pdftotext input.pdf output.txt
for file in *.pdf; do pdftotext "$file" "$file.txt"; done  # Batch

# Remove pdf password
sudo dnf -y install qpdf
qpdf --decrypt input.pdf output.pdf

# Record screen / Screen capture - SimpleScreenRecorder
sudo dnf install dnf-plugins-core
sudo dnf copr enable nickth/ssr
sudo dnf install ssr

# View video information
sudo dnf -y mediainfo

# Tools to work csv
pip install csvkit

# Convert .xlsx to .csv then insert to MySQL database
in2csv --sheet <sheetname> <infile>.xlsx | csvsql --insert \
  --db mysql://user:password@<hostname>/<dbname> \
  --table <newtablename>

# Check public ip
curl ipecho.net/plain

# Calculate hash
echo -n "text to hash" | shasum

# Set dynamic library path
export LD_LIBRARY_PATH=mylibpath

# Fix grub2 after installing Windows
# https://www.reddit.com/r/linuxquestions/comments/3b8tlp/how_do_i_fix_grub2_so_that_fedora_22_will_show_up/
# Check with fdisk -l
mkdir /mnt/root
mount /dev/mapper/fedora-root /mnt/root
mount /dev/sda1 /mnt/root/boot
mount -o bind /dev /mnt/root/dev
mount -o bind /proc /mnt/root/proc
mount -o bind /sys /mnt/root/sys
mount -o bind /run /mnt/root/run
chroot /mnt/root
grub2-install --no-floppy --recheck /dev/sda
grub2-mkconfig -o /boot/grub2/grub.cfg
exit
/sbin/shutdown -r now

# Convert CER (DER encoded) to PEM
# https://www.sslshopper.com/ssl-converter.html
openssl x509 -inform der -in certificate.cer -out certificate.pem

# Add certificate: new root CA
# https://www.happyassassin.net/2015/01/14/trusting-additional-cas-in-fedora-rhel-centos-dont-append-to-etcpkitlscertsca-bundle-crt-or-etcpkitlscert-pem/
sudo cp <new-ca>.pem /etc/pki/ca-trust/source/anchors/
sudo update-ca-trust

# Activate Office 2010 with KMS
# http://askubuntu.com/questions/277709/activate-office-2010-running-in-playonlinux-with-a-kms-server
wine regedit
HKEY_LOCAL_MACHINE\Software\Microsoft\OfficeSoftwareProtectionPlatform
Add String Value: KeyManagementServiceName - Server Addr
Add String Value: KeyManagementServicePort - Port (1688)
-----
Check
HKEY_USERS\S-1-5-20\Software\Microsoft\OfficeSoftwareProtectionPlatform

# Change default font for serif, sans-serif etc
http://seasonofcode.com/posts/how-to-set-default-fonts-and-font-aliases-on-linux.html

# Setup MKL env variables
source /opt/intel/bin/compilervars.sh intel64

# Install printer config tool
sudo dnf -y install system-config-printer

# Install Fedora on USB Drive
https://fedoraproject.org/wiki/How_to_create_and_use_Live_USB

# Set the number of file descriptors
# http://docs.hortonworks.com/HDPDocuments/Ambari-2.1.1.0/bk_Installing_HDP_AMB/content/_check_the_maximum_open_file_descriptors.html
ulimit -Sn  # Check first
ulimit -Hn
ulimit -n 10000  # Set to 10000

# Generate system resource statistics
sudo dnf -y install dstat
dstat -tcmd > dstat.out

# Combine with gnuplot to output graph
# http://www.linuxuser.co.uk/tutorials/create-a-graph-of-your-systems-performance/4

# Remove line with words time/date/system
grep -Ev 'time|date|system' dstat.out > dstat-2.out

#!/usr/bin/gnuplot
set terminal png
set output "cpu.png"
set title "CPU usage"
set xlabel "time"
set ylabel "percent"
set xdata time
set timefmt "%d-%m %H:%M:%S"
set format x "%H:%M"
plot "dstat-2.out" using 1:3 title "system" with lines, \
"dstat-2.out" using 1:2 title "user" with lines, \
"dstat-2.out" using 1:4 title "idle" with lines

# Create startup script for systemd
# http://stackoverflow.com/questions/28420466/fedora-20-how-to-run-script-at-the-end-of-startup
# http://stackoverflow.com/questions/29508981/systemd-service-startup-issue
$ sudo vim /usr/lib/systemd/system/<service_name>.service
[Unit]
Description=<description_string>

[Service]
WorkingDirectory=<working_directory>
Type=simple
ExecStart=/bin/bash <absolute_path_to_script>
KillMode=process  # or process https://www.freedesktop.org/software/systemd/man/systemd.kill.html#
User=user

[Install]
WantedBy=multi-user.target

# Reload systemd after changes in the unit service description
sudo systemctl daemon-reload

# List all services https://fedoraproject.org/wiki/SysVinit_to_Systemd_Cheatsheet
systemctl list-unit-files --type=service

# Disable firewall
systemctl disable firewalld
systemctl stop firewalld

# Startup service
systemctl enable <service-name>

# Install BIND tools e.g. to find ip of host
sudo dnf -y install bind-utils

# Find ip of domain
# http://unix.stackexchange.com/questions/20784/how-can-i-resolve-a-hostname-to-an-ip-address-in-a-bash-script
dig +short google.com

# Password generator: pwgen [ OPTION ] [ pw_length ] [ num_pw ]
pwgen

# Check sha1 of file
sha1sum filename

# Journalctl for systemd logs
==================================================================
# https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs
timedatectl status  # Check system time
journalctl 
-b: logs from current boot (-1/caf0524a1d394ce0bdbcff75b94444fe: specific boot)
--list-boots

# Filter by time
--since "2015-01-10 17:15:00" --until "2015-01-11 03:00"
--since yesterday 
--since 09:00 --until "1 hour ago"

# Filter by message interest
journalctl -u nginx.service [--since today]
journalctl -u nginx.service -u php-fpm.service --since today  # Nginx connected to PHP-FPM
journalctl _PID=8088
id -u user / id -G usergroup
journalctl _UID=33 --since today  # _GID

# Change ls color (directory/folder color)
# http://askubuntu.com/questions/466198/how-do-i-change-the-color-for-directories-with-ls-in-the-console
vim ~/.bashrc
export LS_COLORS=$LS_COLORS:'di=0;36:'

# Check available fields
man systemd.journal-fields
journalctl -F _GID  # check which group IDs the systemd journal has entries for

journalctl /usr/bin/bash  # Those involve that executable
journalctl -k  # Kernel messages

journalctl -p err -b   # Error level and above from current boot
                       # crit, error, warning, info, debug

journalclt --no-pager  # Prints to stdout, default is pager
journalctl -b -u nginx -o json
journalctl -b -u nginx -o json-pretty

journalctl -n [10]     # Recent 10 messages
journalctl -f          # Follow logs as they are being written

journalctl --disk-usage  # How much storage is used by journal
sudo journalctl --vacuum-size=1G  # Remove old entries until space taken is 1G
sudo journalctl --vacuum-time=1years

# Journalctl save past boots
sudo mkdir -p /var/log/journal OR
sudo vim /etc/systemd/journald.conf
  [Journal]
  Storage=persistent
  SystemMaxUse=1G/500M
==================================================================

# Enable normal user to run on port 80
# https://www.digitalocean.com/community/tutorials/how-to-use-pm2-to-setup-a-node-js-production-environment-on-an-ubuntu-vps
sudo apt-get install libcap2-bin
sudo setcap cap_net_bind_service=+ep /usr/local/bin/node 
sudo setcap cap_net_bind_service=+ep /usr/bin/nodejs  # In Ubuntu

# Install gem to manage ruby package
sudo dnf rubygem-bundler

# Cron tabs
# https://en.wikipedia.org/wiki/Cron
┌───────────── min (0 - 59)
│ ┌────────────── hour (0 - 23)
│ │ ┌─────────────── day of month (1 - 31)
│ │ │ ┌──────────────── month (1 - 12)
│ │ │ │ ┌───────────────── day of week (0 - 6) (0 to 6 are Sunday to
│ │ │ │ │                  Saturday, or use names; 7 is also Sunday)
│ │ │ │ │
│ │ │ │ │
* * * * *  command to execute

# Check if server is serving gzipped content
# http://stackoverflow.com/questions/9140178/how-can-i-tell-if-my-server-is-serving-gzipped-content 
curl http://example.com/ --silent --write-out "%{size_download}\n" --output /dev/null
curl http://example.com/ --silent -H "Accept-Encoding: gzip,deflate" --write-out "%{size_download}\n" --output /dev/null