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
  
![Screenshot (185)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/ba2e01be-90ba-4fae-985d-d6b74e92300d)

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
   
  ![Screenshot (189)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/77b66be8-3e6e-4229-bc0b-0ffcdbbb6710)
 
* We also install Nmap. It is a network scanning tool. It helps users see the open ports of a targeted machine. It can be installed with this command.
   ```
   sudo apt-get install nmap
   ```
  Just like for hping3, we can check if Nmap has been installed with this command.
   ```
   nmap --version
   ```
  This displays the name and version of the tool.
  
  ![Screenshot (190)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/4a5cc4ae-eac9-41a2-82dc-edc5d03d1465)

### Victim (Kali Linux) 

- We go to the terminal by pressing the terminal icon in the top left corner. 

- Just like in Ubuntu, we update our system. 

  ```
  sudo apt-get update
  ```

- Like in the attacker's machine, we don't need Nmap and hping3 tool in the victim's device. Instead, we need to open our 
  ports, and we need Wireshark to analyse our network traffic. We will host the Apache server in port 80 and allow 
  attackers to attack through port 80. We installed the Apache server with this command.
   ```
   sudo apt install apache2
   ```
   Once it's been installed, you can start it with this command.
   ```
   sudo systemctl start apache2
   ```
   This hosts the Apache server in port 80. We can visit the server by typing our IP address into the web browser.
   
   ![Screenshot (191)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/3718ec0a-b857-42e8-9ca4-38081e89535a)

- We can see our machine's IP address with this command.
  ```
  ifconfig
  ```

  ![Screenshot (192)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/1d1de6a5-a477-43b6-bd36-40d4463846e6)

  As we can see, our IP address is `10.0.2.15`. Let's put it in the browser.

  ![Screenshot (193)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/564bfc2c-5499-442c-88ce-44a8af6eae91)

  The server is working.

- Wireshark comes auto-installed in Kali Linux, so we don't have to install it.

- Since everything we need has been installed, let's turn off our machines and go to our VirtualBox. Going to Settings and clicking the Network button, we should change 
  our Network adapter to a Host-only Adapter for both machines. This isolates our virtual machines from the internet and makes it safe to simulate an attack.

  ![Screenshot (194)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/f384c171-4901-4164-ba43-df506ed4b16e)

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

   ![Screenshot (195)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/cb47842f-fe76-4f08-ba44-1fd49813d5a4)

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
  
  ![Screenshot (196)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/7e92aa60-0af4-4dac-a5b3-2a6ccd79e2ea)

- Now, going to the Attacker machine. Let's scan the Victim machine to see the open ports. The command is
  ```
  nmap 192.168.191.8
  ```
  As we can see, port 80 is open.

  ![Screenshot (199)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/cb266d2b-ec89-455c-8d85-06bd2e78ca9b)

  
- Next, let's open Wireshark in our Victim machine and start capturing the network traffic. This command is
  ```
  wireshark
  ```
  This opens the Wireshark tool. After it's opened, press that blue button to start capturing the file.

  
![Screenshot (198)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/cabc1075-c1a6-4e74-93ef-12c157489795)

- As this is an HTTP port. Let's attack port 80 by flooding SYN packets. This means we send many SYN packets to the Victim but do not complete the three-way handshake by 
  not sending ACK packets. This will use the Victim's resources and eventually overwhelm it. The command is given below.
  ```
  sudo hping3 192.168.191.8 -S -p 80 --rand-source --flood
  ```
  > We are using hping3 tool to attack 192.168.191.8 with SYN packets to port 80. The server will receive SYN packets from random spoofed IP addresses, hiding our real IP 
  address.

  ![Screenshot (203)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/835e1e84-0c79-49a4-9a76-4ae10800404e)

  This will slow down the Victim server and eventually freeze or crash the machine. Before that happens, we go to Wireshark, stop the capture and save it for analysis 
  later.

- This way, we successfully DOS attacked a machine with SYN flood. We can stop the attack by pressing `Ctrl + C` on our keyboard.

# Analysing using Wireshark.  
  We went to our victim machine and opened Wireshark. After that, we import the saved network traffic capture and open it. Now, the network traffic that was captured 
  during the attack is displayed.

  ![Screenshot (204)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/7e75fad5-e435-441f-9ba6-9905d9abbaf9)

  We can see above that there is a lot of incoming TCP traffic. As this is an unusual activity, we want to study it. To filter the SYN packets without any acknowledgement 
  among all the captures, we use the following display filter.

  `tcp.flags.syn == 1 and tcp.flags.ack == 0`
  
  ![Screenshot (205)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/9fc63619-487c-4fa1-b6b0-2be61471d3f9)

  We can see a lot of SYN packets are coming in a very tiny time frame. Although they come from different IP sources, the destination port is 80 for all packets. They are 
  also of identical length of 0 and window size 512. This clearly indicates a TCP SYN flood attack.


  We can also use the I/O graph in the statistics section of Wireshark for visual representation.

  ![Screenshot (207)](https://github.com/davidsunuwar007/DOS-attack-simulation-and-Wireshark-analysis/assets/148152961/d7e3ccf0-7770-4838-abf6-4669d6c5c180)


  As we can see, the traffic has increased from 0 to over 5000 packets per second in a very short period of time.

# Prevention
  - Firewalls can block malicious requests and rate-limit incoming SYN packets.
  - Installing security patches in the system without delay. This minimizes the vulnerabilities attackers might exploit.
  - Implementing Intrusion Prevention System(IPS) to block realtime attacks.
  - Distributing the incoming traffic evenly among different servers. This implementation is called a load balancer. This prevents the resources from exhaustion due to SYN 
    flood attacks.




     
   

      


          







