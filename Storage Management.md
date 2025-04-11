# Storage Mnagement
- Create a partition with MBR
   ```
  fdisk /dev/sda
  ```
  - Press n
  - Select - p – for primary e- for extended
  - Enter partition number - 
  - First sector
  - Last sector
  - Press P for print 
  - Press w for save and quit
  - Press -q for quit without save
  
- to check the partition  
  ```
  lsblk 
  ```
-  Now format the partition for make use able
    ```
    mkfs.ext4 /dev/sda1
    ```
- to check the partition is formatted and have UUID  
  ```
  blkid
  ```
After all above we want to access the partition useable or we want to store some files in this partition.
- mount the partition 
  ```
  mount /dev/sda1 /abc
  ```
- to check mount is done
  ```
  lsblk
  ```
- for permanent mounting 
  ```
  vim /etc/fstab
  ```
- Here five fields are there
  1. UUID/PATH -	
  2. MOUNT POINT - /abc		# Disk Partition Access point
  3. FILESYSTEM TYPE – ext4		# enter the filesystem which you format the disk
  4. OPTIONS (read, write) – defaults	#default means read write both
  5. DUMP TERMINAL – 0		# whether you want to take dump backup or not of your partition (1 for ON 0 for OFF)
  6. FSCK(File System Consistency Check) – 0		#When system restart it checks the disk health. ( 1 for ON 0 for OFF)
  - Note: use TAB key for next field don’t use Spacebar
  - save and exit!
- to verify your partition mount changes  
  ```
  mount –a
  ``` 
- to see file system type
  ```
  df –hT
  ```
- to see the mount point
  ```
  lsblk 
  ```
#  SWAP PARTITION
- know the Ram memory  
  ```
  free –h
  ```		
- create swap partition 
  ```
  fdisk /dev/sda
  ```
  - Press n
  - Select - p 		# for primary
  - Enter partition number - 2 
  - First sector – (press Enter)
  - Last sector - +1G	# Enter the swap memory size
  - Press P for print
  - Press t 		# to change the type of disk
  - partition type- 82	# 82 Hex code to create swap type partition
  - Press P			# To see the change in the partition type
  - Press w			# for save and exit

# For use the partition as Swap
- Format swap partition useable
  ```
  mkswap /dev/sda2	
  ```
- check the swap memory before swap on
  ```
  free –h
  ```			
- swap memory ON Temporarily
  ```
  swapon /dev/sda2
  ```	
- Now you see the swap memory size is increase
  ```
  free –h
  ``` 			
- Swap memory OFF Temporarily
  ```
  swapoff /dev/sda2
  ```	
# For permanent add this swap memory 
  ```
  vim /etc/fstab
  ```
   - UUID/PATH -	
   - MOUNT POINT - swap
   - FILESYSTEM TYPE – swap
   - OPTIONS (read, write) – defaults
   - DUMP TERMINAL – 0
   - FSCK(File System Consistency Check) – 0	
  - Note: use TAB key for next field don’t use Spacebar
  - save and exit!

# LVM (LOGICAL VOLUME MANAGER)
  - We can increase or decrease our partition size in runtime without data loss.
  - LVM provide an utility to create multiple logical volume

- Let's add a 20 GB drive(sdb) to our system
  - Drive (sda)	PV (sda1,sda2,.)	               VG(sda1+sdb2) 		   LV
- Create a disk partition of 3GB
  ```
  fdisk /dev/sdb
  ```
  - Press n
  - Select - p 		# for primary
  - Enter partition number - 1
  - First sector – (press Enter)
  - Last sector - +3G	# Enter the swap memory size
  - Press P for print
  - Press t 		# to change the type of disk
  - partition type- 8E	# LVM type partition
  - Press P			# To see the change in the partition type
  - Press w			# for save and exit
- To create Physical Volume
    ```
    pvcreate /dev/sdb1
    ```
  - To verify the list of PV
    ```
    pvdisplay
    ```		
    OR
    ```
    pvs
    ```
- To create Volume Group	(“myvg” is the name of vg)    
    ```
    vgcreate myvg /dev/sdb1	
    ```
  - To verify the list of VG
    ```
    vgdisplay
    ```
    OR
    ```
    vgs
    ```
    OR 
    ```
    vgscan
    ```
- To create Logical Volume	(“mylv” is the name of lv)
    ``` 
    lvcreate –L +50M –n mylv myvg	
    ```
    OR lv is created through no. of extent(4*13=52MB) 
    ```
    lvcreate –l 13 –n mylv myvg
    ```
  - To verify the list of LV
    ```
    lvdisplay
    ```
  - Format drive with file system
    ```
    mkfs.ext4 /dev/myvg/mylv	
    ```

  - To check the partition file system
    ```
    blkid 				
    ```
  - Lv access point
    ```
    mount /dev/myvg/mylv /abc	
    ```
- Increase the existing LV
  - To increase the size of LV
    ```
    lvresize –L +30M /dev/myvg/mylv	
    ```
  - Check
    ```
    lvs
    ```
- To resize file system
    ```
    resize2fs /dev/myvg/mylv
    ```		
  - now see the size is changed
    ```
    df –hT
    ```					
  - Note: If you format the your partition with ext4 file system then you can increase or decrease the size of partition.
  - If you format the your partition with xfs file system then you can only increase partition.
- Decrease the existing LV
  - To decrease the size of LV
    ```
    lvresize –L -30M /dev/myvg/mylv
    ```	
  - To resize file system
    ```
    resize2fs /dev/myvg/mylv
    ```		
- To decrease the size of LV and also resize the file system
    ```
    lvresize –rL -30M /dev/myvg/mylv
    ```	

- Extend the existing VG
  - Checl vg size
    ```
    myvg
    ```
  - sdb2 is new vg
    ```
    pvcreate /sda/sdb2			# 
    ```
  - volume group myvg is successfully extended
    ```
    vgextend myvg /dev/sdb2		
	  ```
# Remove LVM
  ```
  umount /abc
  ```
  ```
  vim /etc/fstab
  ```
  ```
  lvremove /dev/myvg/mylv
  ```
  ```
  vgremove myvg
  ```
  ```
  pvremove /dev/sdb2
  ```
  ```
  pvremove /dev/sdb1
  ```
  ```
  fdisk /dev/sdb
  ```
  - For Check
    ```
    blkid
    ```
  - unplug the disk
