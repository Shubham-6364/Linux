# **Advanced Linux File Permissions: ACL, SUID, SGID, Sticky Bit, and chattr**

## **1. Access Control Lists (ACLs)**
**Purpose:** Extends standard permissions to allow more granular user/group access controls.

### **Key Commands**
```bash
setfacl -m u:username:permissions file  # Add user ACL
setfacl -m g:groupname:permissions file # Add group ACL
setfacl -x u:username file              # Remove user ACL
getfacl file                            # View ACLs
```

### **Examples**
```bash
setfacl -m u:alice:rwx project/      # Give Alice full access
setfacl -m g:developers:rx script.sh # Group read/execute
setfacl -m o::- backup/              # Remove all others access
```

### **Default ACLs (for new files)**
```bash
setfacl -d -m u:user:rwx /shared_dir  # Applies to new files
```

## **2. SUID (Set User ID)**
**Purpose:** Makes an executable run with owner's privileges, regardless of who runs it.

### **Characteristics**
- Represented by `s` in owner's execute position (`rwsr-xr-x`)
- Numeric value: `4000`
- Security risk if overused

### **Examples**
```bash
chmod u+s /usr/bin/passwd   # Classic example (always runs as root)
find / -perm /4000          # Find all SUID files
```

## **3. SGID (Set Group ID)**
**Purpose:** Two behaviors:
1. For executables: Runs with group's privileges
2. For directories: New files inherit directory's group

### **Characteristics**
- `s` in group execute position (`rwxr-sr-x`)
- Numeric value: `2000`

### **Examples**
```bash
chmod g+s /shared           # New files get shared's group
chmod 2755 /usr/bin/mlocate # SGID executable
```

## **4. Sticky Bit**
**Purpose:** In shared directories, only file owners can delete/rename their own files.

### **Characteristics**
- `t` in others' execute position (`rwxrwxrwt`)
- Numeric value: `1000`
- Critical for `/tmp` and `/var/tmp`

### **Examples**
```bash
chmod +t /shared_uploads    # Enable sticky bit
chmod 1777 /tmp             # Common tmp permissions
```

## **5. chattr (File Attributes)**
**Purpose:** Sets advanced filesystem attributes that even root can't bypass.

### **Common Flags**
| Flag | Effect |
|------|--------|
| `i` | Immutable (cannot be modified/deleted) |
| `a` | Append-only (can add but not modify existing) |
| `A` | No atime updates (performance) |
| `c` | Compressed (some filesystems) |
| `u` | File is recoverable if deleted |

### **Commands**
```bash
chattr +i /critical_file    # Make immutable
chattr -i /now_editable     # Remove immutable
lsattr file                 # View attributes
```

### **Real-World Uses**
```bash
chattr +i /etc/passwd       # Prevent account changes
chattr +a /var/log/secure   # Security logging
```

## **Comparison Table**
| Feature | Symbol | Numeric | Purpose | Common Use |
|---------|--------|---------|---------|------------|
| **SUID** | `s` (owner) | 4000 | Run as owner | `/usr/bin/passwd` |
| **SGID** | `s` (group) | 2000 | Inherit group | Shared directories |
| **Sticky** | `t` (others) | 1000 | Restrict deletion | `/tmp` |
| **ACL** | N/A | N/A | Granular access | Multi-user projects |
| **chattr** | N/A | N/A | Advanced attributes | System hardening |

## **Security Best Practices**
1. **Minimize SUID/SGID binaries** (`find / -perm /4000 -o -perm /2000`)
2. **Use ACLs instead of 777 permissions**
3. **Apply sticky bit to all world-writable directories**
4. **Make critical files immutable** (`chattr +i`)
5. **Regularly audit permissions** with:
   ```bash
   find / -perm /4000 -type f -exec ls -ld {} \;
   getfacl -R /sensitive_data
   ```

## **Troubleshooting Tips**
- If `s`/`t` appear as `S`/`T`: The underlying execute bit is missing
- ACLs not working? Check filesystem is mounted with `acl` option
- `chattr` changes persist even if file is moved/copied
