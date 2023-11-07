# Creating a Honeypot to Capture Live RDP Cyberattacks on an Exposed VM

In this project, I have configured Azure Sentinel (SIEM) and established its integration with a live virtual machine designed as a honeypot. This setup enables the live monitoring of potential attacks, specifically RDP Brute Force incidents originating from various global locations. To enhance our threat intelligence, a PowerShell script has been developed to extract and investigate the geolocation details of these attackers. The aim is to utilize this information for visualization on the Azure Sentinel Map, providing a geographical perspective of the detected threat origins.

The image below shows a PowerShell script running and getting the location from the event viewer. The script was from joshmadakor1 and it was modified a bit to fit my needs, the script is called Log_Exporter.ps1 and is in this project. The script looks for any event ID that is 4625, that event ID is for failed RDP attempts to connect to the virtual machine. This shows brute force attempts to access my VM, and all the wrong username credentials.

![image](https://github.com/Savier5/Creating-a-Honeypot-to-Capture-Live-RDP-Attacks-on-a-Exposed-VM/assets/55478673/204bac96-87ae-4e58-a881-fc76c17922f1)

The Map after 6 hours of running the VM:
![6h](https://github.com/Savier5/Creating-a-Honeypot-to-Capture-Live-RDP-Cyberattacks-on-a-Exposed-VM/assets/55478673/9fd3e728-1878-4937-8656-8ba62439e44d)

The Map after 12 hours of running the VM:
![12h](https://github.com/Savier5/Creating-a-Honeypot-to-Capture-Live-RDP-Cyberattacks-on-a-Exposed-VM/assets/55478673/e78ec3db-7eed-417b-9e00-c0d20286cb59)

The Map after 24 hours of running the VM:
![24h](https://github.com/Savier5/Creating-a-Honeypot-to-Capture-Live-RDP-Cyberattacks-on-a-Exposed-VM/assets/55478673/324fe9b4-769f-4aa0-871c-ef159e9db73d)

I can give you an overview of how the project was created if you want to replicate it (Note: You would want to put the region to your region so their is no issue with the times of the attack):

  1. You would need to create an account if you don't have one with Microsoft Azure.

  2. Create a Windows 10 Pro virtual machine, the resource group, and the user account to sign into it. For the NIC network security group, you would want to put it to advanced and then create a new one. Then you would delete the inbound rule and add a new one with the destination port range to *, as that would make it any port, and change the priority to 100. After that name the rule anything you want and then create the VM.

  3. Go to the Log Analytics workspace and create a log so we can be able to collect the logs from the VM. Attach the Log to the resource group we created with the VM and then create the log analytic.

  4. Go to Microsoft Defender for Cloud and open your log analytics. You want to turn off everything except for Servers to save money on the credit and won't use the other options.
     
  5. Open Data collection rules and create a rule, set up the settings, and connect it to the resource group we created with the VM. For the Collect and Deliver tab, create a data source, select Windows Event Logs, and then select all the events shown.

  6. Go to the VM and connect to it.

  7. Open Microsoft Sentinel, then create Microsoft Sentinel and connect it to the log analytics we created.

  8. Then get the Public IP address, open a Remote Desktop Connection, and enter the IP address of the VM. Then enter the username and password created when the VM was created.

  9. You want to be able to use the data from Event Viewer to get the attacker's location and I used https://ipgeolocation.io/ to get the locations in the script. You need to create the account and then get an API key and save it to the script (Log_Exporter.ps1). Open the script in the VM and run it. The script will create a folder at C:\ProgramData\ and name it failed_rdp.log. You would want to open another Remote Desktop Connection and put in the wrong credentials, logging into the VM. This will create events in the failed_rdp.log log file.

  10. Copy everything in the failed_rdp.log in the VM and create new one like it into your actual computer. THis will be used to train the 

  11. s

  12. s

  13. s

  14. s

  15. s

  16. s

  17. s

  18. s

  19. s

  20. s
