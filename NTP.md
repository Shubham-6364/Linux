# NTP (NETWORK TIME PROTOCOL)
  - Ntp is standared internet protocol that synchronizes clocks across a network. It is used to synchronizes clock on computers, servers, and other devices. 
    - Package - chrony
    - service - chronyd
    - Config file - /etc/chrony.conf
- Add NTP in configuration file
  ```
  vim /etc/chrony.conf
  ```
  - comment this line - 
  "pool 2.rhe l.pool.ntp.org"
  - add this line (line is coming from USA ntppool.org) - 
  server 0.us.pool.ntp.org iburst
    - save and exit
  ```
  sysyemctl restart chronyd
  ```
- check ntp server time zone is now our server time zone
  ```  
  cronyc sources –v
  ```
- check system clock is synchronized: yes/no
  ```
  timedatectl				
  ```
