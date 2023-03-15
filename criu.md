# CRIU

## Build from source

### Install dependencies
```
sudo apt update
sudo apt install \
    build-essential \
    pkg-config \
    libnet-dev \
    libaio-dev \
    libprotobuf-dev \
    libprotobuf-c-dev \
    protobuf-c-compiler \
    protobuf-compiler \
    python3-protobuf \
    python3-future \
    libnl-3-dev \
    libcap-dev \
    python3-yaml \
    libbsd-dev \
    libnftables-dev \
    asciidoctor
```

### Clone repo and build

```
git clone git@github.com:checkpoint-restore/criu.git
cd criu
git checkout v3.17.1
make clean
make
sudo make install
```

### Check installed version
```
criu --version
```