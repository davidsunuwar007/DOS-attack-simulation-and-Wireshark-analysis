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

## Step 2: Install and set up virtual machines.

Now, we set up the virtual machines. We do that by downloading the ISO image files for our virtual machines. I used Kali Linux and Ubuntu, so I downloaded their image ISO files from their official websites. Once they are downloaded, we open VirtualBox and start the setup process.
* First, we click the New button. Name our virtual machine and select the correct version and ISO image. I used Ubuntu as the attacker’s machine, so I named it Attacker and 
chose the Linux 64-bit version.

* Second, we set the minimum RAM size of 2 GB, a Hard disk of 25 GB and 2 CPU processors. This is to make sure that the machine runs smoothly.
* 
* Third, we click the start button and start the operating system installation process. This could take several minutes. During the process, we set our username, password and other requirements.
* 
* Fourth, we do the same steps for the victim’s machine. We name it Victim, choose the Kali Linux ISO file and select the Debian 64-bit version.
Your display should look similar to this.

## Step 3: Installing necessary components in the attacker’s and victim's virtual machine.

### Attacker (Ubuntu)
* After the machine has started, we open the terminal. This can be done by pressing 
              ``Ctrl + Alt + t`` on your keyboard.
  
* Now, the first thing we do is update the system. The command is
  ```
  sudo apt-get update
  ``` 
  For the system to work smoothly, it is crucial to keep the system up to date. This command checks and installs if there are any available updates.
  
* After this, we install the hping3 tool. The command is 
  ```
  sudo apt install -y hping3
  ```
  It is a powerful open-source network tool which allows users to send custom TCP, UDP and ICMP packets to target hosts and analyse network behaviour. We can check if it 
  has been installed with this command.
   ```
   hping3 –version
   ```
* We also install Nmap. It is a network scanning tool. It helps users to see the open ports of a targeted machine. It can be installed with this command.
   ```
   sudo apt-get install nmap
   ```
  Just like for hping3, we can check if nmap has been installed with this command.
   ```
   nmap --version
   ```
  This displays the name and version of the tool.

### Victim (Kali Linux) 

- We go to terminal by pressing the terminal icon in top left corner. 

- Just like in Ubuntu we update our system. 

  ```
  sudo apt-get update
  ```

- Like in Attackers machine we dont need nmap and hping3 tool in Victims device. Instead we need to open our ports and wee need Wireshark to analyse our network traffic. We will host apache server in port 80 and allow attacker to attack through port 80. We install Apache server with this command.
```
sudo apt install apache2
```
Once its been installed you can start it with this command.
```
sudo systemctl start apache2
```
This hosts the apache server in port 80. We can visit the server by typing our ip address in the web browser.


- We can see ip address of our machine with this command.
```
ifconfig
```

As we can see, our ip address is xxxxxxxxxxxxxx.Let's put it in browser.

      


          







