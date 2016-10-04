# Add Linux to Boot Manager
http://superuser.com/questions/499617/how-can-i-add-linux-to-the-new-windows-8-boot-manager

# Open file with command line 
start <filename>

# Rename user folder
http://superuser.com/questions/495290/how-to-rename-user-folder-in-windows-8
1. Login with other admin account
2. Win+X, G (Computer Management) → System Tools → Local Users and Groups → Users, right-click user, Rename.
3. Win+X, A (Command Prompt (Admin))
> ren C:\Users\dzinx_000 dzinx
4. Navigate to HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList\ and find the SID for your user account. You can simply open each folder and check the ProfileImagePath for the correct one. Rename the ProfileImagePath value to your desired name, like C:\Users\dzinx

Windows10: MouseWheelRouting

# Search filter
# http://windows.microsoft.com/en-sg/windows7/advanced-tips-for-searching-in-windows
# http://www.sevenforums.com/tutorials/286043-search-find-more-filters-operators.html
filename:
datetaken: datecreated: datemodified:
kind:{picture,document...}
ext:{doc,zip...}
size:

Filter:
~!: does not contain
~<: starts with
~>: ends with

Wildcards:
*
?

# Decrypt and extract esd
http://forums.mydigitallife.info/threads/53855-Windows-8-1-with-Update-ESDs-Repository

# Add DNS server, run powershell as admin
interface show interface  # Check <interface-name> under 'Interface Name'
netsh interface ipv4 add dnsserver <interface-name> address=8.8.8.8 index=1

# Schedule Windows to sleep
psshutdown -d -t 02:00

# hosts file
C:\Windows\System32\drivers\etc\hosts

# Sreenshot
Alt + Print Screen: Copy 'active' window into clipboard

# Check which app is using a file
http://superuser.com/questions/117902/find-out-which-process-is-locking-a-file-or-folder-in-windows

# Measure IOPS
http://blogs.technet.com/b/josebda/archive/2014/10/13/diskspd-powershell-and-storage-performance-measuring-iops-throughput-and-latency-for-both-local-disks-and-smb-file-shares.aspx

# Start menu path location
%APPDATA%\Microsoft\Windows\Start Menu\Programs

# Windows certificate manager
certmgr.msc

# Scroll mouse for inactive windows with Katmouse on Windows 7 and below
http://ehiti.de/katmouse/KatMouseSetup.exe

# Disable OneDrive in Explorer in Windows 10. Run gpedit.msc
# http://www.howtogeek.com/225973/how-to-disable-onedrive-and-remove-it-from-file-explorer-on-windows-10/
Local Computer Policy\Computer Configuration\Administrative Templates\Windows Components\OneDrive

# Install .NET Framework 3.5 offline
DISM /Online /Enable-Feature /FeatureName:NetFx3 /All /LimitAccess /Source:d:\sources\sxs

# Change windows border size
HKEY_CURRENT_USER\Control Panel\Desktop\WindowMetrics
-- BorderWidth & PaddedBorderWidth set to 0

# Make titlebar smaller as well
# http://superuser.com/questions/461982/how-do-i-reduce-the-size-of-the-titlebar-and-window-border-padding-on-windows-8
# Save the following as .reg and run it
Windows Registry Editor Version 5.00

    [HKEY_CURRENT_USER\Control Panel\Desktop\WindowMetrics]
    "CaptionHeight"="-285"
    "CaptionWidth"="-285"
    "CaptionFont"=hex:f4,ff,ff,ff,00,00,00,00,00,00,00,00,00,00,00,00,90,01,00,00,\
      00,00,00,01,00,00,05,00,53,00,65,00,67,00,6f,00,65,00,20,00,55,00,49,00,00,\
      00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,\
      00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00,00
    "ScrollWidth"="-240"
    "ScrollHeight"="-240"
    "PaddedBorderWidth"="0"

# Automatically boot Windows To Go
pwlauncher /enable  # Run cmd as admin

# Fix folder keeps opening in new window, cuz of Photoshop CC Export Tool
# https://forums.adobe.com/message/8200779#8200779
Remove this key
Regjump HKEY_CURRENT_USER\SOFTWARE\MICROSOFT\WINDOWS\CURRENTVERSION\EXPLORER\MODULES\NavPane

# Format / wipe an encrypted drive
http://timourrashed.com/clean-wipe-encrypted-disk-on-windows/

# Set different wallpapers for multiple monitors
Run: control /name Microsoft.Personalization /page pageWallpaper

# Windows 10: Change lock screen background
# http://www.howtogeek.com/223875/how-to-change-the-login-screen-background-on-windows-10/
Regjump HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\System
Add DWORD: DisableLogonBackgroundImage, Set value to 1

# Bitlocker
manage-bde -unlock c: -password
manage-bde -protectors c: -get

# Enable PIN after drive is encrypted
# http://serverfault.com/questions/55394/how-do-i-set-the-bitlocker-pin
- Local Computer Policy > Computer Configuration > Administrative Templates > Windows Components > Bitlocker Drive Encryption > Operating System Drives
- Require additional authentication at startup > Require startup PIN with TPM
CMD: manage-bde -protectors -add c: -TPMAndPIN 

# Fast xmlreader, can handle large file
http://www.firstobject.com/dn_editor.htm
https://www.dropbox.com/s/i0zmjv5mdfvak3t/foxesetup242.exe?dl=0

# Calculate md5 of file
# http://superuser.com/questions/245775/is-there-a-built-in-checksum-utility-on-windows-7
certutil -hashfile pathToFileToCheck [HashAlgorithm]
certutil -hashfile C:\TEMP\MyDataFile.img MD5
# Powershell verify
if ( $($(CertUtil -hashfile C:\TEMP\MyDataFile.img MD5)[1] -replace " ","") -eq "your_hash" ) { echo "ok" }

# Disable Windows Update
# http://forums.mydigitallife.info/threads/62525-Windows-10-Automatic-Updates-Enable-Disable-script
https://www.dropbox.com/s/gu7lu2v54zhl1c4/disable-windows10-update.bat?dl=0

# Font: Bookerly
https://www.dropbox.com/s/wcrjj8m086agp3f/Bookerly.zip?dl=0
# Font: Helvetica
https://www.dropbox.com/s/g080l2afa0tdt2n/Helvetica.zip?dl=0
# Font: Monaco
https://www.dropbox.com/s/vk8uffkot4oilzf/Monaco.zip?dl=0

# Create Ad Hoc Network + Internet sharing (see .html below)
# http://tipsandtricksforum.com/thread-210.html
netsh wlan show drivers  # Check that Hosted network supported=Yes
netsh wlan set hostednetwork mode=allow ssid=<your desired network name> key=<your password>
netsh wlan start hostednetwork
netsh wlan stop hostednetwork
# Setup IPv4 to 169... perhaps so other connected users can connect to the host

# Share a folder
# https://technet.microsoft.com/en-us/library/cc770880.aspx
1. Open Computer Management. (compmgmt.msc)
2. If the User Account Control dialog box appears, confirm that the action it displays is what you want, and then click Yes.
3. In the console tree, click System Tools, click Shared Folders, and then click Shares.
4. On the Action menu, click New Share.
5. Follow the steps in the Create a Shared Folder Wizard, and then click Finish.
# From cmd (admin)
net share <sharename=drive:path>

# Create a symbolic link
mklink <target> <source> 
mklink /D <target_folder> <source>
# Create a symbolic link with powershell
cmd /c mklink <source> <target>

# Remove remote desktop entry
Regjump HKEY_CURRENT_USER\Software\Microsoft\Terminal Server Client\Default

# Remote desktop shortcuts
Alt + Home: Start menu
Ctrl + Alt + Pause: Toggle full-screen
Ctrl + Alt + End: Equivalent to Ctrl + Alt + Del 

# Change default screensaver / Force screensaver
# http://www.computerstepbystep.com/force-specific-screen-saver.html
HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Control Panel
\Desktop

# Repair Windows Installation
# http://answers.microsoft.com/en-us/windows/forum/windows_8-update/i-get-your-pc-needs-to-be-repaired-and-error-code/e17ece75-0c69-4551-a3a1-a4e2ae2e8234?auth=1
bootrec /scanos
bootrec /rebuildbcd
bootrec /fixmbr
bootrec /fixboot

# If /scanos returns Total identified Windows installations: 0
# http://answers.microsoft.com/en-us/windows/forum/windows8_1-windows_install/total-identified-windows-installations-0/52359f87-de4a-41dc-b0c3-cc275e1d9fbf

In my case, I've booted the installation-DVD, Repair, Advances, Command Prompt.
Then I navigated to C:\Windows\System32\config.
There I've renamed ...
DEFAULT, SAM, SECURITY, SOFTWARE, SYSTEM
to *.old and copied the registry-hives from C:\Windows\System32\config\RegBack to C:\Windows\System32\config

# Add open with Sublime Text 3 context. Save the snippet as .bat and run it
# https://gist.github.com/roundand/9367852
```
@echo off
SET st3Path=C:\Program Files\Sublime Text 3\sublime_text.exe
 
rem add it for all file types
@reg add "HKEY_CLASSES_ROOT\*\shell\Open with Sublime Text 3"         /t REG_SZ /v "" /d "Open with Sublime Text 3"   /f
@reg add "HKEY_CLASSES_ROOT\*\shell\Open with Sublime Text 3"         /t REG_EXPAND_SZ /v "Icon" /d "%st3Path%,0" /f
@reg add "HKEY_CLASSES_ROOT\*\shell\Open with Sublime Text 3\command" /t REG_SZ /v "" /d "%st3Path% \"%%1\"" /f
 
rem add it for folders
@reg add "HKEY_CLASSES_ROOT\Folder\shell\Open with Sublime Text 3"         /t REG_SZ /v "" /d "Open with Sublime Text 3"   /f
@reg add "HKEY_CLASSES_ROOT\Folder\shell\Open with Sublime Text 3"         /t REG_EXPAND_SZ /v "Icon" /d "%st3Path%,0" /f
@reg add "HKEY_CLASSES_ROOT\Folder\shell\Open with Sublime Text 3\command" /t REG_SZ /v "" /d "%st3Path% \"%%1\"" /f
pause
```

# Windows Server: Allow multiple sessions per user
regjump HKEY_LOCAL_MACHINE\SYSTEM\CURRENTCONTROLSET\CONTROL\TERMINAL SERVER
Change fSingleSessionPerUser to 0

# Synaptics Touchpad: Enable two finger tap for right click (TouchPadPS2 may be TouchPadPS2*)
[HKEY_CURRENT_USER\Software\Synaptics\SynTP\TouchPadPS2]
"2FingerTapAction"=dword:00000002
[HKEY_CURRENT_USER\Software\Synaptics\SynTP\TouchPadSMB2c]
"2FingerTapAction"=dword:00000002

# Change network location from Public to Private
# http://www.tenforums.com/tutorials/6815-network-location-set-private-public-windows-10-a.html
Run secpol.msc
Left pane: Network List Manager Policies
Right pane: Double cliek the network name