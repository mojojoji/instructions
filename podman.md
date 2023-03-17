# Podman

## Installation
https://podman.io/getting-started/installation#building-from-scratch

```
sudo apt-get install \
  btrfs-progs \
  crun \
  git \
  golang-go \
  go-md2man \
  iptables \
  libassuan-dev \
  libbtrfs-dev \
  libc6-dev \
  libdevmapper-dev \
  libglib2.0-dev \
  libgpgme-dev \
  libgpg-error-dev \
  libprotobuf-dev \
  libprotobuf-c-dev \
  libseccomp-dev \
  libselinux1-dev \
  libsystemd-dev \
  pkg-config \
  uidmap
```

### Install golang
```
sudo apt install golang-go
```

### Install conmon from source
```
git clone https://github.com/containers/conmon
cd conmon
export GOCACHE="$(mktemp -d)"
make
sudo make podman
```

### Install runc from source
```
git clone https://github.com/opencontainers/runc.git
cd $GOPATH/src/github.com/opencontainers/runc
make BUILDTAGS="selinux seccomp"
sudo cp runc /usr/bin/runc
```

### Setup CNI config
```
sudo mkdir -p /etc/containers
sudo curl -L -o /etc/containers/registries.conf https://src.fedoraproject.org/rpms/containers-common/raw/main/f/registries.conf
sudo curl -L -o /etc/containers/policy.json https://src.fedoraproject.org/rpms/containers-common/raw/main/f/default-policy.json
```

### Install app armour (optional)
```
sudo apt-get install libapparmor-dev
```

### Install podman

```
sudo apt install libdevmapper-dev
git clone https://github.com/containers/podman
cd podman
git checkout v4.4.2
make BUILDTAGS="selinux seccomp"
sudo make install PREFIX=/usr
```

### Change to runc
Checkpoint and restore is only supported in `runc`
Edit file 
```
sudo vim /usr/share/containers/containers.conf
```
Set runtime as `runc`
```
runtime = "runc"
```

# Checkpoint & Restore

## Checkpoint and restore
```
sudo podman run -dt -p 8080:80/tcp docker.io/library/httpd
sudo podman ps
sudo podman container checkpoint <container_id>
sudo podman container restore <container_id>
```

## Live migration
```
sudo podman container checkpoint <container_id> -e /tmp/checkpoint.tar.gz
sudo podman system prune -a
sudo podman container restore -i /tmp/checkpoint.tar.gz
sudo podman container restore -i /tmp/checkpoint.tar.gz --name testclone
```