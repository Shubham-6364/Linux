# TAR
  - Create a archieve or compress file
  - combining multiple files into a single archieve flie for easier storage, transfer or backup.
- compress a file using tar
  ```
  tar -cvf <nameoftarfile.tar> <path of directory>
  ```
- Extract data from tar file
  ```
  tar -xvf <tarfilename.tar>
  ```
- Extract data on specific path
  ```
  tar -c <destination path> -xcf <tarfilename.tar>
- Read data from tar file with extracting
  ```
  tar -tvf <tarfilename.tar>
  ```
# Types of Compression method in tar
  1. bz2(j)
      ```
      tar -cvjf <tarfilename.tar.bz2> <path of source directory/file>
      ```
  2. gz(z)
      ```
      tar -cvzf <tarfilename.tar.gz> <path of source directory/file>
      ```
  3. xz(J)
      ```
      tar -cvJf <tarfilename.tar.xz> <path of source directory/file>
      ```
- to know the size of directory/file
    ```
    du -sch <nameof directory/file>
    ```
- to know the size of all files and directory in current path
    ```
    du -sch *
    ```
