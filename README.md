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

```none
wsl -d Gentoo
```

Setup make.conf:

```none
cd /etc/portge \
rm make.conf \
wget https://raw.githubusercontent.com/brettcurtis/gentoo-wsl2/main/make.conf
```

Setup a few thangs':

```none
emerge --sync \
emerge --oneshot sys-apps/portage \
emerge sudo vim \
eselect vi set vim \
eselect editor set 3 \
. /etc/profile \
emerge -a --depclean
```

NOTE:

```none
 * Messages for package app-vim/gentoo-syntax-2:

 *
 * This plugin provides documentation via vim's help system. To
 * view it, use:
 *     :help gentoo-syntax
 *
 * This plugin makes use of filetype settings. To enable these,
 * add lines like:
 *     filetype plugin on
 *     filetype indent on
 * to your ~/.vimrc file.
 *
```

Setup user:

```none
useradd -m -G wheel brett \
passwd brett \
echo "brett ALL=(ALL) NOPASSWD:ALL" | EDITOR='tee -a' visudo
```

Setup wsl.conf

```none
cat << EOF >> /etc/wsl.conf
[user]
default=brett
EOF
```
Revert to package.use file vs directory:

```none
rm -rf /etc/portage/package.use \
touch /etc/portage/package.use
```

Setup CPU_FLAGS_*:

```none
emerge app-portage/cpuid2cpuflags \
echo "*/* $(cpuid2cpuflags)" > /etc/portage/package.use
```

Setup timezone:

```none
echo "America/New_York" > /etc/timezone \
emerge --config sys-libs/timezone-data
```

Locale generation:

```none
vi /etc/locale.gen \
locale-gen \
eselect locale set 6 \
env-update && source /etc/profile
```

Updating the @world set:

```none
emerge --ask --verbose --update --deep --newuse @world
```

Go make some ☕!

```none
emerge -a --depclean
```

Create an export from powershell:

```none
mkdir -p C:\WSL2
wsl --export Gentoo C:\WSL2\Gentoo.tar
```
