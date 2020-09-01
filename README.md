# containerd-workshop
Containerd basic tutorial using Ubuntu 18.04LTS

## Requirements
- Container = v1.4.0
- unzip

## Steps
### Step 1: download containerd sources
Download and set containerd source files
```
mkdir containerd-src
cd containerd-src
wget https://github.com/containerd/containerd/archive/v1.4.0.zip
apt-get install unzip
unzip v1.4.0.zip
wget https://github.com/containerd/containerd/releases/download/v1.4.0/containerd-1.4.0-linux-amd64.tar.gz
tar xvf containerd-1.4.0-linux-amd64.tar.gz
```
Copy containerd binary files to /user/local/bin
```
mv bin/* /usr/local/bin
```

### Step 2: create containerd etc and others configurations
create file and directory
```
mkdir /etc/containerd
```
Add this content to /etc/containerd/config.toml  
```
subreaper = true
oom_score = -999

[debug]
        level = "debug"

[metrics]
        address = "127.0.0.1:1338"

[plugins.linux]
        runtime = "runc"
        shim_debug = true
```
copy containerd.service located at containerd-1.4.0 to /etc/systemd/system

```
cp containerd-1.4.0/containerd.service /etc/systemd/system
chmod 644 /etc/systemd/system/containerd.service
systemctl enable containerd
systemctl start containerd
systemctl status containerd
```

### Step3: Install Golang
Download Golang:   
- https://golang.org/dl 

Download and extract sources
```
wget https://golang.org/dl/go1.15.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.15.linux-amd64.tar.gz
```
Set enviroment variables and add this to your ~/.profile
```
mkdir $HOME/go
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/go
. ~/.profile
mkdir $GOPATH/src
```

### Step4: Installing RunC
Install necesary libraries
```
apt-get install -y build-essential
apt-get install -y libseccomp-dev
apt-get install -y pkg-config
```
Compile and install RunC
```
go get github.com/opencontainers/runc
cd $GOPATH/src/github.com/opencontainers/runc
make
sudo make install
```
RunC will be installed on:  
/usr/local/sbin/runc

### Step5: Run Containerd Examples
Install go libreries to compile the example
```
go get github.com/containerd/containerd
go get github.com/containerd/containerd/cio
go get github.com/containerd/containerd/oci
go get github.com/containerd/containerd/namespaces
```
Then compile it and run it
```
go build mycontainerd.go
./mycontainerd.go
```
## References
- https://containerd.io/docs/getting-started
- https://github.com/opencontainers/runc  
  
Visit that awesome CNCF Projects!
