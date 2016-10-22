# Patriot

Experimenting with scripts, tools, and documentation to do system hardening for CyberPatriot. 

Make backups of key files (/etc/group, passwd, shadow)

Take list of users as input
    Remove extras
    Change password for the rest

Search for audio/video
find / -type f \( -name "*.mp3" -o -name "*.mp4" -o -name "*.mov" -o -name "*.wav" -o -name "*.avi" \)
  
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

vi /etc/ssh/sshd_config
    PermitRootLogin no
    PasswordAuthentication no

Firewall:
    sudo ufw allow from any to any port 22
    sudo ufw deny from any to any
    sudo ufw enable


# Disable root login
  sudo passwd -l root


# fix home directory permissions
  sudo chmod 0750 /home/*
  
  Antivirus:
  sudo apt-get install clamav
  sudo freshclam
    # Scans the whole computer, only display infected files, ring a bell when found
  sudo clamscan -r --bell -i /
 
 
 # Find any nc instances
    ps -eaf | grep nc
    
 # same in cron
    cat /etc/cron*/* | grep nc
    
  # remove unneeded services
sudo apt-get remove john vsftpd
 
    
Adjust some networking options:

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
    
    
From practice images 10/21/2016:
--Forensics Question Correct - 14 pts
--Removed unauthorized user lluthor - 8 pts
--User tstark is not an administrator - 8 pts
--FTP service is disabled or removed - 11 pts
--Prohibited software john the ripper removed - 11 pts
--Removed netcat backdoor - 12 pts
--Prohibited movie trailer is removed - 8 pts
--SSH root login has been disabled - 10 pts


    