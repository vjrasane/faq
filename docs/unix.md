# Unix

## MacOS

### NPM trust CA

```shell
curl https://curl.haxx.se/ca/cacert.pem > ~/.npm.certs.pem
npm config set cafile ~/.npm.certs.pem
cat /path/to/ca >> .npm.certs.pem
```

## Ubuntu

### Run command as daemon

```shell
start-stop-daemon -SbCv -x <command>
```

> NOTE: All paths must be absolute 

### Allow multicast DNS for `.local` domains

Edit `/etc/nsswitch.conf`

```
#hosts:          files mdns4_minimal [NOTFOUND=return] dns
hosts:          files dns
```

https://askubuntu.com/questions/81797/nslookup-finds-ip-for-a-hostname-in-local-domain-but-ping-does-not

### Install Yarn

```shell
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update && sudo apt install yarn
```

### Add Docker CA

```shell
mkdir -p /etc/docker/certs.d/<hostname>
cp ./path/to/ca /etc/docker/certs.d/<hostname>/ca.crt 
systemctl restart docker
```

### Recursive search pattern in files 

```shell
grep -rnw . -e <pattern>
```

### Get Kernel version

```shell
uname -r
```

### Get distro

```shell
cat /etc/os-release
```

## Centos

### Change docker data dir

Add to `/usr/lib/systemd/system/docker.service`

```shell
-g /path/to/data/dir
```

E.G.

```shell
ExecStart=/usr/bin/dockerd-current \

          -g /data/docker \

          --add-runtime docker-runc=/usr/libexec/docker/docker-runc-current \

          --default-runtime=docker-runc \

          --exec-opt native.cgroupdriver=systemd \

          --userland-proxy-path=/usr/libexec/docker/docker-proxy-current \

          --init-path=/usr/libexec/docker/docker-init-current \

          --seccomp-profile=/etc/docker/seccomp.json \

          $OPTIONS \

          $DOCKER_STORAGE_OPTIONS \

          $DOCKER_NETWORK_OPTIONS \

          $ADD_REGISTRY \

          $BLOCK_REGISTRY \

          $INSECURE_REGISTRY \

          $REGISTRIES
```

### Install Node on Centos 7 docker

```dockerfile
RUN curl -sL https://rpm.nodesource.com/setup_14.x | bash

RUN yum -y install nodejs
RUN node --version
RUN npm --version
```