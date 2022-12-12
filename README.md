# <img align="left" width="45" height="45" src="https://user-images.githubusercontent.com/1610100/194944104-53a1b3f0-81c5-4759-835d-9b3a8608f38e.png"> Configuration for Gentoo on WSL2

Open up powershell and download the nomultilib stage3 tarball:

```none
curl -o stage3-amd64-nomultilib-openrc.tar.xz https://bouncer.gentoo.org/fetch/root/all/releases/amd64/autobuilds/20221204T163153Z/stage3-amd64-nomultilib-systemd-20221204T163153Z.tar.xz
```

Create a directory for registration:

```none
mkdir -p C:\Users\brett\AppData\Local\WSL\Gentoo
```

Import downloaded file:

```none
wsl --import Gentoo C:\Users\brett\AppData\Local\WSL\Gentoo\ .\stage3-amd64-nomultilib-openrc.tar.xz --version 2
```

Enter the Gentoo distribution on WSL by using the command:

```none
wsl -d Gentoo
```

Setup make.conf:

```none
cd /etc/portage
```

```none
rm make.conf
```

```none
wget https://raw.githubusercontent.com/brettcurtis/gentoo-wsl2/main/etc/portage/make.conf
```

```none
emerge resolve-march-native
```

Setup package.use:

```none
rm -rf /etc/portage/package.use
```

```none
touch /etc/portage/package.use
```

```none
wget https://raw.githubusercontent.com/brettcurtis/gentoo-wsl2/main/etc/portage/package.use
```

Setup a few thangs':

```none
emerge app-eselect/eselect-repository dev-vcs/git
```

```none
eselect repository add gentoo git https://github.com/gentoo-mirror/gentoo.git
```

```none
rm -rf /var/db/repos/gentoo
```

```none
emerge --sync
```

```none
emerge --oneshot sys-apps/portage
```

```none
emerge sudo vim
```

```none
eselect editor set 3
```

```none
. /etc/profile
```

```none
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
useradd -m -G wheel brett
```

```none
passwd brett
```

```none
echo "brett ALL=(ALL) NOPASSWD:ALL" | EDITOR='tee -a' visudo
```

Setup wsl.conf

```none
cat << EOF >> /etc/wsl.conf
[user]
default=brett
EOF
```

Setup CPU_FLAGS_*:

```none
emerge app-portage/cpuid2cpuflags
```

```none
echo "*/* $(cpuid2cpuflags)" > /etc/portage/package.use
```

Setup timezone:

```none
echo "America/New_York" > /etc/timezone
```

```none
emerge --config sys-libs/timezone-data
```

Locale generation:

```none
vi /etc/locale.gen
```

```none
locale-gen
```

```none
eselect locale set 6
```

```none
env-update && source /etc/profile
```

Updating the @world set:

```none
emerge --ask --verbose --update --deep --newuse @world
```

Go make some ☕!

Cleanup:

```none
emerge -a --depclean
```

Create an export from powershell:

```none
mkdir -p C:\WSL2
```

```none
wsl --export Gentoo C:\WSL2\Gentoo.tar
```
