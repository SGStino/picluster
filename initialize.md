# INSTALL

## pin docker version for armv6 devices (optional)
edit apt-get preferences:
```sudo nano /etc/apt/preferences.d/docker-ce```
add the following:
```
Package: docker-ce
Pin: version 18.06.1*
Pin-Priority: 1000
```
## setup network interface
setup vlan for usb0: 
edit the network interface config
```sudo nano /etc/network/interfaces.d/usb0-vlan0```

with the following context
```
auto usb0:0
iface usb0:0 inet static
  address 172.0.0.14
  netmask 255.255.255.0
```
restart networking
```sudo service networking restart```

## docker
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```
enable the current user to use docker without sudo
```sudo usermod -aG docker $USER```
### initialize swarm
```
docker swarm init
```
### join swarm
join docker into the swarm
```
docker swarm join --token SWMTKN-1-4ged201jpxg3qnpbazmqkzukcrrr39pa5nru32abj3xpefajbp-3tu4pb9968l1d2oc2xcgu5f2d 172.0.0.1:2377
```
## setup nfs share
run on the server with the sata drive: (172.0.0.4)

```
docker run -d --name nfs-server   \
-p 111:111                        \
-p 111:111/udp                    \
-p 2049:2049                      \
-p 2049:2049/udp                  \
-p 32765:32765                    \
-p 32765:32767                    \
-p 32765:32765/udp                \
-p 32765:32767/udp                \
-v /mnt/sata/nfs/share:/nfsexport \ 
-e "NFS_EXPORT_0=/nfsexport *(rw,sync,no_subtree_check,insecure)"
```

## setup portainer
deploy [portainer-markdown.yml](portainer-markdown.yml):
```docker stack deploy --compose-file=portainer-compose.yml portainer```
