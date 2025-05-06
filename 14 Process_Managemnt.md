## Process Management
Process- A process is a running istance of a launched executabe program. Ever new process is assigned a unique process id(PID) for tracking & security.
          - Every process jave a parent proces id called PPID
          - All process are descendent of the first system process Systemd or init in linux.
          - systemd - first process after start OS it comes after rhel 6 before rhel 6 init is the first process name have PID 1.
          - systemd is the last process is stop when system shutdown.
          - if there is no parent id of a process then the parent is systemd.
# Process State
  -  R Running State
  -  S Instrupted sleeping State
  -  D Unintruptable Sleeping State
  -  T Stopped State
  -  Z Zombie State
# Process List commands
 - Check the process running on terminal 
   ```
   ps
   ```
 - To get the PID of firefox 
   ```
   pgrep firefox
   ```
 - Detail of process running like(PID,TTY,TIME,CMD) 
   ```
   ps -a
   ```
 - List of all process running in our system in shapshot format 
   ```
   ps -aux
   ```
 - Detail of all process runnig with some advanced fields
   ```
   ps -lax
   ```
 - Detailed process are running 
   ```
   ps -ef
   ```
 - Check the local user running Process 
   ```
   ps -fU <username>
   ```
 - All Running Process in tree format 
   ```
   pstree
   ```
 - Live monitroing of Running Process
   ```
   top
   ```
 
 
 
 - Run command in background called job 
   ```
   <command> &
   ```
 - check the background jobs and the job id 
   ```
   jobs
   ```
 - forground the background process 
   ```
   fg %<job_id>
   ```
 - background the forground process  
   ```
   bg %<job_id>
   ```
 - terminate the foreground process
   ```
   ctrl + c
   ```
 - temprary stop the foreground process
   ```
   ctrl +z
   ```
 - Get the process id by process name
   ```
   pidof <process name>
   ```
   OR
   ```
   pgrep <process name>
   ```
   e.g.
   ```
   pgrep firefox
   ```
 
 
 
 
 
 - To know the running process of local user
   ```
   ps -fU <username>
   ```
 - Kill all process of local user
   ```
   pkill -U <username>
   ```
 - list of all process runnng on terminal 0
   ```
   ps -t pts/0
   ```
 - Kill all process running on terminal 0
   ```
   pkill -t pts/0
   ```
 - Kill the parent process
   ```
   pkill -P <process id>
   ```
    
      
