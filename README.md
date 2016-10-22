# Patriot
Experimenting with scripts, tools, and documentation to do system hardening for CyberPatriot. 

1. Access control
   Remove extra accounts `sudo deluser lluthor`
   Remove any extra admins from wheel in /etc/group `tstark`
2. Search for audio/video
```bash
find / -type f \( -name "*.mp3" -o -name "*.mp4" -o -name "*.mov" -o -name "*.wav" -o -name "*.avi" \)
```
3. Modify SSHD
```bash
   vi /etc/ssh/sshd_config
   PermitRootLogin no
   PasswordAuthentication no
```
4. Remove Netcat Backdoor
  * Find the cron
  `cat /etc/cron*/* | grep nc`
  * Kill any that are running
  `ps -eaf | grep nc`
5. Remove unneeded services
   `sudo apt-get remove john vsftpd`
6. Setup Autoupdates
   * Gui -> Applications -> System Tools -> System Settings -> Software & Updates
   * Update Tab
        Install important security updates
        (Pick a few reasonable options here like Daily, Download Automatically)
 
## From the scoring page:
From practice images 10/21/2016:
Forensics Question Correct - 14 pts
Removed unauthorized user lluthor - 8 pts
User tstark is not an administrator - 8 pts
Install updates from important security updates - 9 pts
FTP service is disabled or removed - 11 pts
Prohibited software john the ripper removed - 11 pts
Removed netcat backdoor - 12 pts
Prohibited movie trailer is removed - 8 pts
SSH root login has been disabled - 10 pts


    
## Non-Scoring changes    
Some stuff I do out of system hardening habit that didn't get me points:
    (AKA Fishing for solutions...)
 
 Make backups of key files (/etc/group, passwd, shadow)

 # Adjust some networking options:

`vi /etc/sysctl.conf` And add:
   ```bash
    net.ipv4.conf.default.rp_filter = 1
    net.ipv4.conf.all.rp_filter = 1
    net.ipv4.tcp_syncookies = 0
    net.ipv4.conf.all.accept_redirects = 0
    net.ipv6.conf.all.accept_redirects = 0
    net.ipv4.conf.all.send_redirects = 0
    net.ipv4.conf.all.accept_source_route = 0
    net.ipv6.conf.all.accept_source_route = 0
    net.ipv4.conf.all.log_martians = 1
    net.ipv6.conf.all.disable_ipv6 = 1
    net.ipv6.conf.default.disable_ipv6 = 1
    net.ipv6.conf.lo.disable_ipv6 = 1
 ```
    
In System Settings GUI, set the Screen Saver in "Brightness and Lock"

1. Disable root login
  `sudo passwd -l root`
1. fix home directory permissions
  `sudo chmod 0750 /home/*`
1. Setup Antivirus:
  ```bash 
  sudo apt-get install clamav
  sudo freshclam
    # Scans the whole computer, only display infected files, ring a bell when found
  sudo clamscan -r exclude­dir=^/sys\|^/proc\|^/dev --bell -i /
   ```
1. Firewall:
   ```bash
   sudo ufw allow from any to any port 22
    sudo ufw deny from any to any
    sudo ufw enable
   ```
1. Set password parameters
    ```bash 
    for each in $USERLIST
        do
        # Sets min password change to 0, Max to 90, Warning at 7. 
            chage -m 0 -M 90 -W 7 $each
        done
    ```
1. Password Minimum length:
  ```bash
  sudo vi /etc/pam.d/common-password
    password	[success=1 default=ignore]	pam_unix.so obscure sha512 minlen=8 remember=5
  # Complexity:
  password requisite pam_cracklib.so retry=3 minlen=10 difok=3 ucredit=-1 lcredit=-1 dcredit=-1  ocredit=-1
```
1. Also tried /etc/login.defs for password complexity:
  ```bash
  PASS_MAX_DAYS	90
  PASS_MIN_DAYS	1
  PASS_WARN_AGE	7
  ```
1. Tried removing games from Applications -> Ubuntu Software Center -> Installed
1. Removed /home/mordecai and /home/lluthor

(Neat trick... .bash_history is softlinked to /dev/null)
    Cool anti-forensics trick

Come back to these when working with the kids:
http://r2d2.cochise.edu/guilmetted/CyberPatriot/

