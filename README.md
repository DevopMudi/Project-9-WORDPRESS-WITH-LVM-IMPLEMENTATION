# Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION

Implementing wordpress website with LVM management

This project consist of two parts which are: 

- 1.	Configure storage subsystem for Web and Database servers based on Linux OS(redHat)

- 2.  Install WordPress and connect it to a remote MySQL database server
 
Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.

Three-tier Architecture is a client-server software architecture pattern that comprise of 3 separate layers as illustrated in image below

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/6c0cd422-08d7-4533-88ef-169be6e36723)


The three layers:

1. **Presentation Layer (PL):** This is the user interface such as the client server or browser on your laptop.

2. **Business Layer (BL):** This is the backend program that implements business logic. Application or Webserver

3. **Data Access or Management Layer (DAL)**: This is the layer for computer data storage and data access. Database Server or File System Server such as FTP server, or NFS Server


Our 3 three tier set will be:

1.	A Laptop or PC to serve as a client

2.	An EC2 Linux Server as a web server (This is where we will install WordPress)

3.	An EC2 Linux server as a database (DB) server

**Note: We will use RedHat OS for this project**

The following are steps invlove:

# 1. LAUNCH AN TWO EC2 INSTANCE THAT WILL SERVE AS “WEB SERVER” AND DATABASE SERVER(DB).

Step 1 — Prepare a Web Server

1.	Launch two EC2 instance that will serve as “Web Server” and database server.

   

2.	Then create. Create 3 volumes in the same Area Zone as your Web Server EC2, each of 10 GiB.

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/8d4bfcc5-6dce-4357-805d-3ec7566ae685)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/f2f9aae2-4801-4b19-8a68-24f8e5385a07)


3.	Attach all three volumes one by one to your Web Server EC2 instance and do the same for your database server separately, ensure when attaching they are in the same zonal area for same instance atached to it.(3 volume created attached to webserver, and another 3 volume created attached to database server(db) as shown in the images below

 ![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/c95745b3-77b3-4a19-8f84-c7c2a51b19ab)

 ![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/9b39b06c-08d3-40fa-a8d9-1cdd9d7de252)

 ![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/25cf4c72-9ffa-4493-bbe4-307c93a52471)

 ![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/0cbcfa1d-1bb0-4392-91ee-c731a59d95f8)


4. Open up the Linux terminal to begin configuration for both.

5. Use **lsblk** command to inspect what block devices are attached to the server. Notice names of your newly created devices. All devices in Linux reside in /dev/ directory. Inspect it with **ls /dev/** and make sure you see all 3 newly created block devices there – their names will likely be nvme1n1, nvme2n1, nvme3n1. As display in image below.

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/5a3e19b6-d7d3-40f9-9171-95ed2329a26a)


6. Use **df -h** command to see all mounts and free space on the servers:

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/0fb0bced-9f34-462d-83a4-f8d7cf37bd2c)


7. use **gdisk** to create single partition on each of the three disk for both servers using the command:**sudo gdisk /dev/nvme1n1**


![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/519da085-2fdf-48b9-8e93-98a1625e61fa)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/78a71528-7acd-4e82-9757-652d5abff3fd)

 also do it for **sudo gdisk /dev/nvme2n1**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/ab1dcf9b-6ab3-4c30-a63e-e5d7865a576a)

also do it for **sudo gdisk /dev/nvme3n1**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/aa5ad68c-aab5-41a6-aa0d-1761a1bc101c)


8. Use lsblk command to view the newly created partition on each of the three disk as display below

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/2fde311b-3802-4669-8c28-998f410fb9c9)


9. Install lvm2 package using **sudo yum install lvm2** then Run **sudo lvmdiskscan** command to check for available partitions.

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/e4d3534e-cb63-4880-96a0-276c233d1ec8)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/d62a3070-9430-4310-a47e-9968c0a23af5)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/54b64263-a43f-46c0-9c1c-10e431fc6108)

10. 7.	Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM using the following command:
sudo pvcreate /dev/nvme1n1p1
sudo pvcreate /dev/nvme2n1p1
sudo pvcreate /dev/nvme3n1p1

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/c9d0ef0e-a781-4f38-ab61-a05d331991b2)

11. Verify that your Physical volume has been created successfully by running **sudo pvs**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/2d5da133-580a-40f1-a6fd-f69d44813e9d)


12. Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg using the following command:**sudo vgcreate webdata-vg /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1**

13. Verify that your VG has been created successfully by running:**sudo vgs**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/1e1fd225-e153-4aa8-9e54-ff7405f95e06)

14. Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.use the below command
    
**sudo lvcreate -n apps-lv -L 14G webdata-vg**

**sudo lvcreate -n logs-lv -L 14G webdata-vg**

**Note: for the DB server, the 2 logical volumes created will be created with the following below command**

**sudo lvcreate -n db-lv -L 14G webdata-vg**

**sudo lvcreate -n logs-lv -L 14G webdata-vg**

- Use the **sudo lvs** command to check the logical volumes created, image will be as below
  
![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/bc31d6af-22b3-42e7-bdf9-5f818ebd1484)

15. Verify the entire setup with the following command:**sudo vgdisplay -v #view complete setup - VG, PV, and LV**

**sudo lsblk**
image will be as display below:

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/b9078874-4bc3-4f5c-ab52-e87b0e3eeee0)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/eaa8d8dc-4ee8-4165-8377-2bff9ea49492)


16. Use mkfs.ext4 to format the logical volumes with ext4 filesystem using the below command:

On webserver use the below command:

**sudo mkfs -t ext4 /dev/webdata-vg/apps-lv**
**sudo mkfs -t ext4 /dev/webdata-vg/logs-lv**

Note: on the DB server use the below command:

**sudo mkfs -t ext4 /dev/webdata-vg/db-lv**
**sudo mkfs -t ext4 /dev/webdata-vg/logs-lv**

Image for both servers will be as display below:

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/6b5a3d81-10b9-4554-8e18-4fe8b802664f)

17. Create /var/www/html directory to store website files using the command:**sudo mkdir -p /var/www/html** on the webserver
    also create /var/www/db directory to store data on the DB server using the command: **sudo mkdir -p /var/www/db**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/5b9ffd44-f12d-4f85-b4bb-e1f1239df589)


18.	Create /home/recovery/logs to store backup of log data on both the webserver and the DB server using:**sudo mkdir -p /home/recovery/logs**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/d641bcfc-74b9-41ef-a8d7-abc684788d72)


18.	Mount /var/www/html on apps-lv logical volume using the command: **sudo mount /dev/webdata-vg/apps-lv /var/www/html/**  on webserver.

    Also on the DB server, mount /var/www/db on db-lv logical volume using the command:**sudo mount /dev/webdata-vg/db-lv /var/www/db/**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/7e75fdde-1c65-48f7-b616-e17fc64d9dce)


20.	Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)

  use this command to backup :**sudo rsync -av /var/log/. /home/recovery/logs/** (this is done on both servers)

NOte: this is required before before mounting the file system because whenever you mount it delete all files on the mount file. furthermore /var/log is system file so we dont want to delete it.

After the inputting command and pressing enter, display will be as below:

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/5b2b68a6-6b71-42aa-a374-ccb525eecc54)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/a5356236-a6a7-41cd-b657-307acfaf8441)

21. Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 20 above is very important)
Use this command to mount:**sudo mount /dev/webdata-vg/logs-lv /var/log** on both servers.

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/a6befa9d-6485-4d4e-ac7f-cb47f3fcb243)


22.	Restore log files back into /var/log directory :**sudo rsync -av /home/recovery/logs/log/. /var/log** on both servers

  ![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/cacc10f7-543b-425c-9a38-f9f3f6cd21c6)

  ![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/8418bc4b-0f4a-44d6-be8c-bf691b22474d)


23.	Update /etc/fstab file so that the mount configuration will persist after restart of the server.
The UUID of the device will be used to update the /etc/fstab file; use the following command to access the UUID: **sudo blkid**	

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/d0f87ef9-8e41-4f95-b769-b221620597b6)


24. copy the UUID from the above image and put it on the editor for fstab, access the editor with the command:**sudo vi /etc/fstab**
the format will be as below for the webserver:

**UUID=f8773d15-9c88-412a-a704-1e8a87b1aa8d  /var/www/html  ext4 defaults 0  0**

**UUID=9de5b9d7-135e-4e22-976e-6fcff769e6d8  /var/log   ext4  defaults   0  0**

while format for the Database will be as below

**UUID=223ba7e1-880d-42d0-ab41-87ce96840f19 /var/www/db  ext4 defaults 0  0**

**UUID=d4c6e9f9-22b5-4377-bff9-a5d08fc315cd /var/log ext4   defaults 0  0**

image of display will be as below:

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/b5dec452-74cf-40b2-8b7d-e7fc5284d009)

then press esc :wq! and enter to save and exit

25. Test the configuration and reload the daemon using the command :**sudo mount -a**(this is to test the configuration, if no error), then reload using the command:
    **sudo systemctl daemon-reload**

    Image will be as below:

    ![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/70c4e951-0be6-439e-abee-b6e62e88f1ce)


27.	Verify your setup by running df -h, output must look like this:


![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/ccb113f6-8fb0-42a0-999f-1046345e46e6)


# Step 2 — Install WordPress on your Web Server EC2

1.	Update the repository :**sudo yum -y update**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/a81bd6bd-f4ca-4ddf-b107-1d50317fa419)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/a3717705-3860-479f-89e3-5074d2232591)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/c81a90f6-eb02-4e7c-8b34-1868c4b181d1)

2.	Install wget, Apache and it’s dependencies: **sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/d7b4ee23-86e9-4a02-a606-b75c25267081)



3.	Start Apache using the command:**sudo systemctl enable httpd**  
**sudo systemctl start httpd**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/0f754a4e-7750-4628-a841-26b3bd2e02c5)

4. 	To install PHP and it’s depemdencies, we follow the following steps using the below command:

5.	**sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/84d48e62-bdfd-44bf-947b-db848c058383)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/8edded79-e61c-4a04-a87c-4f4cb1c09656)


6.	**sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm**

7.	**sudo yum module list php**

8.	**sudo yum module reset php**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/f58fd2a2-b826-4eac-9219-dbea77b70fc8)


9.	**sudo yum module enable php**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/33f72f9c-5934-470c-99ae-5edb877a67fd)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/852e345e-d875-443c-a6d9-34a4bd44e3c6)



10.Run this comm **sudo dnf install php php-opcache php-gd php-curl php-mysqlnd**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/3f9b4f5d-37a4-41fb-9d0f-e9a1a0d69e23)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/784695c3-fcf7-49ca-ad31-6e6ff95a7f45)

12.	**sudo systemctl start php-fpm**

13.	**sudo systemctl enable php-fpm**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/d0e29598-f4ef-4436-a991-904c274c764d)


**setsebool -P httpd_execmem 1**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/f4968f91-3a66-4787-9f54-0905240bc483)

14.	Restart Apache with the command :**sudo systemctl restart httpd**

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/53b306a3-d9fa-4d3f-9f89-277f8e9c5a9a)

15. check version of php install with the command: **php -v** display should be as below

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/a056d500-5ab9-483c-90c4-6892b9e35eb2)

# Download wordpress and copy wordpress to var/www/html using the below command

**mkdir wordpress**

**cd   wordpress**

**sudo wget http://wordpress.org/latest.tar.gz**

**sudo tar xzvf latest.tar.gz**

**sudo rm -rf latest.tar.gz**

**cp wordpress/wp-config-sample.php wordpress/wp-config.php**

**cp -R wordpress /var/www/html/**

 ![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/593b8b48-d0dd-4b37-90c7-fe88843f6a71)

# configure SELinux policies  using the below command

 sudo chown -R apache:apache /var/www/html/wordpress

 sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
 
 sudo setsebool -P httpd_can_network_connect=1
 
![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/db5fd6f7-9ae1-48b0-abb1-c53d01eddda0)

 # Step4 : Install mysql on the DB server EC2 using the below command

 sudo yum update

sudo yum install mysql-server

Image display as below

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/74ccbd19-a42c-4a58-8cba-782c86bd6840)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/2e960fe3-1a49-4806-926f-007ff54a40ef)

- Restart and re-enable mysql with the below command:
  
sudo systemctl restart mysqld

sudo systemctl enable mysqld

image display as below

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/df5eaab6-0c87-47b8-8dd1-7c0e91c1b4fa)


# Step5 : Configure the Database to work with wordpress using the following command:

sudo mysql
create database wordpress;
CREATE USER 'wordpress'@'%' IDENTIFIED WITH mysql_native_password BY 'wordpress';
GRANT ALL PRIVILEGES ON *.* TO 'wordpress'@'%' WITH GRANT OPTION;
flush privileges;
exit

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/4ab61358-6ab7-4ba6-af2e-83b44c0dfcd7)

# step6: Install mysql client on the webserver and connect to DB server by using mysqlclient using the following steps/command:

 sudo yum install mysql

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/31c2e209-1d24-427d-bcde-61002df2e1f1)

 Check to see if webserver can connect remotely to the database with following command: **sudo mysql -u admin -p -h <DB-Server-Private-IP-address>**
 remember to input DB server private IP as image below:

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/9738a19a-ffd4-410a-b735-1d9b79acedb1)


bind the database mysqld as image below (run the command :**sudo vi etc/my.cnf** to access page and configure as below

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/92bfdd8d-97eb-4c71-9056-ce2d3ba2d28a)


![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/bfe35f11-553f-439e-b391-42a4de18cb82)


 Run the command: sudo wp-config.php on webserver for configuring as in the image below

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/2804e7a4-fb8c-4514-a8cb-9fa1deb83c7a)


You may rename httpd default page with the below command:

sudo mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf_backup

checking on webserver to see or check if your webserver is connecting to your database:

Image should be as below:

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/20d0398c-0ad2-4b21-ad45-0a4c87e227ab)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/c9d4a28b-75c4-47e3-8992-aa9fa8aaa5fb)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/55a92aa1-1cd5-42b7-ba86-077aa2ca46d3)

![image](https://github.com/DevopMudi/Project-9-WORDPRESS-WITH-LVM-IMPLEMENTATION/assets/149855241/f29ea383-0559-4801-93b3-a56b387b823a)

























































































 
