```
#!/bin/bash
set -x

if [ $# -eq 0 ]
then
  echo "usage: install-mail.sh <mail> <password>"
  exit 1
fi

M_MAIL="${1}"
M_PASSWORD="${2}"

sudo apt-get update
sudo apt-get install --assume-yes ssmtp mailutils

sudo rm -rf "/etc/ssmtp/revaliases"
sudo cat >/etc/ssmtp/revaliases <<EOL
# sSMTP aliases
# 
# Format:	local_account:outgoing_address:mailhub
#
# Example: root:your_login@your.domain:mailhub.your.domain[:port]
# where [:port] is an optional port number that defaults to 25
root:${M_MAIL}:mail.gmx.net:465
www-data:${M_MAIL}:mail.gmx.net:465
pi:${M_MAIL}:mail.gmx.net:465
EOL


sudo rm -rf "/etc/ssmtp/ssmtp.conf"
sudo cat >/etc/ssmtp/ssmtp.conf <<EOL
root=${M_MAIL}
mailhub=mail.gmx.net:465
rewriteDomain=gmx.net
hostname=gmx.net
FromLineOverride=NO
AuthUser=${M_MAIL}
AuthPass=${M_PASSWORD}
UseTLS=YES
EOL
```
