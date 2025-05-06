## FIND COMMAND
To find inside the file we use grep command, but we want to find file/directory than we use find command


- find all file with name specified under the /
```
find / -name <file_name>
```
- find the file under /etc 
```
find /etc -name <filename>
```
- find only file
```
find / -name <filename> -type f
```
- find only directory
```
find / -name <filename> -type d
```
- find the file or directory with igonore the casesentive name under /
```
find / -iname <filename>
```
- find the file/directory with its inode number
```
find / inum <inode number>
```
- find the files/directory with owned by specfied user
```
find / -user <username>
```
- find the files/directory with owned by specfied group
```
find / -group <groupname>
```
- find the file which have size more than 50mb
```
find / -size +50M
```
- find the file which have size exact 50mb
```
find / -size 50M
```
- find the file which have size less than 50mb
```
find / -size -50M
```
- find the file which have size between 50mb to 100mb
```
find / -size +50M -size -100M
```
- find the files/directories with have full permission
```
find / -perm 777
```
- find the file/directories which have sticky bit
```
find / -perm /o=t
```
- find the empty file
```
find / -empty
```
- find the file/directory and delete them
```
find / -iname <name> -delete
```
- find the empty file and delete them
```
find / -empty -delete
```
- find the all files which has owned by specific user copy to the specific directory
```
find / -user <username> -exec cp -avrf {} <destination_directory>/ \;
```
- Change the own of all files of specific user to new user 
```
find / -user <old_username> -exec chown <new_username> {} \;
```
