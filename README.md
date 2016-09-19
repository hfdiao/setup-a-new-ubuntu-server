# setup-a-new-ubuntu-server

```sh
SKIP_SET_HOST_NAME=0
SKIP_INSTALL_VIM=0
SKIP_INSTALL_DOCKER=0
NEED_REBOOT=1

UBUNTU_CODENAME=`lsb_release -c -s`

# gen locale
locale-gen zh_CN.UTF-8 en_US.UTF-8

# set apt repos
cat <<EOF > /etc/apt/sources.list
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_CODENAME} main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_CODENAME}-backports main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_CODENAME}-proposed main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_CODENAME}-security main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_CODENAME}-updates main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_CODENAME} main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_CODENAME}-backports main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_CODENAME}-proposed main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_CODENAME}-security main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_CODENAME}-updates main multiverse restricted universe
EOF

# update apt cache
apt-get update

# install vim
if [ $SKIP_INSTALL_VIM -eq 0 ]; then
    apt-get install -y --no-install-recommends vim
fi

# modify hostname
NEW_HOSTNAME=${1:-""}
if [ $SKIP_SET_HOST_NAME -eq 0 -a ${#NEW_HOSTNAME} -gt 0 ]; then
    NEED_REBOOT=1
    OLD_HOSTNAME=`cat /etc/hostname`

    echo "setting hostname to ${NEW_HOSTNAME}"
    
    sed -i -- "s/$OLD_HOSTNAME/$NEW_HOSTNAME/g" /etc/hosts
    cat <<EOF > /etc/hostname
    $NEW_HOSTNAME
EOF
else
    NEED_REBOOT=0
fi

# install docker
if [ $SKIP_INSTALL_DOCKER -eq 0 ]; then
    apt-get install -y --no-install-recommends apt-transport-https ca-certificates
    apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
    cat <<EOF > /etc/apt/sources.list.d/docker.list
    deb https://apt.dockerproject.org/repo ubuntu-${UBUNTU_CODENAME} main
EOF

    apt-get update
    apt-get install -y --no-install-recommends linux-image-extra-$(uname -r)
    apt-get install -y --no-install-recommends apparmor
    apt-get install -y --no-install-recommends docker-engine
fi

# reboot
if [ $NEED_REBOOT -eq 1 ]; then
    reboot
fi
```
