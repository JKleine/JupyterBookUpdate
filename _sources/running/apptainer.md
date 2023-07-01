# Apptainer

Apptainer is the new version of Singularity. Directions are based on a Mint 21.1 VM with Miniconda installed.

## Installing Apptainer

To install Apptainer on a new Linux VM:

Ensure repositories are up-to-date
```
sudo apt-get update
```
Install debian packages for dependencies
```
sudo apt-get install -y \
    build-essential \
    libseccomp-dev \
    pkg-config \
    uidmap \
    squashfs-tools \
    squashfuse \
    fuse2fs \
    fuse-overlayfs \
    fakeroot \
    cryptsetup \
    curl wget git
```
Install Go
```
export GOVERSION=1.19.6 OS=linux ARCH=amd64  
```

```
wget -O /tmp/go${GOVERSION}.${OS}-${ARCH}.tar.gz \
  https://dl.google.com/go/go${GOVERSION}.${OS}-${ARCH}.tar.gz
sudo tar -C /usr/local -xzf /tmp/go${GOVERSION}.${OS}-${ARCH}.tar.gz
```
```
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc
```
Install golangci-lint
```
curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $(go env GOPATH)/bin v1.51.1
```
Clone the repo
```
git clone https://github.com/apptainer/apptainer.git
cd apptainer
```
Configure build and install apptainer
```
./mconfig
cd ./builddir
make
sudo make install
```
Confirm installation
```
apptainer --version
```


