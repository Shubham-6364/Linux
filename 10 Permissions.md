# **Linux File Permissions**

Permissions in Linux control access to files and directories for users and processes. Understanding them is crucial for system security and proper functioning.

## **1. Basic Permission Types**

Linux has three types of permissions:

| Permission | Symbol | Files | Directories |
|------------|--------|-------|-------------|
| **Read**   | `r`    | View file content | List directory contents |
| **Write**  | `w`    | Modify file content | Create/delete files |
| **Execute**| `x`    | Run as program | Enter directory (cd) |

## **2. Permission Classes**

Permissions are assigned to three classes:

1. **Owner (u)** - The file/directory owner
2. **Group (g)** - Members of the file's group
3. **Others (o)** - All other users

## **3. Viewing Permissions**

Use `ls -l` to view permissions:

```bash
-rw-r--r-- 1 user group 4096 May 15 10:00 file.txt
drwxr-xr-x 2 user group 4096 May 15 10:00 directory/
```

The first character indicates file type (`-` for file, `d` for directory). The next 9 characters show permissions for owner, group, and others.

## **4. Permission Modes**

Permissions can be represented in two ways:

### **Symbolic Mode**
```bash
u=user, g=group, o=others, a=all
+ add, - remove, = set exactly
```

Examples:
```bash
chmod u+x file.txt      # Add execute for owner
chmod g-w file.txt      # Remove write from group
chmod o=r file.txt      # Set others to read-only
chmod a=rw file.txt     # Set all to read-write
```

### **Numeric (Octal) Mode**
Each permission has a numeric value:
- Read (r) = 4
- Write (w) = 2
- Execute (x) = 1

Combine them for each class (owner/group/others):

```bash
chmod 755 file.txt
# 7 (4+2+1) = rwx for owner
# 5 (4+0+1) = r-x for group
# 5 (4+0+1) = r-x for others
```

Common permission sets:
- `755` - Executable files (rwxr-xr-x)
- `644` - Regular files (rw-r--r--)
- `700` - Private files (rwx------)
- `750` - Shared executables (rwxr-x---)

## **5. Special Permissions**

Three additional special permissions:

| Permission | Symbol | Numeric | Effect |
|------------|--------|---------|--------|
| **Set User ID (SUID)** | `s` | 4000 | Runs as owner |
| **Set Group ID (SGID)** | `s` | 2000 | Runs as group (or new files inherit group) |
| **Sticky Bit** | `t` | 1000 | Only owner can delete (for shared dirs) |

Examples:
```bash
chmod u+s /usr/bin/passwd   # SUID (always runs as root)
chmod g+s /shared           # SGID (new files inherit group)
chmod +t /tmp               # Sticky bit (only owners can delete)
```

Numeric examples:
```bash
chmod 4755 /usr/bin/special  # SUID with 755
chmod 2755 /shared           # SGID with 755
chmod 1777 /tmp              # Sticky with 777
```

## **6. Changing Ownership**

Use `chown` and `chgrp` to change owners and groups:

```bash
chown user:group file.txt    # Change both owner and group
chown user file.txt          # Change owner only
chgrp group file.txt         # Change group only
chown -R user:group /dir/    # Recursive change
```

## **7. Default Permissions (umask)**

The `umask` determines default permissions for new files:

```bash
umask 022       # Common default (files: 644, dirs: 755)
umask 027       # More restrictive (files: 640, dirs: 750)
```

Calculation:
- Files: 666 - umask
- Directories: 777 - umask

## **8. Permission Best Practices**

1. **Follow principle of least privilege**
2. **Use groups** for shared access instead of opening to "others"
3. **Regularly audit** permissions (`find / -perm /4000` for SUID files)
4. **Be careful with 777** - only use when absolutely necessary
5. **Use ACLs** for complex permission schemes

## **9. Access Control Lists (ACLs)**

For more granular control than standard permissions:

```bash
setfacl -m u:user:rwx file.txt   # Add user permissions
setfacl -m g:group:rx file.txt   # Add group permissions
getfacl file.txt                 # View ACLs
```

## **10. Troubleshooting Permission Issues**

Common commands for diagnosis:

```bash
ls -ld /path                    # Check directory permissions
namei -l /path/to/file          # Show permissions along path
sudo -u user command            # Test as specific user
strace -e open command          # See permission errors
```

Remember that for accessing a file:
1. You need **directory execute** permission to traverse directories
2. You need **file read** permission to view contents
3. The **effective permissions** depend on both file and parent directory permissions
