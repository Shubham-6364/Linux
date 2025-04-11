# Tuning
  Enhanced the performance of system. optimizing system performance for specific workloads by adjusting settings 
  such as CPU, memory, disk I/O and network parameters, it focus on making a linux system run faster more efficiently and with improved responsibilities.
- Two types of performance tuning
  1.	Static tuning
  2.	Dynamic tuning

- Static Tuning 
  1. package : tuned
  2. service : tuned.service
# Commands:-
- List of profile
    ```
    tuned–adm profile
    ```
- Current active profile    
    ```
    tuned–adm active
    ```
- Change tuning profile    
    ```
    tuned–adm profile powersave
    ```
- Check the active profile  
    ```
    tuned-adm active
    ```
- Recommend profile by system   
    ```
    tuned-adm recommend 
    ```
