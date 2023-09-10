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

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/image2.png)

Then we attach all three volumes one by one to your Web Server EC2 instance

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%206/image2.png)

After creating our EC2 server and adding the volumes, we connect to the inctances using:
```ssh -i "mykey.pem" ec2-user@ec2-34-230-24-179.compute-1.amazonaws.com```

















