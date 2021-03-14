```
#!/bin/bash
set -x

# update and install samba
echo "update and install samba"
sudo apt-get update
sudo apt-get upgrade --assume-yes
sudo apt-get install --assume-yes samba samba-common-bin

# /etc/samba/smb.conf
SAMBA_CONF="/etc/samba/smb.conf"

echo "(re)create ${SAMBA_CONF}"

sudo rm -rf "${SAMBA_CONF}"

sudo cat >$SAMBA_CONF <<EOL
[global]
workgroup = WORKGROUP
security = user
encrypt passwords = yes
usershare allow guests = no
guest account = nobody
map to guest = bad user
 
[home]
path = /home/pi
read only = false
public = true
writable = true
guest ok = false
browsable = true
EOL

echo "contents of ${SAMBA_CONF}:"
sudo cat "${SAMBA_CONF}"

# create samba password
echo "create samba password for user pi"
sudo smbpasswd -a pi
```
