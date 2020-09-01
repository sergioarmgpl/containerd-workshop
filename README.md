# containerd-workshop
Containerd basic tutorial using Ubuntu 18.04LTS

## Requirements
- Container = v1.4.0
- unzip

## steps
### step 1: download containerd sources
mkdir containerd-src
cd containerd-src
wget https://github.com/containerd/containerd/archive/v1.4.0.zip
apt-get install unzip
unzip v1.4.0.zip
wget https://github.com/containerd/containerd/releases/download/v1.4.0/containerd-1.4.0-linux-amd64.tar.gz
tar xvf containerd-1.4.0-linux-amd64.tar.gz

### step 2: create containerd etc configuration
create file and directory
mkdir /etc/containerd


Add this content to /etc/containerd/config.toml  

subreaper = true
oom_score = -999

[debug]
        level = "debug"

[metrics]
        address = "127.0.0.1:1338"

[plugins.linux]
        runtime = "runc"
        shim_debug = true


mv bin/* /usr/local/bin

copy containerd.service located at containerd-1.4.0 to /etc/systemd/system

cp containerd-1.4.0/containerd.service /etc/systemd/system

chmod 644 /etc/systemd/system/containerd.service
systemctl enable containerd
systemctl start containerd
systemctl status containerd

## Install Golang
Download Golang
https://golang.org/dl/

wget https://golang.org/dl/go1.15.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.15.linux-amd64.tar.gz

mkdir $HOME/go

Add this to your ~/.profile
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/go
. ~/.profile
mkdir $GOPATH/src


## Installing RunC
https://github.com/opencontainers/runc
apt-get install -y build-essential
apt-get install -y libseccomp-dev
apt-get install -y pkg-config

go get github.com/opencontainers/runc
cd $GOPATH/src/github.com/opencontainers/runc
make
sudo make install

RunC will be install on:
/usr/local/sbin/runc

## Run Containerd Example
go get github.com/containerd/containerd
go get github.com/containerd/containerd/cio
go get github.com/containerd/containerd/oci
go get github.com/containerd/containerd/namespaces
