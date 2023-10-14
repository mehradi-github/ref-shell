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
    - [Scheduling jobs with CRON](#scheduling-jobs-with-cron)
  - [Networking](#networking)
    - [Commands for troubleshooting network issues](#commands-for-troubleshooting-network-issues)
    - [DNS Lookups](#dns-lookups)
    - [Secure remote operations with SSH](#secure-remote-operations-with-ssh)
    - [Setting Proxy](#setting-proxy)
    - [Testing web services with CURL](#testing-web-services-with-curl)

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

### Scheduling jobs with CRON

```sh
man cron
man -s5 crontab

crontab -e
# * * * * * date >> crontest.txt
tail -f crontest.txt
crontab --help
# examples:
 # use /bin/bash to run commands, instead of the default /bin/sh
       SHELL=/bin/bash
       # mail any output to `paul', no matter whose crontab this is
       MAILTO=paul
       #
       # run five minutes after midnight, every day
       5 0 * * *       $HOME/bin/daily.job >> $HOME/tmp/out 2>&1
       # run at 2:15pm on the first of every month — output mailed to paul
       15 14 1 * *     $HOME/bin/monthly
       # run at 10 pm on weekdays, annoy Joe
       0 22 * * 1-5    mail -s "It's 10pm" joe%Joe,%%Where are your kids?%
       23 0-23/2 * * * echo "run 23 minutes after midn, 2am, 4am ..., everyday"
       5 4 * * sun     echo "run at 5 after 4 every Sunday"
       0 */4 1 * mon   echo "run every 4th hour on the 1st and on every Monday"
       0 0 */2 * sun   echo "run at midn on every Sunday that's an uneven date"
       # Run on every second Saturday of the month
       0 4 8-14 * *    test $(date +\%u) -eq 6 && echo "2nd Saturday"

       All the above examples run non-interactive programs.  If  you  wish  to
       run  a  program that interacts with the user's desktop you have to make
       sure the proper environment variable DISPLAY is set.

       # Execute a program and run a notification every day at 10:00 am
       0 10 * * *  $HOME/bin/program | DISPLAY=:0 notify-send "Program run" "$(cat)"
```

## Networking

### Commands for troubleshooting network issues

```sh
netstat -in | sort -r -k5 | less
ifconfig | less
ping google.com
traceroute google.com
```

### DNS Lookups

```sh
dig amazon.com
dig amazon.com cname
nslookup amazon.com

```

### Secure remote operations with SSH

```sh
# Generating ssh key
ssh-keygen -f <filename> -t rsa -b 4096
ssh-keygen -f <filename> -t dsa
ssh-keygen -f <filename> -t ecdsa -b 521
ssh-keygen -f <filename> -t ed25519

# Removing Old host key in /home/$USER/.ssh/known_hosts
ssh-keygen -f "~/.ssh/known_hosts" -R "target-server"
cat ~/.ssh/known_hosts

#scp ~/.ssh/id_rsa.pub USER@<target-server>:/root/.ssh/uploaded_key.pub
#cat ~/.ssh/uploaded_key.pub >> ~/.ssh/authorized_keys
ssh-copy-id -i ~/.ssh/mykey USERNAME@<target-server>
cat ~/.ssh/authorized_keys

# Connetct to server via ssh
ssh USERNAME@<target-server>
ssh root@127.0.0.1

# Disabling to use a login and password to authenticate
vi /etc/ssh/sshd_config # passwordAthuntication no
service ssh restart
service sshd reload



```

### Setting Proxy

```sh
# Show proxy
printenv | grep -i proxy
unset all_proxy && unset ALL_PROXY
export all_proxy=socks5://127.0.0.1:20170/ && export ALL_PROXY=socks5://127.0.0.1:20170/
export http_proxy=http://127.0.0.1:20171/ && export HTTP_PROXY=http://127.0.0.1:20171/
export https_proxy=http://127.0.0.1:20171/ && export HTTPS_PROXY=http://127.0.0.1:20171/

# Setting Proxy for sudo
sudo visudo -f /etc/sudoers.d/NAME
# Write in above file
# Defaults env_keep += "no_proxy all_proxy NO_PROXY ALL_PROXY"
Defaults env_keep += "no_proxy NO_PROXY http_proxy HTTP_PROXY https_proxy HTTPS_PROXY"
source ~/.bashrc
```

### Testing web services with CURL

```sh
curl -H  "Content-Type: application/json" -s localhost:8080/v1/getdata -d '{"firstName":"jack", "salary":"100000"}' | jq

curl -s localhost:8080/v1/getdata | jq .[].salary | awk '{total+=$1} END {printf "$%\047.2f\n",total}'
# $420,000.00

curl -X DELETE localhost:8080/v1/getdata/5
```
