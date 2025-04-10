# Cron
It is software utility, offered by Linux OS that automates the schedule task at a predefined time.
Package: cronie
Service: crond
# Commands:-
- Edit and add task in this file at predefined time
  ```
  crontab –e
  ```
  Colums in crontab- Min	  Hour	  Day	  Month	  Day of week	  CMD
- For e.g. Add automate task in crontab create a file (newfile.txt) on 12 Dec at 12:12 Tue .
  ```
  12	12	12	12	2		touch /root/newfile.txt
  ```
  For e.g. Add automate task in crontab create a file (data.txt) on hold every minute date and time.
  ```
  */1	*	*	*	*		date >> /root/data.txt
  ```
  other
  ```
  00	1-3,8-10	1-5	*	*		touch /root/f1
  15	12	*	*	*		/root/file.sh
  @reboot	mount /dev/sda1 /abc
  ```
  Note: Multiple commands can’t write in single cron task we can use the executable shell script.

- Macro Values
  1. @hourly	CMD
  2. @daily		CMD
  3. @weekly	CMD
  4. @monthly	CMD
  5. @reboot	CMD
# Cron Setup for user
 - Add cron for local user
  ```
  crontab –e –u <username>		
  ```
 - List of cron of local user
   ```
    crontab –l –u <username>		
   ```
 - Remove all cron of local user
   ```
   crontab –r –u <username>		
   ```
 - Remove all cron of root user
   ```
   crontab –r				
   ```
# Crontab configuration file:-
   ```
   cd /etc/cron.
   ```
  - cron.d
  - cron.daily/
  - cron.hourly/
  - cron.montly/
  - cron.weekly/
  - crontab
  - cron.deny
# Add a custum file (cron.allow) create by admin
  This file have higher priority than cron.deny
- Add the user in this file who can use cron even if the user is mention in cron.deny
   ```
   vim /etc/cron.allow			
   ```
# the history of cron command the use run
   ```
   cat /var/log/cron
   ```
