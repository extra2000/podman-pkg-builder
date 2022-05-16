# podman-pkg-builder

| License | Versioning | Build |
| ------- | ---------- | ----- |
| [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) | [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release) | [![Build status](https://ci.appveyor.com/api/projects/status/20vkrxo2jv0wmp5y/branch/master?svg=true)](https://ci.appveyor.com/project/nikAizuddin/podman-pkg-builder/branch/master) |

Podman RPM builder.


## Clone

Clone Podman and `rpkg` repositories:
```
git clone -b v3.4.7 https://github.com/extra2000/podman.git src/podman
git clone https://pagure.io/rpkg-util.git src/rpkg
```

Allow `./src` and `./rpms` directories to be mounted into container:
```
chcon -R -v -t container_file_t ./src ./rpms
```


## Building Podman builder image

```
cd src
podman build -t extra2000/podman-pkg-builder .
```


## Building Podman using the builder image

Build:
```
podman run -it --rm -v ./src/podman:/opt/podman:rw -v ./rpms:/root/rpmbuild/RPMS:rw --workdir /opt/podman localhost/extra2000/podman-pkg-builder bash
make clean
make package
```

The output RPM files can be found in `./rpms`. Optionally, GPG sign the RPM files:
```
rpm --addsign ./rpms/x86_64/*.rpm
```

Test RPM installation:
```
podman run -it --rm -v ./rpms/:/opt/rpms:ro quay.io/almalinux/almalinux:8.5 bash
dnf install -y /opt/rpms/x86_64/*.rpm
podman version
podman info
```
