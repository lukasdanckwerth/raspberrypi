# initialize a new raspberry pi

## local configuration

```
sudo raspi-config
```

 * set new password
 * set new hostname
 * expand filesystem

## update & upgrade system

```
sudo apt-get update -y && sudo apt-get upgrade -y && sudo apt-get install -y \
    vim \
    git
```

## disable ssh local environment
```
sudo vim /etc/ssh/ssh_config

# comment out following line
SendEnv LANG LC_*
```
