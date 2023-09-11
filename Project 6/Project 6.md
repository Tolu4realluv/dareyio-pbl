# DEPLOYING A THREE-TIER ARCHITECTURE WEB SOLUTION WITH WORDPRESS ON AWS.
---

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/3-tier.jpg)

## What is a Thee tier Architecture?

A three-tier architecture is a client-server architecture in which the functional process logic, data access, computer data 
storage, and user interface are developed and maintained as independent modules on separate platforms. The three-tier architecture
is the most popular implementation of a multi-tier architecture and consists of a single presentation tier, logic tier, and data tier. The following illustration shows an example of a simple, 
generic three-tier application.

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/image2.png)

*1. Presentation Layer (PL): This is the user interface, such as the client-server or browser on your laptop.

*2. Business Layer (BL): This is the backend program that implements business logic. Application or Webserver

*3. Data Access or Management Layer (DAL): This is the layer for computer data storage and data access. Database Server or File System Server such as FTP server or NFS Server

In this project, we will have the hands-on experience that showcases Three-tier Architecture while also ensuring that the disks used to store files on the Linux servers are adequately partitioned and managed through programs such as gdisk and LVM respectively.

## Our 3-Tier Setup
1. A Laptop or PC to serve as a client
2. An EC2 Linux Server as a web server (This is where we will install WordPress)
3. An EC2 Linux server as a database (DB) server.

We will be working working with several storage and disk management concepts, this [Video](https://www.youtube.com/watch?v=2Z6ouBYfZr8) provides more insights.

> NB: For this project, we are using the RedHat OS to give us a deeper understanding of different Linux distros.
 
## Launching an EC2 Instance that will serve as a "web server"
We Launch an EC2 instance that will serve as “Web Server”. And create 3 volumes in the same AZ as our Web Server EC2, each of 10 GiB. This
[Video](https://www.youtube.com/watch?v=HPXnXkBzIHw) shows us How to Add EBS Volume to an EC2 instance.

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/add%20volumes.png)

Then we attach all three volumes one by one to your Web Server EC2 instance

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/attach%20volumes.png)

After creating our EC2 server and adding the volumes, we connect to the inctances using:

```ssh -i "mykey.pem" ec2-user@ec2-34-230-24-179.compute-1.amazonaws.com```

We use ```lsblk``` command to inspect what block devices are attached to the server. we notice names of your newly created devices. All devices in Linux reside in /dev/ directory. We inspect it with ```ls /dev/``` and make sure we see all 3 newly created block devices there – their names are xvdf, xvdg, & xvdh as shown in the image below.

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/lsblk.JPG)

Then we used the  ```df -h``` command to see all mounts and free space on our server, then we used ```gdisk``` utility to create a single partition on each of the three disks with the command:

```sudo gdisk /dev/xvdf```

We follow the utility gdisk utility commands through like this:

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/partition.JPG)

Then we repeat the same steps to create a single partition for the other two drives.

After successfully creating the partition, we can run the ```lsblk``` command again to be sure the partitions were created.

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/lsblk2.JPG)

This shows that our partitions were successfully created.

We now proceed to Install lvm2 package using ```sudo yum install lvm2```, then we Run ```sudo lvmdiskscan``` command to check for available partitions.

> NB: Previously, in Ubuntu we used apt command to install packages, in RedHat/CentOS a different package manager is used, so we used yum command instead.

We used pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1

```

To verify that our Physical volume has been created successfully, we run:

```sudo pvs```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/pvs.JPG)

We used vgcreate utility to add all 3 PVs to a volume group (VG). And named the VG webdata-vg

```sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1```

To verify that our volume group is created, we run:

```sudo vgs```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/vgs.JPG)

We used lvcreate utility to create 2 logical volumes. apps-lv (using half of the PV size), and logs-lv using the remaining space of the PV size. 
> NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.

```
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg

```

To verify that your Logical Volume has been created successfully we run:

```sudo lvs```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/lvs.JPG)

To verify the entire setup we run:

```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk

```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/lsblk3.JPG)

We used mkfs.ext4 to format the logical volumes with ext4 filesystem

```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv

```

We created /var/www/html directory to store website files and /home/recovery/logs to store backup of log data we then mount /var/www/html on apps-lv logical volume

```
sudo mkdir -p /var/www/html
sudo mkdir -p /home/recovery/logs
sudo mount /dev/webdata-vg/apps-lv /var/www/html/

```

We used rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system) 

```sudo rsync -av /var/log/. /home/recovery/logs/```

We mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why it is important to backup the log files before we mount.)

```sudo mount /dev/webdata-vg/logs-lv /var/log```

We then restored our log files back into /var/log directory

```sudo rsync -av /home/recovery/logs/log/. /var/log```

We updated /etc/fstab file so that the mount configuration will persist after restart of the server.
The UUID of the device will be used to update the /etc/fstab file;

```sudo blkid```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/blkid.JPG)

We updated /etc/fstab in the format below using our own UUID and we removed the leading and ending quotes

```sudo vi /etc/fstab```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/blkid.JPG)


UUID="63a4ea9c-3c13-46c6-80d3-e103e9525c47" BLOCK_SIZE="4096" TYPE="ext4"
UUID="464be630-15ef-4850-b90b-9b0a5e7e2c49" BLOCK_SIZE="4096" TYPE="ext4"
