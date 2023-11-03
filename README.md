# Overview

This is a home-based project. Here, I have simulated a simple TCP SYN flood DOS (Denial of Service) attack and analysed it using Wireshark. In a SYN flood attack, an attacker exploits the three-way handshake protocol TCP uses by sending an overwhelming number of SYN requests to the server. Meanwhile, Wireshark is an open-source network protocol analyser tool that helps professionals capture, inspect and analyse real-time network traffic. The main purpose of this project is to learn how DOS attack works and to gain familiarity with Wireshark. I have created two virtual machines in Oracle VirtualBox. Ubuntu is the attacker’s machine, and Kali Linux is the victim’s. Network packets are captured in the victim’s device with Wireshark.

# Technologies Used

* Oracle VirtualBox
* Ubuntu
* Kali Linux
* Wireshark
  
# Installation and Setup

## Step 1: Install VirtualBox.

In this step, we go to the official Oracle VirtualBox website and install the VirtualBox. My host machine is Windows, so I downloaded the Windows version. You can select yours based on the machine you have. Once the .exe file is downloaded, double-click it and go over a simple installation process.

## Step 2: Install and set up virtual machines.

We set up the virtual machines. We do that by downloading the ISO image files for our virtual machines. I used Kali Linux and Ubuntu, so I downloaded their image ISO files from their official websites. Once they are downloaded, we open VirtualBox and start the setup process.
* First, we click the New button. Name our virtual machine and select the correct version and ISO image. I used Ubuntu as the attacker’s machine, so I named it Attacker and chose the Linux 64-bit version.

* Second, we set the minimum RAM size of 2 GB, a Hard disk of 25 GB, and 2 CPU processors. This is to make sure that the machine runs smoothly.
  
* Third, we set our hostname, username, password and other requirements. After this, the machine starts, and the operating system installation starts.
  
* Fourth, we do the same steps for the victim’s machine. We name it Victim, choose the Kali Linux ISO file and select the Debian 64-bit version.
  For Kali Linux, setting up a hostname, username, and password will only be available during the operating system installation.

  After setting up both of the machines, the VirtualBox looks like this.  
  ![Screenshot (185)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/7f7d893d-f60a-4509-a760-2ce92b3a4aad)

## Step 3: Installing necessary components in the attacker’s and victim's virtual machine.

### Attacker (Ubuntu)
* After starting the machine, we opened the terminal. This can be done by pressing 
              ``Ctrl + Alt + t`` on your keyboard.
  
* Now, the first thing we do is update the system. The command is
        ```
        sudo apt-get update
        ``` 
  For the system to work smoothly, it is crucial to keep it up to date. This command checks and installs if there are any available updates.
  
* After this, we install the hping3 tool. 
        ```
        sudo apt install -y hping3
        ```
  It is a powerful open-source network tool that allows users to send custom TCP, UDP, and ICMP packets to target hosts and analyse network behaviour. We can check if it 
  has been installed with this command.
        ```
       hping3 –version
        ```
   
![Screenshot (189)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/52594fd7-d88a-439b-a59c-1628b5f02c31)
   
* We also install Nmap. It is a network scanning tool. It helps users see the open ports of a targeted machine. It can be installed with this command.
   ```
   sudo apt-get install nmap
   ```
  Just like for hping3, we can check if Nmap has been installed with this command.
   ```
   nmap --version
   ```
  This displays the name and version of the tool.

  ![Screenshot (190)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/a112214c-9cc6-4453-af68-df5ebb92989f)


### Victim (Kali Linux) 

- We go to the terminal by pressing the terminal icon in the top left corner. 

- Just like in Ubuntu, we update our system. 

  ```
  sudo apt-get update
  ```

- Like in the attacker's machine, we don't need Nmap and hping3 tool in the victim's device. Instead, we need to open our ports, and we need Wireshark to analyse our 
  network traffic. We will host the Apache server in port 80 and allow attackers to attack through port 80. We installed the Apache server with this command.
   ```
   sudo apt install apache2
   ```
   Once it's been installed, you can start it with this command.
   ```
   sudo systemctl start apache2
   ```
   This hosts the Apache server in port 80. We can visit the server by typing our IP address into the web browser.

   ![Screenshot (191)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/2dd410a8-cd1f-4bba-bae6-7a92f070c139)



- We can see our machine's IP address with this command.
  ```
  ifconfig
  ```

  ![Screenshot (192)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/718f3a36-abb8-485c-a732-050b4232a30e)

  As we can see, our IP address is `10.0.2.15`. Let's put it in the browser.

  ![Screenshot (193)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/de8c703d-a404-4cd2-aed8-b7ac117079a5)


  The server is working.

- Wireshark comes auto-installed in Kali Linux, so we don't have to install it.

- Since everything we need has been installed, let's turn off our machines and go to our VirtualBox. Going to Settings and clicking the Network button, we should change 
  our Network adapter to a Host-only Adapter for both machines. This isolates our virtual machines from the internet and makes it safe to simulate an attack.
  ![Screenshot (194)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/d5921905-30f5-45e9-b4c5-b91c01279fbc)


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

   ![Screenshot (195)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/80a4a5e1-1de7-4dd0-ac6f-a8789bc76569)

   As we can see, the Attacker's IP address is ``192.168.191.7``, and the Victim's IP address is ``192.168.191.8``. We can note it somewhere.
   > We see the IP address for Victim has changed. This is because we changed our Network adapter.

- Now, let's ping each other to see if the connection has been established. The command is given below.
  > For Attacker
  ```
  ping 192.168.191.8
  ```
  > For Victim(Kali Linux)
  ```
  ping 192.168.191.7
  ```
  As we can see, the connection has been established. Let's end the ping request with ``Ctrl + Z``.

  ![Screenshot (196)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/96c851ac-abff-46dd-847e-5c009397eca6)

- Now, going to the Attacker machine. Let's scan the Victim machine to see the open ports. The command is
  ```
  nmap 192.168.191.8
  ```
  As we can see, port 80 is open.

  ![Screenshot (199)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/09b23a48-9734-4efc-88e5-85c8ac5d5873)
  
- Next, let's open Wireshark in our Victim machine and start capturing the network traffic. This command is
  ```
  wireshark
  ```
  This opens the Wireshark tool. After it's opened, press that blue button to start capturing the file.

  ![Screenshot (198)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/922ffac2-156e-4a24-bd6a-0b186a669dc2)

- As this is an HTTP port. Let's attack port 80 by flooding SYN packets. This means we send many SYN packets to the Victim but do not complete the three-way handshake by 
  not sending ACK packets. This will use the Victim's resources and eventually overwhelm it. The command is given below.
  ```
  sudo hping3 192.168.191.8 -S -p 80 --rand-source --flood
  ```
  > We are using hping3 tool to attack 192.168.191.8 with SYN packets to port 80. The server will receive SYN packets from random spoofed IP addresses, hiding our real IP 
  address.

  ![Screenshot (203)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/6b9d60f8-576e-4cef-bb08-e527d607d7a4)

  This will slow down the Victim server and eventually freeze or crash the machine. Before that happens, we go to Wireshark, stop the capture and save it for analysis 
  later.

- This way, we successfully DOS attacked a machine with SYN flood. We can stop the attack by pressing `Ctrl + C` on our keyboard.

# Analysing using Wireshark.  
  We went to our victim machine and opened Wireshark. After that, we import the saved network traffic capture and open it. Now, the network traffic that was captured 
  during the attack is displayed.

  ![Screenshot (204)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/9c9715aa-6f0e-4bb8-b2ab-0ae729f913fa)

  We can see above that there is a lot of incoming TCP traffic. As this is an unusual activity, we want to study it. To filter the SYN packets without any acknowledgement 
  among all the captures, we use the following display filter.

  `tcp.flags.syn == 1 and tcp.flags.ack == 0`

  ![Screenshot (205)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/30f4e5a9-1132-487e-ae78-1f90913b626f)

  We can see a lot of SYN packets are coming in a very tiny time frame. Although they come from different IP sources, the destination port is 80 for all packets. They are 
  also of identical length of 0 and window size 512. This clearly indicates a TCP SYN flood attack.


  We can also use the I/O graph in the statistics section of Wireshark for visual representation.

  ![Screenshot (207)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/e803c63e-3ec1-40c5-9dbf-b45cd53708d9)

  As we can see, the traffic has increased from 0 to over 5000 packets per second in a very short period of time.

  #Prevention
  - Firewalls can be used to block malicious requests and rate-limit incoming SYN packets.
  - Installing security patches in the system without delay. This minimizes the vulnerabilities attackers might exploit.
  - Implementing Intrusion Prevention System(IPS) to block realtime attacks.
  - Distributing the incoming traffic evenly among different servers. This implementation is called a load balancer. This prevents the resources from exhaustion due to SYN 
    flood attacks.




     
   

      


          







