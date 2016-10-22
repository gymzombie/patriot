# Patriot
Experimenting with scripts, tools, and documentation to do system hardening for CyberPatriot. 

Access control
    Remove extra accounts (lluthor)
    Remove any extra admins from wheel in /etc/group (tstark)

Search for audio/video
find / -type f \( -name "*.mp3" -o -name "*.mp4" -o -name "*.mov" -o -name "*.wav" -o -name "*.avi" \)
  
vi /etc/ssh/sshd_config
    PermitRootLogin no
    PasswordAuthentication no
 
  # Remove Netcat Backdoor
    
  # Find the cron
cat /etc/cron*/* | grep nc

  # Kill any that are running
ps -eaf | grep nc
    
    
  # remove unneeded services
sudo apt-get remove john vsftpd
 
 # Autoupdates
 Gui -> Applications -> System Tools -> System Settings -> Software &
 Updates
    Update Tab
        Install important security updates
        (Pick a few reasonable options here like Daily, Download Automatically)
 

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


    
    
 Some stuff I do out of system hardening habit that didn't get me points:
    (AKA Fishing for solutions...)
 
 Make backups of key files (/etc/group, passwd, shadow)

 # Adjust some networking options:

vi /etc/sysctl.conf
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
    
In System Settings GUI, set the Screen Saver in "Brightness and Lock"

# Disable root login
  sudo passwd -l root


# fix home directory permissions
  sudo chmod 0750 /home/*
  
  Antivirus:
  sudo apt-get install clamav
  sudo freshclam
    # Scans the whole computer, only display infected files, ring a bell when found
  sudo clamscan -r --bell -i /
 
Firewall:
    sudo ufw allow from any to any port 22
    sudo ufw deny from any to any
    sudo ufw enable

  Set password parameters
    for each in $USERLIST
        do
        # Sets min password change to 0, Max to 90, Warning at 7. 
            chage -m 0 -M 90 -W 7 $each
        done

  # Minimum length (None of these gave me points, but WTH):
  sudo vi /etc/pam.d/common-password
password	[success=1 default=ignore]	pam_unix.so obscure sha512 minlen=8 remember=5
# Complexity:
password requisite pam_cracklib.so ucredit=-1 lcredit=-1 dcredit=-1  ocredit=-1
password requisite pam_cracklib.so retry=3 minlen=10 difok=3 ucredit=-1 lcredit=-1 dcredit=-1  ocredit=-1



