## Shell_script
  Shell scripting is like writing a recipe for your computer to follow automatically. Instead of typing commands one by one, you put them in a file (script) so your computer can execute them all at once.
# Think of it like this:
  Shell = The "interpreter" that understands your commands (like bash on Linux/Mac)
  Script = A text file containing commands you want to run
# Why it's useful:
  - Automate repetitive tasks (like organizing files)
  - Combine multiple commands into one action
  - Schedule jobs to run at specific times
  - Create your own custom commands

Shebang is the first line of shell script 
```
#!/bin/bash
```
# Example-1 Print simple Hello world 
vim first-script.sh
```
#!/bin/bash
echo "Hello World!"
```
make the file executable by giving permission 
chmod +x first-script.sh

methods to execute the shell script 
first
```
bash first-script.sh
```
second
```
sh first-script.sh
```
third
```
./first-script.sh
```
forth
copy the file at path /use/bin to execute the file anywhere

# Example-2 Use varable in shell script
vim second-script.sh
```
#!/bin/bash
NAME=karan
AGE=26
HOST=$(hostname)
echo "My name is $NAME and my age is $AGE"
echo "Hostname of my machine is $HOST"
```
# Example-3 User condition formatting in shell script
vim condition.sh
```
#!/bin/bash
echo "WELCOME TO ELECTION"
read -p "ENTER YOUR AGE:- " AGE
if [ $AGE => "18" ]
then
echo "YOU CAN VOTE"
else
echo "SORRY, YOU CANNOT VOTE"
fi
```

# Example-4 Getting an input from the user and create a backup of directory and store locally
vim simple-backup.sh
```
#!/bin/bash
echo "WELCOME TO BACKUP"
read -p "ENTER DIRCTORY OR FILE PATH WHICH YOU WANT TO CREATE BACKUP:- " NNN
read -p "ENTER THE BACKUP STORE PATH :- " RRR
mkdir $RRR 2> /dev/null
tar -cvf $RRR/backup_new.tar $NNN
```

# Example-5 Getting an input from the user and create a backup of directory and store locally, and remote servers
# Generate public and private key pair
```
ssh-keygen
```
# copy the public key to guest server
# Enter the password to confirm the copy
```
ssh-copy-id user@ipaddress
```
- NOW YOU CAN REMOTE LOGIN WITHOUT PASSWORD 

vim backup-method.sh
```
#!/bin/bash
#----CREATE DIFFERENT TYPES OF TAR AND STORE IT LOCALLY AND OTHER SERVER TOO----
# Make sure there is ssh password less login between server
set -exo pipefail
# x for run in debugging mode, e for exit the script when there is an error in a script, o when pipefail
TIMESTAMP=$(date +%d_%m_%Y-%H_%M_%S)
echo "WELCOME TO BACKUP"
read -p "ENTER DIRCTORY OR FILE PATH WHICH YOU WANT TO CREATE BACKUP:- " NNN
read -p "ENTER THE BACKUP STORE PATH :- " RRR
mkdir $RRR 2> /dev/null
read -p "ENTER THE BACKUP METHOD :-
Press 1 for backup.tar backup
Press 2 for backup.tar.gz
Press 3 for backup.tar.xz
Press 4 for backup.tar.bz2" TTT
if [ $TTT = "1" ]
then
tar -cvf $RRR/backup_$TIMESTAMP.tar $NNN
#rsync $RRR/backup_new.tar user@ipaddress:/home/user/backup/

elif [ $TTT = "2" ]
then
tar -cvf $RRR/backup_$TIMESTAMP.tar.gz $NNN

elif [ $TTT = "3" ]
then
tar -cvf $RRR/backup_$TIMESTAMP.tar.xz $NNN

elif [ $TTT = "4" ]
then
tar -cvf $RRR/backup_$TIMESTAMP.tar.bz2 $NNN

else
echo "INVALID INPUT"
fi
# Tranfer the backup file to the remote server
rsync $RRR/backup_$TIMESTAMP.tar.bz2 user@ipaddress:/home/user/backup/
# delete the backup file store locally after the remote transfer
rm -rf $RRR
```
