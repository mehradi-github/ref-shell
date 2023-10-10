# Essential Shell scripting for developers

- [Essential Shell scripting for developers](#essential-shell-scripting-for-developers)
  - [System Commands](#system-commands)
    - [Users \& Groups](#users--groups)
    - [Determining the version of operating system](#determining-the-version-of-operating-system)
    - [Viewing system log information](#viewing-system-log-information)

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
