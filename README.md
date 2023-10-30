# Overview
This is a home-based project. Here, I have simulated a simple SYN flood DOS (Denial of Service) attack and analysed it using Wireshark. The main purpose of this project is to learn how DOS attack works and to gain familiarity with Wireshark. I have created two virtual machines in Oracle VirtualBox. Ubuntu is the attacker’s machine and Kali Linux is the victim’s machine. Network packets are captured in the victim’s device with Wireshark.

# Technologies Used
* Oracle VirtualBox
* Ubuntu
* Kali Linux
* Wireshark
  
# Installation and Setup

## Step 1: Install VirtualBox.
In this step, we go to the official Oracle VirtualBox website and install the VirtualBox. My host machine is Windows, so I downloaded the Windows version. You can select yours based on the machine you have. Once the .exe file is downloaded you can double-click it and go over a simple installation process.

## Step 2: Install and setup virtual machines.
Now, we set up the virtual machines. We do that by downloading the ISO image files for our virtual machines. I used Kali Linux and Ubuntu, so I downloaded their image ISO files from their official websites. Once they are downloaded, we open VirtualBox and start the setup process.
* First, we click the New button. Name our virtual machine and select the right version and ISO image. I used Ubuntu as the attacker’s machine, so I named it “Attacker” and 
chose the Linux 64-bit version.
* Second, we set the minimum RAM size of 2 GB, a Hard disk of 25 GB and 2 CPU processors. This is to make sure that the machine runs smoothly.
* Third, we click the start button and start the operating system installation process. This could take several minutes. After it is finished, we click Setting, go to the Network section and change the settings to Host-only Adaptor to make sure *******
* Fourth, we do the same steps for the victim’s machine. We name it “Victim”, choose the Kali Linux ISO file and select Debian 64-bit version.
Your display should look similar to this.

