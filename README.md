# Essential Shell scripting for developers

- [Essential Shell scripting for developers](#essential-shell-scripting-for-developers)
  - [System Commands](#system-commands)
    - [Users \& Groups](#users--groups)
    - [Determining the version of operating system](#determining-the-version-of-operating-system)
    - [Viewing system log information](#viewing-system-log-information)
    - [Installing applications on Debian/Ubuntu Linux](#installing-applications-on-debianubuntu-linux)
    - [Getting process application IDs](#getting-process-application-ids)
    - [Managing system resources with TOP](#managing-system-resources-with-top)
    - [Finding processes by file \& Vice-Versa](#finding-processes-by-file--vice-versa)

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

```sh
ps aww
ps auxww
ps af
ps auxf
ps w -aC java

kill -9 15333
```

### Managing system resources with TOP

Linux load average is a metric that shows the number of tasks currently executed by the CPU and tasks waiting in the queue.
The metric is expressed as the average number of processes in a runnable state over the last 1, 5, or 15 minutes.

Fields Management for window 1:Def, whose current sort field is %CPU
Navigate with Up/Dn, Right selects for move then <Enter> or Left commits,
'd' or <Space> toggles display, 's' sets sort. Use 'q' or <Esc> to end!
use 'k' kill process.

```sh
top
```

### Finding processes by file & Vice-Versa

```sh
lsof file.db
fuser file.db
fuser -k file.db
ps ax | grep 62205
lsof -p 67367
```
