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
