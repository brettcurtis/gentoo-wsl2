<img src="https://user-images.githubusercontent.com/1610100/194944104-53a1b3f0-81c5-4759-835d-9b3a8608f38e.png" width=25% height=25%>

# Configuration for Gentoo on WSL2

Open up powershell and download the nomultilib stage3 tarball:

```none
curl -L -o stage3-amd64-nomultilib-openrc.tar.xz https://bouncer.gentoo.org/fetch/root/all/releases/amd64/autobuilds/20221002T170543Z/stage3-amd64-nomultilib-openrc-20221002T170543Z.tar.xz
```

Create a directory for registration:

```none
mkdir -p C:\Users\brett-home\AppData\Local\WSL\Gentoo
```

Import downloaded file:

```none
wsl --import Gentoo C:\Users\brett-home\AppData\Local\WSL\Gentoo\ .\stage3-amd64-nomultilib-openrc.tar.xz --version 2
```

Enter the Gentoo distribution on WSL by using the command:

``none
wsl -d Gentoo
```

Setup make.conf:

```none
cd /etc/portge \
rm make.conf \
wget https://raw.githubusercontent.com/brettcurtis/gentoo-wsl2/main/make.conf
```
