 # **`grep`, `awk`, and `sed` in Linux**  
These are **powerful text-processing tools** used for searching, filtering, and manipulating text in files or streams.  

---

## **1. `grep` (Global Regular Expression Print)**  
**Purpose:** Search for **patterns** (text or regex) in files or input.  

### **Basic Syntax**  
```bash
grep [options] "pattern" file
```

### **Common `grep` Examples**  
| Command | Description |
|---------|-------------|
| `grep "error" log.txt` | Find lines containing "error" in `log.txt` |
| `grep -i "warning" file` | Case-insensitive search for "warning" |
| `grep -r "main" /path/` | Recursively search in directories |
| `grep -v "success" file` | Show lines **not** containing "success" |
| `grep -A 3 "exception" file` | Show **3 lines after** the match |
| `grep -B 2 "start" file` | Show **2 lines before** the match |
| `grep -n "pattern" file` | Show line numbers |
| `grep -E "[0-9]+" file` | Use **regex** (extended mode) |

---

## **2. `awk` (Aho, Weinberger, Kernighan)**  
**Purpose:** Process and analyze **structured text** (e.g., columns in CSV/logs).  

### **Basic Syntax**  
```bash
awk 'pattern {action}' file
```

### **Common `awk` Examples**  
| Command | Description |
|---------|-------------|
| `awk '{print $1}' file` | Print **1st column** of each line |
| `awk -F "," '{print $2}' data.csv` | Use **comma** as separator (for CSV) |
| `awk '/error/ {print $3}' log.txt` | Print 3rd column of lines with "error" |
| `awk '$3 > 100 {print $0}' data.txt` | Print lines where 3rd column > 100 |
| `awk '{sum += $1} END {print sum}' nums.txt` | Sum values in 1st column |
| `awk 'NR==2, NR==5 {print}' file` | Print lines **2 to 5** |
| `awk 'length($0) > 80' file` | Print lines longer than 80 chars |

---

## **3. `sed` (Stream Editor)**  
**Purpose:** **Edit text** in a stream (find/replace, delete, insert).  

### **Basic Syntax**  
```bash
sed 's/find/replace/' file
```

### **Common `sed` Examples**  
| Command | Description |
|---------|-------------|
| `sed 's/old/new/' file` | Replace **first** "old" with "new" per line |
| `sed 's/old/new/g' file` | Replace **all** occurrences (global) |
| `sed '2d' file` | **Delete line 2** |
| `sed '/pattern/d' file` | Delete lines matching "pattern" |
| `sed '3i\new line' file` | **Insert** "new line" **before** line 3 |
| `sed '5a\new line' file` | **Append** "new line" **after** line 5 |
| `sed -n '10,20p' file` | Print **lines 10 to 20** |
| `sed -i.bak 's/foo/bar/g' file` | Edit file **in-place** (creates backup `.bak`) |

---

## **Comparison: `grep` vs `awk` vs `sed`**
| Tool | Best For | Key Feature |
|------|---------|-------------|
| **`grep`** | Searching text | Fast pattern matching |
| **`awk`** | Column-based processing | Built-in variables (`$1`, `NR`, etc.) |
| **`sed`** | Editing text streams | Regex substitution & line manipulation |

---

## **Combining Them Together**
### **Example 1: Find Errors & Extract Timestamps**  
```bash
grep "ERROR" log.txt | awk '{print $1, $2}'
```
- `grep` finds lines with "ERROR".  
- `awk` extracts the 1st & 2nd columns (timestamp).  

### **Example 2: Replace Text & Count Matches**  
```bash
sed 's/old/new/g' file | grep -c "new"
```
- `sed` replaces "old" with "new".  
- `grep -c` counts occurrences of "new".  

### **Example 3: Filter CSV & Format Output**  
```bash
awk -F "," '$3 > 50 {print $1, $2}' data.csv | sed 's/ /,/g'
```
- `awk` filters rows where 3rd column > 50.  
- `sed` replaces spaces with commas.  

---

## **Cheat Sheet**
### **`grep`**  
```bash
grep "text" file       # Basic search  
grep -i "text" file    # Case-insensitive  
grep -v "text" file    # Invert match  
grep -E "regex" file   # Extended regex  
```

### **`awk`**  
```bash
awk '{print $2}' file          # Print 2nd column  
awk -F ":" '{print $1}' file   # Use ":" as separator  
awk 'NR==1,NR==5' file         # Print lines 1-5  
```

### **`sed`**  
```bash
sed 's/old/new/g' file       # Replace text  
sed '/pattern/d' file        # Delete lines  
sed -i 's/foo/bar/' file     # Edit file in-place  
```

---

### **When to Use Which?**
- **`grep`** → Quickly find text in files.  
- **`awk`** → Process structured data (columns, calculations).  
- **`sed`** → Edit files (find/replace, delete lines).  

These tools are **essential** for Linux sysadmins, DevOps, and power users! 🚀  
