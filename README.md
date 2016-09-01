# setup-a-new-ubuntu-server

```sh

NEW_HOSTNAME=my_new_host
OLD_HOSTNAME=`cat /etc/hostname`
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
apt-get install vim

# modify hostname
sed -i -- "s/$OLD_HOSTNAME/$NEW_HOSTNAME/g" /etc/hosts
cat <<EOF > /etc/hostname
$NEW_HOSTNAME
EOF

# install docker
apt-get install apt-transport-https ca-certificates
apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
cat <<EOF > /etc/apt/sources.list.d/docker.list
deb https://apt.dockerproject.org/repo ubuntu-${UBUNTU_CODENAME} main
EOF

apt-get update
apt-get install -y linux-image-extra-$(uname -r)
apt-get install apparmor
apt-get install -y docker-engine


# reboot
reboot
```
