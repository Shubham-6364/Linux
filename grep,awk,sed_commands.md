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
 # **Real-World Examples of `grep`, `awk`, and `sed`**  

Here are **practical, real-world scenarios** where these commands are used in Linux/DevOps.

---

## **1. Log Analysis (`grep` + `awk`)**
**Problem:**  
You need to find all `"ERROR"` messages in a log file and extract their **timestamps** and **error codes**.

**Solution:**  
```bash
grep "ERROR" /var/log/syslog | awk '{print $1, $2, $3, $9}'
```
**Explanation:**  
- `grep "ERROR"` → Filters lines containing "ERROR".  
- `awk '{print $1, $2, $3, $9}'` → Extracts **date, time, process, and error code**.  

**Example Output:**  
```
May 5 14:30:01 cron[123] 500  
May 5 14:32:45 nginx[456] 404  
```

---

## **2. CSV Data Processing (`awk`)**  
**Problem:**  
You have a `data.csv` file and want to extract **users with salaries > $5000**.  

**`data.csv` Example:**  
```
Name,Age,Salary  
Alice,25,6000  
Bob,30,4500  
Charlie,35,7000  
```

**Solution:**  
```bash
awk -F "," '$3 > 5000 {print $1, $3}' data.csv
```
**Explanation:**  
- `-F ","` → Uses comma as separator.  
- `$3 > 5000` → Filters rows where salary (3rd column) > 5000.  
- `{print $1, $3}` → Prints **name** and **salary**.  

**Output:**  
```
Alice 6000  
Charlie 7000  
```

---

## **3. Bulk File Renaming (`sed` + `mv`)**  
**Problem:**  
You have files like `file1.txt`, `file2.txt`, and want to rename them to `backup1.txt`, `backup2.txt`.  

**Solution:**  
```bash
for file in *.txt; do
    newname=$(echo "$file" | sed 's/file/backup/')
    mv "$file" "$newname"
done
```
**Explanation:**  
- `sed 's/file/backup/'` → Replaces "file" with "backup".  
- `mv` renames the file.  

**Result:**  
```
file1.txt → backup1.txt  
file2.txt → backup2.txt  
```

---

## **4. Extracting IPs from Logs (`grep` + Regex)**  
**Problem:**  
You need to extract **all IP addresses** from an Nginx access log.  

**Solution:**  
```bash
grep -Eo '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' /var/log/nginx/access.log | sort | uniq
```
**Explanation:**  
- `-Eo` → Enables **regex** and **only prints matches**.  
- `[0-9]{1,3}\.` → Matches IP-like patterns (e.g., `192.168.1.1`).  
- `sort | uniq` → Removes duplicates.  

**Output:**  
```
192.168.1.1  
10.0.0.5  
```

---

## **5. Parsing `ps aux` Output (`awk`)**  
**Problem:**  
You want to find **top 3 memory-consuming processes**.  

**Solution:**  
```bash
ps aux | sort -nk4 -r | awk 'NR<=3 {print $4, $11}'
```
**Explanation:**  
- `ps aux` → Lists all processes.  
- `sort -nk4 -r` → Sorts by **memory usage (4th column)** in descending order.  
- `awk 'NR<=3 {print $4, $11}'` → Prints **%MEM** and **command name** for the top 3.  

**Output:**  
```
7.5 /usr/bin/firefox  
5.2 /usr/bin/docker  
3.1 /usr/bin/slack  
```

---

## **6. Removing Empty Lines (`sed`)**  
**Problem:**  
You have a file with **blank lines** and want to clean it up.  

**Solution:**  
```bash
sed '/^$/d' messy_file.txt > clean_file.txt
```
**Explanation:**  
- `/^$/` → Matches **empty lines** (`^`=start, `$`=end).  
- `d` → Deletes them.  

**Before:**  
```
Line 1

Line 2  

Line 3
```
**After:**  
```
Line 1  
Line 2  
Line 3  
```

---

## **7. Counting HTTP Status Codes in Logs (`awk`)**  
**Problem:**  
You need to count how many `200`, `404`, and `500` responses appear in a web log.  

**Solution:**  
```bash
awk '{print $9}' access.log | sort | uniq -c | sort -nr
```
**Explanation:**  
- `awk '{print $9}'` → Extracts the **HTTP status code** (9th column).  
- `sort | uniq -c` → Counts occurrences of each code.  
- `sort -nr` → Sorts in descending order.  

**Output:**  
```
200 1500  
404 32  
500 5  
```

---

## **8. Replacing Text in Multiple Files (`sed`)**  
**Problem:**  
You need to replace `"old_domain.com"` with `"new_domain.com"` in **all `.conf` files**.  

**Solution:**  
```bash
sed -i 's/old_domain\.com/new_domain.com/g' *.conf
```
**Explanation:**  
- `-i` → Edits files **in-place**.  
- `s/old/new/g` → Replaces all occurrences.  

**Result:**  
All `.conf` files now use `new_domain.com`.

---

### **Summary of Use Cases**
| Task | Command Used |
|------|-------------|
| **Find errors in logs** | `grep + awk` |
| **Process CSV data** | `awk` |
| **Rename files in bulk** | `sed + mv` |
| **Extract IPs from logs** | `grep -E` |
| **Find top memory processes** | `ps + awk` |
| **Remove empty lines** | `sed '/^$/d'` |
| **Count HTTP status codes** | `awk + uniq` |
| **Bulk text replacement** | `sed -i` |

These commands are **essential for automation, log analysis, and data processing** in Linux. 🚀  

