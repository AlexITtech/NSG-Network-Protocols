<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- 

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

- Monitor ICMP Traffic
- Monitor SSH Traffic
- Monitor DHCP Traffic
- Monitor DNS Traffic
- Monitor RDP Traffic

<h2>Actions and Observations</h2>

1.) Begin by creating a resource group to contain both virtual machines. Once the resource group is set up, proceed to create the first virtual machine, which will be a Windows 10 VM. Use the resource group you just created and name the virtual machine "VM1." Select Windows 10 Pro, version 22H2 as the operating system. For the machine size, choose a configuration with at least 2 vCPUs and 16 GB of memory. Create a username and password of your choice, and leave the default inbound port rules unchanged.

![68747470733a2f2f696d6775722e636f6d2f58365a4d544a472e706e67](https://github.com/user-attachments/assets/491df512-0fd7-411d-88ad-25ba785c3e49)


![image](https://github.com/user-attachments/assets/d16b94bd-6e89-4ed3-ad8a-7b53690da191)


 2.) After completing the previous step, click "Next" through each section until you reach the Networking page. At this stage, a virtual network and subnet will be automatically generated for you.
![68747470733a2f2f696d6775722e636f6d2f587a6453506f522e706e67](https://github.com/user-attachments/assets/da57e2a7-37ad-4062-a7bc-7ff5231f772b)

 Click "Review + Create" to finalize and deploy the virtual machine.

With the first VM successfully created, proceed to set up the second virtual machine, which will run Ubuntu Server 20.04 LTS. The steps are similar to those used for the first VM, but this time, instead of using an SSH public key, select password authentication during configuration.

![68747470733a2f2f696d6775722e636f6d2f304b5433466d622e706e67](https://github.com/user-attachments/assets/acac0600-fe0d-4be3-a44e-353c1eaf0b11)



![68747470733a2f2f696d6775722e636f6d2f707978734866462e706e67](https://github.com/user-attachments/assets/0de405e0-2d2e-4f80-b9ce-055b9544ff90)
Click "Next" through each section until you reach the Networking page again.

At this point, the system should automatically use the virtual network and subnet that were created during the setup of the first VM.


![68747470733a2f2f696d6775722e636f6d2f336651585263772e706e67](https://github.com/user-attachments/assets/caabc01d-5b81-455e-a2ab-9e8d9f3b7d9c)
Click "Review + Create" to finalize and deploy the second virtual machine.

2.) With both virtual machines now running, connect to the Windows 10 VM using the Remote Desktop Connection application. Once connected, open a web browser and download Wireshark, then proceed with the installation.

Wireshark is a free and open-source packet analyzer used for network troubleshooting, analysis, software and communication protocol development, and education.

3.) After launching Wireshark, apply a filter to capture only ICMP traffic.

Would you like help with the exact filter syntax or next steps for generating ICMP packets?


![68747470733a2f2f696d6775722e636f6d2f527274436855652e706e67](https://github.com/user-attachments/assets/9f5387f5-1209-4d5b-abc7-90056feb4cdb)

4.) Retrieve the private IP address of the Ubuntu VM, then use the Windows 10 VM to ping it while monitoring the traffic in Wireshark.

To do this, open Command Prompt or PowerShell on the Windows VM and enter the following command, replacing the IP with your Ubuntu VM's private address: ping 10.0.0.5
This action will generate ICMP traffic, which should be visible in Wireshark using the previously applied filter.

![68747470733a2f2f696d6775722e636f6d2f7a6d4a7a796e652e706e67](https://github.com/user-attachments/assets/1dff778d-8a21-45d3-a455-94a0fe58f671)

![68747470733a2f2f696d6775722e636f6d2f707034655a644b2e706e67](https://github.com/user-attachments/assets/65777157-dec4-4a90-8af6-40a7276c4837)
While still in Command Prompt or PowerShell on the Windows 10 VM, run: ping www.google.com

Observe the resulting network traffic in Wireshark to view the outgoing and incoming packets.

5.) Next, initiate a continuous ping from the Windows 10 VM to the Ubuntu VM using its private IP address. In the terminal, use: ping -t 10.0.0.5

This will allow you to continuously generate ICMP traffic between the two machines.

6.) Navigate to the Network Security Group (NSG) associated with the Ubuntu VM. To block incoming ICMP traffic:

Click "Add" to create a new inbound security rule.

Match all the rule settings exactly as provided in the reference or instructions.

Once the rule is added, it will automatically appear in the list of inbound rules, effectively blocking ICMP traffic to the Ubuntu VM.


![68747470733a2f2f696d6775722e636f6d2f723364483359792e706e67](https://github.com/user-attachments/assets/28fdc30e-fe7d-4818-84c2-a4cfaa5557f6)

![68747470733a2f2f696d6775722e636f6d2f716953497273582e706e67](https://github.com/user-attachments/assets/69bdcc2e-3545-4a23-b1c1-9cccd5f19014)

Now that incoming ICMP traffic has been disabled on the Ubuntu VM (VM2), return to the Windows 10 VM (VM1). You will notice that the ping requests are timing out, indicating that ICMP packets are being blocked.

7.) Re-enable ICMP traffic in the Network Security Group (NSG) assigned to the Ubuntu VM by either removing the previously added rule or modifying it to allow ICMP.
Once re-enabled, go back to the Windows 10 VM and observe that:

Ping requests resume successfully in the command line.

ICMP traffic is once again visible in Wireshark.

8.) The next step is to observe SSH traffic between the two virtual machines.




