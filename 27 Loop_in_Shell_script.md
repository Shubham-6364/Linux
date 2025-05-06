# Loop in shell scripting
A loop in shell scripting is like a repeating command - it lets you do the same task multiple times without writing the same code over and over.
# Real-Life Examples:
  - Rename 100 files at once
  - Process each line in a file
  - Keep trying to connect to a server until successful
  - Create multiple user accounts with one script
# Main Types of Loops in Shell Scripts:
**1. For Loop - When you know how many times to repeat**
```
for i in 1 2 3 4 5
do
   echo "Counting $i"
done
```
This prints numbers 1 through 5

**2. While Loop - Repeat while a condition is true**
```
count=1
while [ $count -le 5 ]
do
   echo "Counting $count"
   count=$((count+1))
done
```
This also counts 1 to 5

**3. Until Loop - Repeat until a condition is true**
```
count=1
until [ $count -gt 5 ]
do
   echo "Counting $count"
   count=$((count+1))
done
```
Same result as above

# Example 1- User add using for loop
```
#!/bin/bash
for username in shubham shivraj shruti radhika
do
useradd $username
done
```
# Example 2- Useradd by getting username from a file and set a common password
```
#!/bin/bash
# create the users by taking the list of name of users from file
for username in $(cat /root/usernamefile)
do
useradd $username
# set a common password "pass"
echo "pass" | passwd --stdin $username
done
```
# Example 3- Run loop until the conditon is true
```
#!/bin/bash
count = 0
num = 0
while [ $count -le $num ]
do
echo $count
let count++
done
```
