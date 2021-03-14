```
#!/usr/bin/env sh
set -x

# ===-------------------------------------------------------------===
# Constants
# ===-------------------------------------------------------------===
export USER_HOME="/home/pi"

export MEDIA_HOME="${USER_HOME}/media"
export SOUNDS_HOME="/usr/share/sounds/pi"
export SCRIPTS_HOME="${USER_HOME}/scripts"
export DEVELOPER_HOME="${USER_HOME}/developer"

export GIT_PROJECT="https://github.com/lukasdanckwerth/investigators-pi.git"
export GIT_PROJECT_HOME="${DEVELOPER_HOME}/investigators-pi"


# ===-------------------------------------------------------------===
# UPDATE AND INSTALL PACKAGES
# ===-------------------------------------------------------------===
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install --assume-yes \
	locales locales-all \
	git \
	samba samba-common-bin \
	mpg123 \
	mplayer \
	vim \
	ssmtp mailutils \
	eyed3 \
	mediainfo \
	uuid
	
	
# ===-------------------------------------------------------------===
# LANGUAGE
# ===-------------------------------------------------------------===
export LANGUAGE=en_US.UTF-8
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8
export LC_TYPE=en_US.UTF-8


# ===-------------------------------------------------------------===
# CREATE DIRECTORIES
# ===-------------------------------------------------------------===
mkdir -p "${USER_HOME}/log"
mkdir -p "${USER_HOME}/media"
mkdir -p "${USER_HOME}/podcasts"
mkdir -p "${SOUNDS_HOME}"
mkdir -p "${SCRIPTS_HOME}"
mkdir -p "${DEVELOPER_HOME}"



# ===-------------------------------------------------------------===
# INSTALL PODGET
# ===-------------------------------------------------------------===
if ! [ -d "${DEVELOPER_HOME}/podget" ]; then
    cd "${DEVELOPER_HOME}"
    git clone https://github.com/dvehrs/podget.git podget
    cd podget
    make
    sudo make install
else
    echo "podget already installed"
fi


# ===-------------------------------------------------------------===
# Git Project
# ===-------------------------------------------------------------===

if ! [ -d "${GIT_PROJECT_HOME}" ]; then

    cd "${DEVELOPER_HOME}"
    git clone "${GIT_PROJECT}"
    sudo chown -R pi:pi "${GIT_PROJECT_HOME}"
    
    sudo ln --symbolic ${GIT_PROJECT_HOME}/bin/* /usr/local/bin
    sudo ln --symbolic ${GIT_PROJECT_HOME}/sounds/* "${SOUNDS_HOME}"
    sudo ln --symbolic ${GIT_PROJECT_HOME}/scripts/* "${SCRIPTS_HOME}"

    mkdir -p "${USER_HOME}/log"

    if ! [ -d "${USER_HOME}/.podget" ]; then
        cp -R "${GIT_PROJECT_HOME}/.podget" "${USER_HOME}"
    else
        echo "podget directory already existing"
    fi

    echo "removing and linking rc.local"
    sudo rm -rf /etc/rc.local
    sudo ln -s "${GIT_PROJECT_HOME}/rc.local" "/etc"
    sudo chown root:root "${GIT_PROJECT_HOME}/rc.local"

    echo "removing and linking asound.conf"
    sudo rm -rf /etc/asound.conf
    sudo ln -s "${GIT_PROJECT_HOME}/asound.conf" "/etc"
    sudo chown root:root "${GIT_PROJECT_HOME}/asound.conf"

    echo "removing and linking smb.conf"
    sudo rm -rf /etc/samba/smb.conf
    sudo ln -s "${GIT_PROJECT_HOME}/smb.conf" "/etc/samba"
    sudo chown root:root "${GIT_PROJECT_HOME}/smb.conf"

    echo "removing and linking ssmtp.conf"
    sudo rm -rf /etc/ssmtp/ssmtp.conf
    sudo ln -s "${GIT_PROJECT_HOME}/ssmtp.conf" "/etc/ssmtp"
    sudo chown root:root "${GIT_PROJECT_HOME}/ssmtp.conf"

else
	echo "git project already existing"
fi


cd "${SCRIPTS_HOME}"


sudo ln --symbolic ./wifi-down.sh button-blue-green-left.sh
sudo ln --symbolic ./wifi-up.sh button-blue-green-right.sh
sudo ln --symbolic ./play.sh button-blue.sh
sudo ln --symbolic ./volume-down.sh button-green-left.sh
sudo ln --symbolic ./volume-up.sh button-green-right.sh



# ===-------------------------------------------------------------===
# CLEANUP
# ===-------------------------------------------------------------===
sudo apt autoremove
sudo apt-get clean

rm -rf "${USER_HOME}/.gnupg"
rm -rf "${USER_HOME}/.local"
rm -rf "${USER_HOME}/.viminfo"
rm -rf "${USER_HOME}/.bash_history"
rm -rf "${USER_HOME}/.selected_editor"
rm -rf "${USER_HOME}/.wget-hsts"


# ===-------------------------------------------------------------===
#
# MANUAL SETUP
# ===-------------------------------------------------------------===

sudo chown -R pi:pi "${USER_HOME}"
sudo smbpasswd -a pi

# sudo vim /etc/ssh/ssh_config
# sudo vim /etc/ssh/sshd_config
```
