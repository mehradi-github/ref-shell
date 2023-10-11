# Essential Shell scripting for developers

- [Essential Shell scripting for developers](#essential-shell-scripting-for-developers)
  - [System Commands](#system-commands)
    - [Users \& Groups](#users--groups)
    - [Determining the version of operating system](#determining-the-version-of-operating-system)
    - [Viewing system log information](#viewing-system-log-information)
    - [Installing applications on Debian/Ubuntu Linux](#installing-applications-on-debianubuntu-linux)
    - [Getting process application IDs](#getting-process-application-ids)

## System Commands

### Users & Groups

```sh
less /etc/passwd
sudo less shadow
```

### Determining the version of operating system

```sh
lsb_release -a
uname -a
```

### Viewing system log information

```sh
sudo dmesg | less
```

### Installing applications on Debian/Ubuntu Linux

```sh
apt search openjdk-17 | less
sudo apt install tree
apt list --installed
apt list --upgradeable
sudo apt upgrade
```

### Getting process application IDs
