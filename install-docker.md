## 0. Install via script
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/lukasdanckwerth/raspberrypi/master/install-docker.sh)"
```

## 1. Install Docker
```shell
curl -sSL https://get.docker.com | sh
```

## 2. Add permission to Pi User to run Docker Commands
```shell
sudo usermod -aG docker pi
```

## 3. IMPORTANT! Install proper dependencies
```shell
sudo apt-get install -y \
  libffi-dev \
  libssl-dev \
  python3 \
  python3-pip

sudo apt-get remove python-configparser
```

## 4. Install Docker Compose
```shell
sudo pip3 install docker-compose
```

> Reboot
