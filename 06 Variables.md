# **Variables in Linux (Shell Scripting)**

Variables in Linux are used to store data temporarily for use in scripts and commands. They are essential for writing dynamic and reusable shell scripts.

---

## **1. Variable Types**
### **a) System (Environment) Variables**
- Predefined by the system
- Affect the shell and child processes
- View all with: `printenv` or `env`

**Common Examples:**
```bash
$HOME    # User's home directory (e.g., /home/username)
$PATH    # List of directories for executable programs
$USER    # Current username
$PWD     # Present working directory
$SHELL   # Current shell (e.g., /bin/bash)
```

### **b) User-Defined Variables**
- Created by users in scripts or command line
- Local to current shell by default (not inherited by child processes)

---

## **2. Variable Syntax Rules**
1. **No spaces** around `=` when assigning:
   ```bash
   correct=value
   wrong = value  # Error
   ```

2. **Variable names** can contain:
   - Letters (a-z, A-Z)
   - Numbers (0-9)
   - Underscores (_)
   - Cannot start with a number

3. **Access values** with `$` prefix:
   ```bash
   name="John"
   echo $name    # Output: John
   ```

4. **Quotes matter**:
   ```bash
   # Double quotes allow variable expansion
   greeting="Hello $name"  # Hello John

   # Single quotes treat as literal
   greeting='Hello $name'  # Hello $name
   ```

---

## **3. Working with Variables**
### **Setting Variables**
```bash
name="Alice"          # Simple assignment
count=10              # Numbers work too
files=$(ls)           # Store command output
today=$(date +%F)     # Store formatted date
```

### **Exporting (Making Environment Variables)**
```bash
export API_KEY="12345"  # Available to child processes
```

### **Unsetting Variables**
```bash
unset name  # Removes the variable
```

### **Read-Only Variables**
```bash
readonly PI=3.14
PI=3.14159  # Error: readonly variable
```

---

## **4. Special Variables**
| Variable | Description |
|----------|-------------|
| `$0`     | Name of the script |
| `$1-$9`  | Positional arguments |
| `$#`     | Number of arguments |
| `$*`     | All arguments as single string |
| `$@`     | All arguments as separate strings |
| `$?`     | Exit status of last command |
| `$$`     | Process ID (PID) of current shell |
| `$!`     | PID of last background job |

**Example Script (`test.sh`):**
```bash
#!/bin/bash
echo "Script: $0"
echo "First arg: $1"
echo "All args: $@"
echo "Total args: $#"
echo "Last command status: $?"
```
Run with: `./test.sh one two three`

---

## **5. Variable Manipulation**
### **String Operations**
```bash
str="Hello World"
echo ${#str}       # Length: 11
echo ${str:6}      # Substring: World
echo ${str:6:3}    # Substring: Wor
echo ${str/World/Linux}  # Replace: Hello Linux
```

### **Default Values**
```bash
echo ${name:-"Guest"}  # Uses "Guest" if $name is unset
echo ${count:=0}       # Sets $count to 0 if unset
```

---

## **6. Best Practices**
1. **Quote variables** to prevent word splitting:
   ```bash
   # Bad (fails with spaces)
   cp $file $dest

   # Good
   cp "$file" "$dest"
   ```

2. **Use curly braces** for clarity:
   ```bash
   echo "${name}script"  # Clear variable boundary
   ```

3. **UPPERCASE** for environment variables
   ```bash
   export DB_HOST="localhost"
   ```

4. **Lowercase** for local script variables
   ```bash
   temp_file="/tmp/data.txt"
   ```

---

## **7. Real-World Examples**
### **Backup Script with Variables**
```bash
#!/bin/bash
BACKUP_DIR="/backups"
TODAY=$(date +%Y-%m-%d)
ARCHIVE="backup-$TODAY.tar.gz"

tar -czf "$BACKUP_DIR/$ARCHIVE" /home/user/documents
echo "Backup created: $BACKUP_DIR/$ARCHIVE"
```

### **Configuration Management**
```bash
#!/bin/bash
# config.sh
DB_USER="admin"
DB_PASS="secret123"
DB_NAME="app_db"

# Use in another script
source config.sh
mysqldump -u "$DB_USER" -p"$DB_PASS" "$DB_NAME" > backup.sql
```

---

## **Summary Table**
| Concept | Example | Description |
|---------|---------|-------------|
| **Assignment** | `var="value"` | Create a variable |
| **Access** | `echo $var` | Get variable value |
| **Export** | `export var` | Make available to subprocesses |
| **Command Substitution** | `files=$(ls)` | Store command output |
| **Default Value** | `${var:-default}` | Use if variable is unset |
| **String Length** | `${#str}` | Get string length |

Mastering variables is crucial for **effective shell scripting** in Linux! 🚀
