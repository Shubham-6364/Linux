# Basic Commands
- information about the command
  ```bash
  <command name> --help 
  man <command name>
  ```

- list of file and directory
  ```bash
  ls
  ```

- list in list format
  ```bash
  ls -l
  ```
  
- list of files and directories under /var
  ```bash
  ls /var
  ```

- list of files and directories under /var in list format
  ```bash
  ls -l  /var
  ```

- list of files at path
  ```bash
  ls -li <path>
  ```

- list of file and directory with permission, owner, group owner
  ```bash
  ll
  ```

- Dispaly the content in file
  ```bash
  cat
  ```

- Copy file / Empty folder
  ```bash
  cp
  ```

- Copy Directory recurcively 
  ```bash
  cp -r
  ```

- Create a duplicate file with different name in same path
  ```bash
  cp <filename> <newfilename>
  ```

- Move file/Empty folder from one path to another
  ```bash
  mv
  ```

- Move the Directory recursively
  ```bash
  mv -r
  ```

- Rename the file/directory
  ```bash
  mv <file/directory old_name> <file/directory new_name>
  ```

- Remove the file 
  ```bash
  rm
  ```

- Remove the directory recursively
  ```bash
  rm -r
  ```

- Remove file forcefully  
  ```bash
  rm -f
  ```

- Remove directory forcefully
  ```bash
  rmdir / rm -rf
  ```

- Remove all file at current path forcefully
  ```bash
  rm -f *
  ```

- Remove all files and directory at current path forcefully  
  ```bash
  rm -rf *
  ```

- Create a file  
  ```bash
  touch <filename>
  ```

- Create multiple files  
  ```bash
  touch file{1..10}
  ```

- Create a Directory  
  ```bash
  mkdir <directory_name>
  ```

- Create a Complete path  
  ```bash
  mkdir -p directoy1/directory2/drectory3
  ```

- Diplay the tree structure of the directory  
  ```bash
  tree <directory_name>
  ```
  
- Display the top lines of the files  
  ```bash
  head <filename>
  ```
  
- Display only top n number of lines (like top 5)  
  ```bash
  head -n 5 <filename>
  ```
  
- Display the botthom lines of the files   
  ```bash
  tail <filename>
  ```
  
- Display only last n number of lines (like last 5)  
  ```bash
  tail -n 5 <filename>
  ```

- View file in Scroll down mode  
  ```bash
  more <filename>
  ```

- View file in Scroll up/down mode   
  ```bash
  less <filename>
  ```
  
- Find the Number of lines,words and lines in a file   
  ```bash
  wc <filename>
  ```

- Find only Number of lines in a file then use options( -l line , -w word, -c character) 
  ```bash
  wc -l <filename>
  ```
  










