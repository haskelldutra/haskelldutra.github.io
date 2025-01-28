+++
title = "Migrating from FreeBSD RELEASE to CURRENT"
date = "2024-06-02"
+++

Migrating from FreeBSD RELEASE to CURRENT without compiling.

<!--more-->

## RELEASE neofetch

![](https://haskelldutra.github.io/freebsd-release.png)

## Pkgbase

The pkgbase distribute the FreeBSD OS in packages. This allows you to update your operating system using
just the pkg.

### Creating the repository

We need to add a new repository to download the base system.

```
cat <<EOF >/etc/pkg/base.conf

base: {
  url: "pkg+https://pkg.FreeBSD.org/\${ABI}/base_latest",
  mirror_type: "srv",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}
EOF
```

### Updating the repository

```
pkg update
```

### Install base packages

```
env IGNORE_OSVERSION=yes ABI=FreeBSD:15:amd64 pkg install -r base -g 'FreeBSD-*'
```

### Restore files

```
cp /etc/master.passwd.pkgsave /etc/master.passwd
cp /etc/group.pkgsave /etc/group
pwd_mkdb -p /etc/master.passwd
cp /etc/sysctl.conf.pkgsave /etc/sysctl.conf
```

### Deleting old files

```
find / -name \*.pkgsave -delete
rm /boot/kernel/linker.hints
```

![](https://haskelldutra.github.io/freebsd-current.png)

## Reference

https://wiki.freebsd.org/PkgBase
