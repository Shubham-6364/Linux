# Package management/ Software management
- Linux package manager – utility which is used to install software
  -	RPM- REDHET PACKAGE MANAGER
  -	YUM- YELLOW DOG UPDATE MODIFIED
  -	DMF- DEFINED YUM
- While install/uninstall package two script are run
    1.	Pre- script run before package install
    2.	Post- script run after package uninstall
-	To check script run while package install
```
rpm –q –scripts <package_name>
```
-	To check the package is install in our system
```
rpm –q <package-name>
```
-	To check the detail info about the package install in our system
```
rpm –qi <package name>
```
-	To list all file along with package installed in our system
```
rpm –ql <package_name>
```
-	To show the configuration file path
```
rpm –qc <package_name>
```
-	To check installed package  documentation
```
rpm –qd <package_name>
```
-	To list all install packages in our system
```
rpm –qa
```
-	To install package(i-install v-verbis h-hedging)
```
rpm –ivh <package name>
```
-	To uninstall/ erase package
```
rpm –evh <package_name>
```
-	To update packages
```
rpm –Uvh <package_name>
```
# YUM ( Yellowdog Updater Modified 2003)
  - It use the rpm database in the backend
  - yum allows to automatically update/upgrade the system available on your system.
  - it relies entirely on the online repository to do all the work.
  - yum allows you to upgrade your system to the latest available version.
  - yum configuration file path (cat /etc/yum.conf)
-	Install package with yum
```
yum install <package_name> -y
```
-	install package though https link
```
yum install <http link of package>
```
-	Remove package
```
yum remove <package name> -y
```
-	List of available packages in directory/repository
```
yum list all --available
```
-	list of all installed package in our system
```
yum list all --installed
```
-	to check the specific package is installed in our system
```
yum list <package name>
```
-	To get the package name by write the command name
```
yum whatprovides <commandname>
```
-	history of yum command used
```
yum history
```
-	Reinstall the package
```
yum reinstall <package name>
```
-	Update all packages in our system
```
yum update <package name>
```
-	Clean the cache memory of yum
```
yum cleanall
```
-	List of modules available in our repository
```
yum module list
```
-	The version is enable to install 
```
yum module enable <module_name>:<version>
```
-	Install the package
```
yum install <package_name> -y
```
-	The version is disable to install
```
yum module disable <module_name>:<version>
```
-	Reset module in starting state
```
yum module reset <module_name>
```
-	Remove the package completely 
```
yum remove <package_name>* -y
```
# DNF (Defined Yum)
-	list all the repository available
```
dnf repolist all
```
-	Enable/disable the repository 
```
dnf config-manager –set –disabled <repository_name>
```
- To get the information about the package
```
dnf info <package name>
```
-	create Cache memory
```
dnf makecache
```

