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

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/lsblk2.JPG)






