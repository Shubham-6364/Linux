# Services
- ssh is enabled/disable status
  ```
  systemctl status sshd
  ```
- when system restart the ssh service automatically active mode
  ```
  systemctl enable sshd
  ```
- when system restart the ssh service is inactive mode
  ```
  systemctl disable sshd
  ```
- To stop ssh service
  ```
  systemctl stop sshd
  ```
- To start ssh service
  ```
  systemctl start sshd
  ```

- for change ssh configuration file
  ```
  vim /etc/ssh/sshd_config
  ```
- To reload the service without any downtime
  ```
  systemctl reload sshd  
  ```
- To restart the service with downtime
  ```
  systemctl restart sshd
  ```
- Mask the service- mode can't be change after that
  ```
  systemctl mask sshd
  ```
- Unmask the servie- mode can be change after that
  ```
  systemctl unmask sshd
  ```
