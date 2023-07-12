# Singularity

## Installing Singularity

Install system dependencies with the commands:
```
sudo apt install git
```
```
sudo apt-get update
```
```
sudo apt-get install -y build-essential libseccomp-dev pkg-config squashfs-tools cryptsetup
```

Install GoLang with the commands:
```
export VERSION=1.18.2 OS=linux ARCH=amd64
```
```
wget -O /tmp/go${VERSION}.${OS}-${ARCH}.tar.gz https://dl.google.com/go/go${VERSION}.${OS}-${ARCH}.tar.gz
```
```
sudo tar -C /usr/local -xzf /tmp/go${VERSION}.${OS}-${ARCH}.tar.gz 
```

```
echo 'export GOPATH=${HOME}/go' >> ~/.bashrc
```
```
source ~/.bashrc
```
```
sudo apt install golang-go
```
To check that it is installed do the command
```
go version
```


Clone Singularity
```
mkdir -p ${GOPATH}/src/github.com/hpcng
```
``` 
cd ${GOPATH}/src/github.com/hpcng
```
``` 
git clone https://github.com/hpcng/singularity.git
```
```
cd singularity 
```

Check out the [release tag](https://github.com/hpcng/singularity/releases) before compiling, at the time of writing the current release is 3.8.7
```
git checkout v3.8.7
```
```
cd ${GOPATH}/src/github.com/hpcng/singularity
```
```
./mconfig
```
```
cd ./builddir
```
```
make
```
```
sudo make install 
```

Check the version by: 
```
singularity --version
```

## Installing Singularity-Compose
```
pip install singularity-compose
```


## Resources

- [Installation Guide](https://sylabs.io/guides/3.0/user-guide/installation.html)
- [Singularity](https://github.com/hpcng/singularity)
