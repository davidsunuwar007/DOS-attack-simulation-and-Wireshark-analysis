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
  
* Third, we click the start button and start the operating system installation process. This could take several minutes. During the process, we set our username, password and other requirements.
  
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
* We also install Nmap. It is a network scanning tool. It helps users see the open ports of a targeted machine. It can be installed with this command.
   ```
   sudo apt-get install nmap
   ```
  Just like for hping3, we can check if Nmap has been installed with this command.
   ```
   nmap --version
   ```
  This displays the name and version of the tool.

### Victim (Kali Linux) 

- We go to the terminal by pressing the terminal icon in the top left corner. 

- Just like in Ubuntu we update our system. 

  ```
  sudo apt-get update
  ```

- Like in the attacker's machine we don't need Nmap and hping3 tool in the victim's device. Instead, we need to open our ports and we need Wireshark to analyse our network traffic. We will host the Apache server in port 80 and allow attackers to attack through port 80. We installed the Apache server with this command.
```
sudo apt install apache2
```
Once it's been installed you can start it with this command.
```
sudo systemctl start apache2
```
This hosts the Apache server in port 80. We can visit the server by typing our IP address into the web browser.


- We can see the IP address of our machine with this command.
```
ifconfig
```

As we can see, our IP address is xxxxxxxxxxxxxx. Let's put it in the browser.
The server is working.

- Wireshark comes auto-installed in Kali Linux so we don't have to install it.

- Since everything we need has been installed, let's turn off our machines and go to our VirtualBox. Going to Settings and clicking the Network button, we should change our Network adapter to a Host-only Adapter. This isolates our virtual machines from the internet and makes it safe to simulate an attack.

# Attack and Capture
- Let's open our virtual machines, go to the terminal and note our IP addresses. The command is given below.
  > For Attacker(Ubuntu)
   ```
   ip addr
   ```
> For Victim(Kali Linux)
  ```
  ifconfig
  ```
As we can see the Attackers IP address is ``xxxxxxxxxxxxxx`` and the Victims IP address is ``xxxxxxxxx``, we can note it somewhere. We need it in further steps.
  > We see the IP address for Victim has changed. This is because we changed our Network adapter.

- Now, let's ping each other to see if the connection has been established. The command is given below.
> For Attacker
```
ping 192.168.191.6
```
> For Victim(Kali Linux)
```
ping 192.168.191.5
```
As we can see the connection has established. Let's end the ping request with ``Ctrl + Z``.

- Next, let's open Wireshark in our Victim machine and start capturing the network traffic. This command is
```
wireshark
```
This opens the Wireshark tool. After it has opened press that blue button to start capturing the file.

- Now, going back to the Attacker machine. Let's scan the Victim machine to see the open ports. The command is
  ```
  nmap 192.168.191.6
  ```
  As we can see port 80 is open.

- As this is an HTTP port. Let's attack port 80 by flooding SYN packets. This means we send a huge amount of SYN packets to the Victim but do not complete the three-way handshake by not sending ACK packets. This will use the Victim's resources and eventually overwhelm it. The command is given below.
```
hping3 -c 15000 -d 120 -S -w 64 -p 80 --flood --rand-source 192.168.191.6
```
> We are using hping3 tool to send 15000 SYN packets of 120 bytes each(-c 15000 -d 120 -S -w 64) to HTTP web server(-p 80) as fast as possible(--flood) from random spoofed IP addresses hiding our real Attack IP address(--rand-source) to Victim(192.168.191.6).

This will slow down the Victim server and eventually freeze or crash the machine. Before that happens, we go to Wireshark, stop the capture and save it for analysis later.

- This way we successfully DOS attacked a machine with SYN flood. We can stop the attack by pressing `Ctrl = Z` on our keyboard.

# Analysing using Wireshark.  
We go to our Victim machine and open Wireshark. After that, we import the saved network traffic capture and open it. Now the network traffic that was captured during the attack is displayed.


We can see above that there is a lot of incoming TCP traffic. As this is an unusual activity we want to study it. To filter the SYN packets without any acknowledgement among all the captures we use the following filter.
`tcp.flags.syn == 1 and tcp.flags.ack == 0
`
We can see a lot of SYN packets are coming in a very tiny time frame. Although they are coming from different IP sources, the destination port is 80 for all packets. They are also of identical length of 120 and window size 64. This clearly indicates a TCP SYN flood attack



     
   

      


          







