# setup-a-new-ubuntu-server

```sh
# gen locale
locale-gen zh_CN.UTF-8 en_US.UTF-8

# set apt repos
cat <<EOF > /etc/apt/sources.list
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-backports main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-proposed main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-security main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-updates main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-backports main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-proposed main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-security main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-updates main multiverse restricted universe
EOF

# update apt cache
apt-get update

# install vim
apt-get install vim

# modify hostname
NEW_HOSTNAME=docker-registry
OLD_HOSTNAME=`cat /etc/hostname`
sed -i -- "s/$OLD_HOSTNAME/$NEW_HOSTNAME/g" /etc/hosts
cat <<EOF > /etc/hostname
$NEW_HOSTNAME
EOF
```
