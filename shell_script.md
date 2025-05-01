#!/bin/bash
shell scripting in Linux involves writing a sequence of commands in a file which can be executed as a single program.These scripts automate tasks, manage files and control programs execution.The shell acts as an interpreter, processing commands from the script and interacting with the OS.

#!/bin/bash

vim first-script.sh
#!/bin/bash
echo "Hello World!"

make the file executable by giving permission 
chmod +x first-script.sh

methods to execute the shell script 
first
bash first-script.sh
second
sh first-script.sh
third
./first-script.sh
forth
copy the file at path /use/bin to execute the file anywhere


vim second-script.sh
#!/bin/bash
NAME=karan
AGE=26
HOST=$(hostname)
echo "My name is $NAME and my age is $AGE"
echo "Hostname of my machine is $HOST"


vim condition.sh
#!/bin/bash
echo "WELCOME TO ELECTION"
read -p "ENTER YOUR AGE:- " AGE
if [ $AGE => "18" ]
then
echo "YOU CAN VOTE"
else
echo "SORRY, YOU CANNOT VOTE"
fi

vim simple-backup.sh
#!/bin/bash
echo "WELCOME TO BACKUP"
read -p "ENTER DIRCTORY OR FILE PATH WHICH YOU WANT TO CREATE BACKUP:- " NNN
read -p "ENTER THE BACKUP STORE PATH :- " RRR
mkdir $RRR 2> /dev/null
tar -cvf $RRR/backup_new.tar $NNN

# Generate public and private key pair
ssh-keygen
# copy the public key to guest server
# Enter the password to confirm the copy
ssh-copy-id user@ipaddress
- NOW YOU CAN REMOTE LOGIN WITHOUT PASSWORD 


vim backup-method.sh
#!/bin/bash
#----CREATE DIFFERENT TYPES OF TAR AND STORE IT LOCALLY AND OTHER SERVER TOO----
# Make sure there is ssh password less login between server
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
tar -cvf $RRR/backup_new.tar $NNN
rsync $RRR/backup_new.tar user@ipaddress:/home/user/backup/

elif [ $TTT = "2" ]
then
tar -cvf $RRR/backup_new.tar.gz $NNN
rsync $RRR/backup_new.tar.gz user@ipaddress:/home/user/backup/


elif [ $TTT = "3" ]
then
tar -cvf $RRR/backup_new.tar.xz $NNN
rsync $RRR/backup_new.tar.xz user@ipaddress:/home/user/backup/

elif [ $TTT = "4" ]
then
tar -cvf $RRR/backup_new.tar.bz2 $NNN
rsync $RRR/backup_new.tar.bz2 user@ipaddress:/home/user/backup/

else
echo "INVALID INPUT"
fi