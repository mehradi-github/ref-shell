# Essential Shell scripting for developers

- [Essential Shell scripting for developers](#essential-shell-scripting-for-developers)
  - [The Basic](#the-basic)
    - [Shows command descriptions](#shows-command-descriptions)
    - [Operators](#operators)
    - [Redirection \& Piping](#redirection--piping)
  - [Text commands](#text-commands)
    - [AWK command](#awk-command)
    - [SED command](#sed-command)
    - [Comparing text files for differences](#comparing-text-files-for-differences)
    - [Converting characters of text](#converting-characters-of-text)
    - [WC command](#wc-command)
  - [System Commands](#system-commands)
    - [systemctl](#systemctl)
    - [Users and Groups](#users-and-groups)
    - [Assigning Sudo Rights to a user](#assigning-sudo-rights-to-a-user)
    - [hardware info](#hardware-info)
    - [Delete and rename and move file](#delete-and-rename-and-move-file)
    - [Determining the version of operating system](#determining-the-version-of-operating-system)
    - [Linux system shutdown and reboot](#linux-system-shutdown-and-reboot)
    - [Viewing system log information](#viewing-system-log-information)
    - [Installing applications on Debian/Ubuntu Linux](#installing-applications-on-debianubuntu-linux)
    - [Getting process application IDs](#getting-process-application-ids)
    - [Managing system resources with TOP](#managing-system-resources-with-top)
    - [Finding processes by file \& Vice-Versa](#finding-processes-by-file--vice-versa)
    - [Creating job and kill process](#creating-job-and-kill-process)
    - [Scheduling jobs with CRON](#scheduling-jobs-with-cron)
    - [Changing the Time Zone in Linux via ssh](#changing-the-time-zone-in-linux-via-ssh)
  - [Networking](#networking)
    - [Commands for troubleshooting network issues](#commands-for-troubleshooting-network-issues)
    - [DNS Lookups](#dns-lookups)
    - [Secure remote operations with SSH](#secure-remote-operations-with-ssh)
    - [Enable or disable remote root login](#enable-or-disable-remote-root-login)
    - [Setting Proxy](#setting-proxy)
    - [Testing web services with CURL](#testing-web-services-with-curl)
    - [Grabing an ip automatically from DHCP](#grabing-an-ip-automatically-from-dhcp)
    - [Display all interfaces which are currently available, even if down](#display-all-interfaces-which-are-currently-available-even-if-down)
    - [What Is My IP Address?](#what-is-my-ip-address)
  - [Utility commands](#utility-commands)
    - [Archiving files \& directories](#archiving-files--directories)
    - [Executing dynamic commands (xargs)](#executing-dynamic-commands-xargs)
    - [Searching for files](#searching-for-files)

## The Basic

```sh

# CTRL+L
clear

```

### Shows command descriptions

```sh
whatis <COMMAND>
man <COMMAND>

# help <COMMAND>
<COMMAND> --help
<COMMAND> -h
```

### Operators

```sh
#The Redirection Operators (>, >>, <)
# re-writing the file
echo "abc" > test.txt

# append to a file
echo "defg" >> test.txt

(ls *.txt > txt-files.list && cp *.tx ~) && (ls *.rpm > rpm-packages.list && cp *.rpm ~) || echo "Precedence Test!"

# The Ampersand Operator (&): execute that Linux command in the background
gedit &

# The Semicolon Operator (;): execute commands in a defined, sequential order
pwd ; mkdir test ; cd test ; touch file

# The OR Operator (||): execute the command that follows only if the preceding command fails
bad_command || ls

# The Pipe Operator (|): directs the output of the preceding command as input to the succeeding command
cat test | grep -i "makeuseof"

# The AND Operator (&&): execute commands only if the preceding command was successfully executed
pwd && mkdir test && cd test && bad_command && ls
```

### Redirection & Piping

```sh
ls -l | grep sam | awk '{print $9}'
find ./ -name "junk" 2> /dev/null > output.txt

cat < output.txt > other.txt
(cat < output.txt) > other.txt

cat << EOF >>  other.txt
> num,date
> 1,01/01/2023
> 2,01/01/2022
> 3,01/01/2021
> 4,01/01/2020
> 5,01/01/2019
EOF

wc other.txt
cat other.txt | sort -g
head -n5 other.txt | tail -n+2

head -n4 other.txt | tail -n+2 | sort -r -t "/" -k 3

```

## Text commands

### AWK command

Awk is a scripting language used for manipulating data and generating reports.

```sh
# Print the lines which match the given pattern
awk '/manager/ {print}' employee.txt
# Output:
# ajay manager account 45000
# varun manager sales 50000

# Splitting a Line Into Fields
awk '{print $1,$4}' employee.txt
# ajay 45000
# sunil 25000

# Built-In Variables
# Display Line Number
awk '{print NR,$0}' employee.txt
# 1 ajay manager account 45000
# 2 sunil clerk account 25000

# Display Last Field
awk '{print $1,$NF}' employee.txt

# Display Line From 3 to 6
awk 'NR==3, NR==6 {print NR,$0}' employee.txt

```

### SED command

stream editor for filtering and transforming text

```sh
# s: s/regexp/replacement/
sed -i -e 's/,/:/g' other.txt

head -n4 other.txt | sed 's/,/:/g'

head -n4 other.txt | sed 's@/@#@g'
#OR
head -n4 other.txt | sed 's/\//#/g'

# Delete range line 2 to 4.
sed '2,4d' other.txt
# Delete any expect range line 2 to 4.
sed '2,4!d' other.txt
# Multiple run command
sed -e '2,4!d'  -e 's/,/:/g' other.txt

# Using regexp-extended convert date format from 11/01/2023 to 2023-11-1.
sed -E -e '2,4!d'  -e 's/,/:/g' -e 's#([[:digit:]]{1,2})/([[:digit:]]{1,2})/([[:digit:]]{4})#\3-\1-\2#g' other.txt

```

### Comparing text files for differences

```sh
diff file1 file2 > patch
patch file1 patch
diff -y file1 file2
```

### Converting characters of text

```sh
cat file3 | tr [:lower:] [:upper:] > upper.txt
head -n4 other.txt | tail -n+2 | tr ',' ':'
```

### WC command

Find out number of lines, word count, byte and characters count

```sh
wc -l state.txt capital.txt
#   5 state.txt
#   5 capital.txt
#  10 total

wc -w state.txt
# 7 state.txt

wc -m state.txt
# 56 state.txt

apt list installed | wc -l
```

## System Commands

### systemctl

```sh
sudo systemctl daemon-reload

sudo systemctl enable --now jenkins
sudo systemctl status jenkins
```

### Users and Groups

```sh

whoami
id
su - USER

useradd [options] USERNAME
passwd USERNAME

# Change password
sudo passwd
sudo less shadow

# User List
cat /etc/passwd
# Group list
cat /etc/group

```

### Assigning Sudo Rights to a user

```sh
usermod -aG wheel USERNAME
groups USERNAME

#Remove a user from a group
sudo gpasswd -d USERNAME wheel

# Edit the sudoers file
visudo
/ALL
USERNAME    ALL=(ALL)       ALL
# /etc/security/limits.conf or /etc/security/limits.d/90-nproc.conf
<user>       -          nproc     2048      <<<----[ Only for "<user>" user ]
```

### hardware info

```sh
cat /proc/cpuinfo
lscpu
```

### Delete and rename and move file

```sh
mv -t DESTINATION file1 file2 file3
mv ./old-name.txt ./new-name.txt
rm -f name*
```

### Determining the version of operating system

```sh
cat /etc/os-release
uname -a

lsb_release -a
neofetch

```

### Linux system shutdown and reboot

```sh
sudo shutdown -p now
shutdown -h now
shutdown -h +0
sudo poweroff

sudo shutdown -r now
sudo reboot
```

### Viewing system log information

```sh
sudo dmesg | less
```

### Installing applications on Debian/Ubuntu Linux

```sh
apt search openjdk-17 | less
sudo apt install tree -y

apt list --installed
apt list --upgradeable
sudo apt upgrade


apt list installed > my_list.txt
apt list installed | egrep -i <NAME>

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

### Creating job and kill process

```sh
watch date
ps
kill -l
kill -9 NUM

# list open files
sudo lsof /etc/sudoers
kill -15 PID

```

### Scheduling jobs with CRON

```sh
crontab -e
* * * * * date >> crontest.txt
tail -f crontest.txt


man cron
man -s5 crontab

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

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

### Changing the Time Zone in Linux via ssh

```sh
ssh root@IP timedatectl
ssh root@IP timedatectl list-timezones
ssh root@IP timedatectl set-timezone Europe/Berlin
ssh root@IP timedatectl
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

ssh-keyscan -H agent1 >> /var/lib/jenkins/.ssh/known_hosts

```

### Enable or disable remote root login

```sh
/etc/ssh/sshd_config:
	PermitRootLogin yes #enabled
  PermitRootLogin no #disabled
```

### Setting Proxy

```sh
# Show proxy
printenv | grep -i proxy
# set proxy
export all_proxy=socks5://192.168.1.34:1089/ && export ALL_PROXY=socks5://192.168.1.34:1089/
export http_proxy=http://192.168.1.34:8889/ && export HTTP_PROXY=http://192.168.1.34:8889/
export https_proxy=http://192.168.1.34:8889/ && export HTTPS_PROXY=http://192.168.1.34:8889/
export NO_PROXY=localhost,192.168.1.34,172.17.0.1,172.17.0.2 && export no_proxy=localhost,192.168.1.34,172.17.0.1,172.17.0.2
# unset
unset all_proxy && unset ALL_PROXY && unset http_proxy && unset HTTP_PROXY && unset https_proxy && unset HTTPS_PROXY && unset NO_PROXY && unset no_proxy

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

### Grabing an ip automatically from DHCP

```sh
dhclient -v

hostnamectl
```

### Display all interfaces which are currently available, even if down

```sh
ifconfig -a
```

### What Is My IP Address?

```sh
curl ifconfig.me
```

## Utility commands

### Archiving files & directories

```sh
# Compress Files
tar -czvf logs_archive.tar.gz *
# Extract from a compressed file
tar -xzvf logs_archive.tar.gz -C ./log

# gzp & bzip2
time gzip -k -1 file.csv
time bzip2 -k -9 file.csv
bzcat file.bz2 | grep -i "name" | wc -l
gunzip file.csv.gz
bunzip2 file.csv.bz2

#  tar
tar --exclude "build/*" --exclude ".idea/*" -acvf dir1.tar.bz2 dir1 file1 file2
tar -tvf dir1.tar.bz2 | less

# zip
du -sh dir1
zip -r arch dir1/ file*
# exclude files
zip -x dir1/.git/\* "*.jar" -r arch dir1/ file*

zip -sf  arch.zip | less
unzip arch.zip
```

### Executing dynamic commands (xargs)

```sh
ls file* | xargs -I{} mv {} {}.txt
seq 1 10 | xargs -I{} sed '{}!d' file.csv
seq -f "%02g" 0 9 | xargs -I{} sh -c 'sed "{}!d" file.csv >> file-{}.txt'
```

### Searching for files

```sh
dd if=dev/random of=testfile bs=500M count=1
touch -t 2208150000 file-16.txt

find . -not -name "file???"
find -E . -print
find -E . -regex ".*file-[[:digit:]]{2}"
find . -empty
find . -size -100M -or -size +500M
find . -perm o+rwx
find . -perm 0664
find . -atime +120w
find . -name "file*" -exec mv {} {}.txt \;
```
