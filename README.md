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

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

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
</p>
<p>
