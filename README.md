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
