#INSTALL

## pin docker version for armv6 devices
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

##install docker
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```
enable the current user to use docker without sudo
```sudo usermod -aG docker $USER```

join docker into the swarm
```
docker swarm join --token SWMTKN-1-4ged201jpxg3qnpbazmqkzukcrrr39pa5nru32abj3xpefajbp-3tu4pb9968l1d2oc2xcgu5f2d 172.0.0.1:2377
```