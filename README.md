<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, I observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>


The first step is to go to https://portal.azure.com/ where we will be creating the Resource Groups and Virtual Machines for this lab.

Create a Resource Group and name it "RG-LAB-02"
<p>
<img src="https://imgur.com/RcbzaSz.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

Once you have created a Resource Group
  - Create 2 Virtual Machines
     - VM1 (Windows 10)
        - Select "RG-LAB-02" in the resource group option
        - Recommended to use a size of atleast 2 vcpu's
        - Use Default Vnet and Subnet
     - VM2 (Linux)
         - Select "RG-LAB-02" in the resource group option
         - Recommended to use a size of atleast 2 vcpu's
         - Be sure to select the same vnet that VM1 is under. This is needed so both VM's will work together.

  *After creating VM1, let the deploying proccess complete 100% and wait a couple minutes for the settings to register*

Trying to make the VM2 before VM1 is done deploying will not allow VM2 to connect to the same Virtual Network.

<img src="https://imgur.com/b0ZRcva.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/MEkL9xP.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/xc7DqO2.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

The next step is to login to the VM1 virtual machine. 
  - Open the Remote Desktop Connection application
  - Find the public IP address for VM1 on the Azure website and copy
  - Paste the IP address into RDC
  - Login with the credentials you used to create the Virtual Machine.

<img src="https://imgur.com/suJvZVg.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/696SrEc.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/cw4RXzG.png" height="20%" width="20%" alt="Disk Sanitization Steps"/>

Once logged in to VM1
  - Search for Wireshark on the web
  - Download the "Windows x64 Installer" version
  - Once file is downloaded, open the file and go through the installation process

<img src="https://imgur.com/X1yJwJg.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/Grd8jmO.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/NvtT8bl.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

Open Wireshark. at the top left corner you will see a blue fin and you will click on it.

Under you will see green line where you will type "icmp" to filter the traffic.

ICMP is the protocol that ping uses. We are trying to test the connectivity between VM1 and VM2.

<img src="https://imgur.com/rh4ZmGl.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

In order to ping VM2
  - Go to the Azure Portal and go to VM2's page where you will look for VM2's Private IP Address
  - Copy The Private IP address
  - Go back to VM1
  - Open "Powershell ISE"
  - type the command "ping 10.0.0.5"
  - You will see a list of Echo (ping) requests and replies

<img src="https://imgur.com/HqTYoAd.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/3ssP86Y.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Next you want to create a perpetual ping for VM2
  - type in the command "ping 10.0.0.5 -t"
  - if successful you will see a long continous ping of VM2

<img src="https://imgur.com/fHqk7QN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Going back to the Azure Portal
  - Search for Netwrok Security Groups
  - Go to VM2 nsg
  - Go to Inbound Security Rules
  - Select Add
  - On the right you will create a rule
      - Select the ICMP protocol
      - Select the Deny action
      - Prioritize it how you'd like, in this lab I made it so it would be prioritized first
      - You can give it any name you choose
  - Save

<img src="https://imgur.com/9TWD9Kf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Going back to the VM1 you will observe that all the request have timed out. We essentially created a firewall to block any traffic from the ping of VM2.

<img src="https://imgur.com/YBx07gG.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

To unblock the traffic
  - Go back to Network Security Groups -> VM2 nsg
  - Inbound Security Rules
  - Click on the rule you just made
  - Adjust the action to now "Allow"
  - Save

<img src="https://imgur.com/A2JAxHt.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

After the rule goes into effect, you will observe that all traffic has resumed.

<img src="https://imgur.com/sbCf2Gm.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

Now we will filter the results to "ssh"

<img src="https://imgur.com/x5CljiF.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

To get an ssh connection
  - Go to powershell
  - Type "ssh labuserjavi@10.0.0.5"
     - You may look different of course. what we're doing is commanding a conncetion to VM2 through ssh, it will require the user of the VM2 and the private IP address
  - Hit "Enter"
  - It may ask you if you are sure if you want to connect, you will type "yes"
  - Next it will ask for VM2's password, you will type that in
    - *it will not show what you are typing on the screen, carefully and correctly type the password in and hit "Enter"*
  - You will know that it connected when you see something similar to this "labuserjavi@VM2:~$"

<img src="https://imgur.com/l2X9OLE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

You have the option to try out different command lines, those will immdeiately pop up on to the wireshark page as you type them in.

To exit out of the ssh connection you simply type "exit" and hit "Enter" and you are now back on VM1's Command prompt.

<img src="https://imgur.com/RyluOeE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>



<img src="https://imgur.com/LDe18hS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

To observe DNS traffic, I used the filter udp.port == 53 and the filter dns as well and the command nslookup. I used the command nslookup followed by the websites of www.google.com and www.disney.com. There I observed the results of that traffi in doing so.

<img src="https://imgur.com/aNQtKda.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

I filtere the traffic to dhcp. I used the comman ipconfig /renew. I hit enter and there I observed the live traffic of dhcp and seeing that my IP was reissued over in the powersheel command.

<img src="https://imgur.com/oi7BiBf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

The filter is tcp.port == 3389. There is non-stop traffic because RDP is constantly showing me a live stream from one computer to another (in my case, my computer accessing the VM that is hosted on Azure)

<img src="https://imgur.com/qS4fb0C.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
