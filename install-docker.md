## 1. Install Docker
```
curl -sSL https://get.docker.com | sh
```

## 2. Add permission to Pi User to run Docker Commands
```
sudo usermod -aG docker pi
```

## 3. IMPORTANT! Install proper dependencies
```
sudo apt-get install -y \
  libffi-dev \
  libssl-dev \
  python3 \
  python3-pip

sudo apt-get remove python-configparser
```

## 4. Install Docker Compose
```
sudo pip3 install docker-compose
```

> Reboot
