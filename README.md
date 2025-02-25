<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Create a Resource Group
- Create a Virtual Machine
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>
</br>
</br>
<h3 align="center">
  Set up your virtual environment
</h3>
</br>
<p>
  First, let's create our Resource Group inside our Azure subscription.
</p>
<p>
  
 ![making RG](https://github.com/user-attachments/assets/ec05874d-6007-4ff4-abd2-c0cbdb8389b9)

</p>
<p>
  Now create your Windows virtual machine. I typically create the VM in (US) US West 2.
</p>
<p>
  While creating the VM, select the previously created Resource Group and allow it to create a new Virtual Network (Vnet) and Subnet. Make sure to use the password option under the <strong>Administrator Account</strong> section:
</p>
<p>
  
![making windows vm](https://github.com/user-attachments/assets/0bc45ffb-7d9a-41c4-8e42-ba1ebed5085a)

</p>
<p>
  Create an Ubuntu virtual machine.
</p>
<p>
  While creating the VM, select the previously created Resource Group and allow it to create a new Virtual Network (Vnet) and Subnet. Make sure to use the password option under the <strong>Administrator Account</strong> section (not seen in image):
</p>
<p>
  
![making linux vm](https://github.com/user-attachments/assets/fe0e7766-4456-409b-a483-985d47ec7184)

</p>
<p>
  Observe Your Virtual Networks Topoglogy to insure you're on the same virtual network:
</p>
<p>
  
![same network](https://github.com/user-attachments/assets/ba4f9181-36a5-418a-8553-3ccb61ce792e)

</p>
<br />
<br />
<h3 align="center">
  Now let's observe some ICMP traffic
</h3>
<br />
<p>
  Remote into your Windows 10 Virtual Machine, install Wireshark, open it and filter for ICMP traffic only.
</p>
<p>
  
  ![log into windows vm](https://github.com/user-attachments/assets/3da911ed-256e-4167-b484-e3972e6492de)
  ![download wireshark](https://github.com/user-attachments/assets/96715f8a-514c-4614-aa05-dfa3f8443fc3)

</p>
<p>
  Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM. Observe ping requests and replies within WireShark:
</p>
<p>
  
 ![use linux private ip](https://github.com/user-attachments/assets/39a59402-96fa-4e5f-98b9-fae3008efce4)
![ping linux](https://github.com/user-attachments/assets/c105f5d9-ce4c-418a-a1a1-b709a28dd77e)

</p>
<p>
  Attempt to ping a public website (such as www.google.com) and observe the traffic in WireShark:
</p>
<p>
  
![ping google](https://github.com/user-attachments/assets/d7695aa7-c2c7-4c4b-b596-b66cbc6e6305)

</p>
<p>
  Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM:
</p>
<p>
  
![ping indefinetly linux](https://github.com/user-attachments/assets/90c49572-6217-45cf-b8cc-87734699bbbd)

</p>
<p>
  Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic, while back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity:
</p>
<p>
  
  ![deny icmp on linux](https://github.com/user-attachments/assets/3f25f91e-c67c-4c19-a0d9-d05e1e3893be)
![observe icmp time out](https://github.com/user-attachments/assets/a96143c2-2174-40d8-bba6-9e1b1fa29fa3)

</p>
<p>
  Re-enable ICMP traffic for the Network Security Group in your Ubuntu VM and back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line ping activity (should start working again).Finally, stop the ping activity:
</p>
<p>
  
  ![allow icmp traffic](https://github.com/user-attachments/assets/316dc3a7-0600-4f6a-85be-730768c46b75)
![observe icmp after allowing traffic again](https://github.com/user-attachments/assets/ac8ca428-7cde-4c34-ae1a-9da210682a76)

</p>
<br />
<br />
<h3 align="center">
  Time to observe SSH traffic
</h3>
<br />
<p>
  Back in Wireshark, filter for SSH traffic only and from your Windows 10 VM, “SSH into” your Ubuntu virtual machine (via its private IP address). Type commands (ls, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark.
</p>
</p>

![ssh traffic](https://github.com/user-attachments/assets/e6d05d6c-4e49-44a4-bacb-2487573aeecb)

 Exit the SSH connection by typing ‘exit’ and pressing [return]:
</p>
<p>
<br />
<br />
<h3 align="center">
  Next, we're going to observe DHCP Traffic
</h3>
<br />
<p>
  Back in Wireshark, filter for DHCP traffic only. From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)
</p>
Observe the DHCP traffic appearing in WireShark:
</p>
<p>
  
![dhcp ip renew](https://github.com/user-attachments/assets/0a61d5f8-d872-4d6d-8ddd-2c023a212bfe)

</p>
<br />
<br />
<h3 align="center">
  Let's now observe our DNS traffic next
</h3>
<br />
<p>
  Back in Wireshark, filter for DNS traffic only.
</p>
<p>
  From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are and observe the DNS traffic being shown in WireShark:
</p>
<p>
  
 ![nslookup](https://github.com/user-attachments/assets/01f54adb-c32a-4b37-be89-b91cbace131f)

</p>
<br />
<br />
<h3 align="center">
  Finally, we will observe RDP traffic to finish up this tutorial
</h3>
<br />
<p>
  Back in Wireshark, filter for RDP traffic only using "tcp.port==3389".
</p>
<p>
  You'll be obseving a non-stop stream of traffic. Do you know why there is constant traffic in our tcp.port==3389?
</p>
<p>
  The answer is because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefor traffic is always being transmitted:
</p>
<p>
  
 ![rdp](https://github.com/user-attachments/assets/d899243b-eba5-4f6f-88f9-4ce8040a3183)

</p>
<p>
  Now that we're finished observing the network, DON'T FORGET TO CLEAN UP YOUR AZURE ENVIRONMENT! This will prevent you from incurring additional charges and you won't be left surprised!
</p>
<p>
  Close your Remote Desktop connection, delete the Resource Group(s) created at the beginning of this tutorial, and verify Resource Group deletion. You'll typically be notified or can click unde the bell notification just to make sure.
</p>
</p>
