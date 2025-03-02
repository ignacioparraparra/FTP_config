# FTP configuration and Hardening

## Objective
Configure FTP to use SSL through vsftpd (very secure FTP daemon)

## Skills learned
- FTP
- SSL
- openSSL
- BASH
- vsftpd

## Walk Through
### Install vsftpd
```
 sudo apt update && apt install -y vsftpd
```
### Generate Certificate for SSL
```
sudo openssl req -x509 -nodes -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/certs/vsftpd.crt -days 365 -newkey rsa:2048
```
![resizedcrpty](https://github.com/user-attachments/assets/b7ebc0b4-af0e-43d0-be94-804a3f213973)


### Create Directory for user input and output
```
     sudo -i
     useradd ftpsecure
     USERS=$(ls /home)
     for u in $USERS; do
             echo $u >> /etc/vsftpd.userlist
             mkdir -p /home/$u/ftp/files
             chown nobody:nogroup /home/$u/ftp
             chown $u:$u /home/$u/ftp/files
             chmod a-w /home/$u/ftp
      done
```
### Using OsbournePro config template and creating backups
```
     sudo mv /etc/vsftpd.conf /etc/vsftpd.conf.orig && echo "[*] Created backup of original /etc/vsftpd.conf file at /etc/vsftpd.conf.orig"
     sudo wget https://raw.githubusercontent.com/OsbornePro/ConfigTemplates/main/vsftpd.conf%20for%20FTP%20over%20SSL -O /etc/vsftpd.conf
     sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.bak && echo "[*] Created backup of active /etc/vsftpd.conf file at /etc/vsftpd.conf.bak"
```
### Adjust firewall rules
```
   UFW
        sudo ufw allow 20:21/tcp
        sudo ufw allow 40000:41000/tcp
        sudo ufw reload
        OR
        sudo ufw allow ftp-data
        sudo ufw allow ftp
        sudo ufw reload
```
### Using filezilla for gui 
```
sudo apt get filezilla
```
![filzillawork](https://github.com/user-attachments/assets/a698c043-b4ae-48e7-a030-d9f303141c3e)

### Using lftp for CLI (not recommended)
```
sudo apt get lftp
lftp ftps://user@server:21 -e "set ssl:verify-certificate false" #this option stops flagging of self signed cert
```

## Automation
in progress...

## Considerations
in progress...

## References
https://github.com/OsbornePro/ConfigTemplates/tree/main

https://youtu.be/hglh6P1ieiI?si=0VOkb1g_I7z0lVKT

http://vsftpd.beasts.org/vsftpd_conf.html

https://ubuntu.com/server/docs/set-up-an-ftp-server
