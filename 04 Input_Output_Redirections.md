# **Input/Output Redirection in Linux**  

In Linux, **redirection** allows you to control where the **input** comes from and where the **output** (or errors) go. It’s a powerful way to manage data flow between commands, files, and devices.

---

## **1. Standard Streams (File Descriptors)**
Every Linux process has three default streams:
| Stream | File Descriptor | Description |
|--------|----------------|-------------|
| **stdin (Standard Input)** | `0` | Input (usually from keyboard) |
| **stdout (Standard Output)** | `1` | Output (usually to terminal) |
| **stderr (Standard Error)** | `2` | Error messages (usually to terminal) |

---

## **2. Output Redirection (`>`, `>>`)**
### **Redirect stdout to a file (overwrite)**
```bash
command > output.txt
```
**Example:**  
```bash
ls > files_list.txt  # Saves `ls` output to a file (replaces if exists)
```

### **Redirect stdout to a file (append)**
```bash
command >> output.txt
```
**Example:**  
```bash
date >> log.txt  # Adds current date to log.txt without deleting old content
```

### **Redirect stderr to a file**
```bash
command 2> error.log
```
**Example:**  
```bash
grep "something" non_existent_file.txt 2> error.log
```

### **Redirect both stdout & stderr to a file**
```bash
command &> output.log
```
**or (older syntax)**
```bash
command > output.log 2>&1
```

---

## **3. Input Redirection (`<`, `<<`)**
### **Read input from a file (instead of keyboard)**
```bash
command < input.txt
```
**Example:**  
```bash
wc -l < data.txt  # Counts lines in data.txt
```

### **Here Document (`<<`)**
Pass multi-line input directly to a command.  
```bash
command << EOF
line1
line2
EOF
```
**Example:**  
```bash
cat << EOF > message.txt
Hello,
This is a generated file.
EOF
```

---

## **4. Pipes (`|`)**
Send the **output of one command** as **input to another**.  
```bash
command1 | command2
```
**Examples:**  
```bash
ls -l | grep ".txt"  # List files and filter only .txt files
ps aux | grep "nginx"  # Find running Nginx processes
```

---

## **5. Advanced Redirections**
### **Discard Output (Send to `/dev/null`)**
```bash
command > /dev/null  # Discard stdout
command 2> /dev/null  # Discard stderr
command &> /dev/null  # Discard all output
```

### **Send stderr to stdout (and vice versa)**
```bash
command 2>&1  # stderr → stdout
command 1>&2  # stdout → stderr
```

### **Redirect to Multiple Commands (`tee`)**
Save output to a file **and** display it on screen.  
```bash
command | tee output.txt
```
**Example:**  
```bash
ls | tee files_list.txt  # Shows output and saves to file
```

---

## **6. Practical Examples**
| Command | Explanation |
|---------|-------------|
| `sort < unsorted.txt > sorted.txt` | Sorts `unsorted.txt` and saves to `sorted.txt` |
| `find / -name "*.log" 2> /dev/null` | Finds log files, hides errors |
| `echo "Hello" > greeting.txt` | Writes "Hello" to `greeting.txt` |
| `cat file1 file2 > combined.txt` | Merges two files into one |
| `tail -f /var/log/syslog | grep "error"` | Monitors syslog for errors |

---

### **Summary Table**
| Redirection | Syntax | Effect |
|------------|--------|--------|
| **stdout to file (overwrite)** | `>` | Replaces file content |
| **stdout to file (append)** | `>>` | Adds to file |
| **stderr to file** | `2>` | Saves errors |
| **stdout + stderr to file** | `&>` | Saves all output |
| **stdin from file** | `<` | Reads input from file |
| **Pipe output** | `|` | Sends output to next command |
| **Discard output** | `> /dev/null` | Silences output |

---

### **Key Takeaways**
✅ **`>`** = Overwrite file  
✅ **`>>`** = Append to file  
✅ **`2>`** = Redirect errors  
✅ **`|`** = Chain commands  
✅ **`<`** = Read from file  

Mastering redirection makes Linux command-line usage **much more powerful**! 🚀  
