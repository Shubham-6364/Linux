# User Administrator Commands

- User add
  ```bash
  useradd <username>
  ```
- Set password for user 
  ```bash
  passwd <username>
  ```
- Check User id of local user
  ```bash
  id <username>
  ```
- userdel with all files releted to this user
  ```bash
  userdel -r <username>
  ```
- Add Group
  ```bash
  groupadd <groupname>
  ```
- Delete Group
  ```bash
  groupdel <groupname>
  ```
- Switch to other user( with old shell)
  ```bash
  su <username>
  ```
- Swithh to other user ( with new shell)
  ```bash
  su - <username>
  ```
- Check the list of user created in system
  ```bash
  cat /etc/passwd
  ```
  OR
  ```bash
  getent passwd
  ```
- Check the list of group created in system
  ```bash
  cat /etc/group
  ```
  OR
  ```bash
  getent group
  ```
# Modify Existing user
- Change UID of local user
  ```bash
  usermod -u <UID> <username>
  ```
- User add in Secondary Group
  ```bash
  usermod -G <group_name> <username>
  ```
  OR
  ```bash
  gpasswd -a <username> <groupname>
  ```
- Multiple users add in a group
  ```bash
  gpasswd -M <username>,<user2>,<user3> <groupname>
  ```
- Delete user from group
  ```bash
  gpasswd -d <username> <groupname>
  ```
- User add in primary Group
  ```bash
  usermod -g <group_name> <username>
  ```
- Add Comment to username
  ```bash
  usermod -c "Write_a_comment_here" <username>
  ```  
- Add user to multiple groups
  ```bash
  usermod -aG <groupname> <username>
  ```
- change the user shell to no login shell 
  ```bash
  usermod -s /sbin/nologin
  ```
  change to login shell
  ```bash
  usermod -s /bin/bash <username>
  ```
- Assgin UID while adding a new user
  ```bash
  useradd -u <UID> <username>
  ```
- Chnage shell while adding a new user
  ```bash
  useradd -s /sbin/nologin <username>
  ```
- Change the home directory path of a new user
  ```bash
  useradd -d <path> <username>
  ```
- 
  ```bash
  
  ```
- 
  ```bash
  
  ```
- 
  ```bash
  
  ```
