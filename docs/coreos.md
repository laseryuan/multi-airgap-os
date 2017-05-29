## On CoreOS Enable ssh
curl https://github.com/laseryuan.keys | update-ssh-keys -a core

## Login
ssh-keygen -f "/home/laser/.ssh/known_hosts" -R [localhost]:3022
ssh -p 3022 -A core@localhost

## Bootstrap
mkdir /media/livecd
lsblk
sudo mount /dev/sda2 /media/livecd
/media/livecd/bootstrap.sh

docker run -it busybox sh

## Install docker compose
curl -L --fail https://github.com/docker/compose/releases/download/1.13.0/run.sh > /opt/bin/docker-compose
chmod +x /opt/bin/docker-compose
