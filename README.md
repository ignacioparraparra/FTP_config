# FTP configuration and Hardening

# Objective

# Skills learned

# Walk Through
Install vsftpd
```
 sudo apt update && apt install -y vsftpd
```
Generate Certificate for SSL
```
sudo openssl req -x509 -nodes -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/certs/vsftpd.crt -days 365 -newkey rsa:2048
```
Create Directory for user input and output
```
#     sudo -i
#     useradd ftpsecure
#     USERS=$(ls /home)
#     for u in $USERS; do
#             echo $u >> /etc/vsftpd.userlist
#             mkdir -p /home/$u/ftp/files
#             chown nobody:nogroup /home/$u/ftp
#             chown $u:$u /home/$u/ftp/files
#             chmod a-w /home/$u/ftp
#      done
```
Using OsbournePro config template and creating backups
```
#     sudo mv /etc/vsftpd.conf /etc/vsftpd.conf.orig && echo "[*] Created backup of original /etc/vsftpd.conf file at /etc/vsftpd.conf.orig"
#     sudo wget https://raw.githubusercontent.com/OsbornePro/ConfigTemplates/main/vsftpd.conf%20for%20FTP%20over%20SSL -O /etc/vsftpd.conf
#     sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.bak && echo "[*] Created backup of active /etc/vsftpd.conf file at /etc/vsftpd.conf.bak"
```
Adjust firewall rules
```
#   UFW
#        sudo ufw allow 20:21/tcp
#        sudo ufw allow 40000:41000/tcp
#        sudo ufw reload
#        OR
#        sudo ufw allow ftp-data
#        sudo ufw allow ftp
#        sudo ufw reload
```
Using filezilla for connecting 
```
sudo apt get filezilla
```
Using lftp for ftps
```
sudo apt get lftp
lftp ftps://user@server:21 -e "set ssl:verify-certificate false" #this option stops flagging of self signed cert
```

# Automation

# Considerations
