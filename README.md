# Creating a Honeypot to Capture Live RDP Cyberattacks on an Exposed VM

In this project, I have configured Azure Sentinel (SIEM) and established its integration with a live virtual machine designed as a honeypot. This setup enables the live monitoring of potential attacks, specifically RDP Brute Force incidents originating from various global locations. To enhance our threat intelligence, a PowerShell script has been developed to extract and investigate the geolocation details of these attackers. The aim is to utilize this information for visualization on the Azure Sentinel Map, providing a geographical perspective of the detected threat origins.

The image below shows a PowerShell script running and getting the location from the event viewer. The script was from joshmadakor1 and it was modified a bit to fit my needs, the script is called Log_Exporter.ps1 and is in this project. The script looks for any event ID that is 4625, that event ID is for failed RDP attempts to connect to the virtual machine. This shows brute force attempts to access my VM, and all the wrong username credentials.

![image](https://github.com/Savier5/Creating-a-Honeypot-to-Capture-Live-RDP-Attacks-on-a-Exposed-VM/assets/55478673/204bac96-87ae-4e58-a881-fc76c17922f1)

The honeypot saw that attackers were using these usernames to try to access my VM:

d'utilisateurs | comptes | Administrateur | \\ | de | Invit‚ | admin | Administrator | Test | 的用户帐户 | for | accounts | User | administrator | STUDENT | box2 | Diag | ESRAYA | CATicala | ArkMGWarranty | ADMINISTRATOR | ArkMGLiaison | ADMIN | APAranda | AManlises | AGJARINA | win | AZUREADMIN | ACruz | BClaudio | ArkTechnician | ARKSERVER

The first 6 usernames had 3800+ failed login attempts in the list and I presume they came from Tunisia as a great amount of traffic came from Tunisia.

The Map after 6 hours of running the VM:
![6h](https://github.com/Savier5/Implementing-a-RDP-Honeypot-in-Azure/assets/55478673/909e9b26-e513-4ad9-a210-89a7896a4532)

The Map after 12 hours of running the VM:
![12h](https://github.com/Savier5/Implementing-a-RDP-Honeypot-in-Azure/assets/55478673/903f6cac-7c45-4653-9c13-74da48679d6d)

The Map after 24 hours of running the VM:
![24h](https://github.com/Savier5/Creating-a-Honeypot-to-Capture-Live-RDP-Cyberattacks-on-a-Exposed-VM/assets/55478673/324fe9b4-769f-4aa0-871c-ef159e9db73d)

I can give you an overview of how the project was created if you want to replicate it (Note: You would want to put the region to your region so there is no issue with the times of the attack):

  1. You would need to create an account if you don't have one with Microsoft Azure.

  2. Create a Windows 10 Pro virtual machine, the resource group, and the user account to sign into it. For the NIC network security group, you would want to put it to advanced and then create a new one. Then you would delete the inbound rule and add a new one with the destination port range to *, as that would make it any port, and change the priority to 100. After that name the rule anything you want and then create the VM.

  3. Go to the Log Analytics workspace and create a log so we can be able to collect the logs from the VM. Attach the Log to the resource group we created with the VM and then create the log analytic.

  4. Go to Microsoft Defender for Cloud and open your log analytics. You want to turn off everything except for Servers to save money on the credit and won't use the other options.
     
  5. Open Data collection rules and create a rule, set up the settings, and connect it to the resource group we created with the VM. For the Collect and Deliver tab, create a data source, select Windows Event Logs, and then select all the events shown.

  6. Go to the VM and connect to it.

  7. Open Microsoft Sentinel, then create Microsoft Sentinel and connect it to the log analytics we created.

  8. Then get the Public IP address, open a Remote Desktop Connection, and enter the IP address of the VM. Then enter the username and password created when the VM was created.

  9. You want to be able to use the data from Event Viewer to get the attacker's location and I used https://ipgeolocation.io/ to get the locations in the script. You need to create the account and then get an API key and save it to the script (Log_Exporter.ps1). Open the script in the VM and run it. The script will create a folder at C:\ProgramData\ and name it failed_rdp.log. You would want to open another Remote Desktop Connection and put in the wrong credentials, logging into the VM. This will create events in the failed_rdp.log log file.

  10. Copy everything in the failed_rdp.log in the VM and create a new one like it into your actual computer, this will be used as a sample. 

  11. Go to Log Analytics workspace, then your log, click on Tables, create, and New custom logs (MMA-based) to create a custom log. Select the sample log file on your actual computer. In the Collection Path, put Windows, the path of the log file in the VM (C:\ProgramData\failed_rdp.log), name it something like FAILED_RDP then finish the setup.

  12. Open Microsoft Sentinel, go to Workbooks and add a workbook. Edit and remove the ones already there, then add a query. Add this in the query:

      FAILED_RDP_CL 
        | extend username = extract(@"username:([^,]+)", 1, RawData),
         timestamp = extract(@"timestamp:([^,]+)", 1, RawData),
         latitude = extract(@"latitude:([^,]+)", 1, RawData),
         longitude = extract(@"longitude:([^,]+)", 1, RawData),
         sourcehost = extract(@"sourcehost:([^,]+)", 1, RawData),
         state = extract(@"state:([^,]+)", 1, RawData),
         label = extract(@"label:([^,]+)", 1, RawData),
         destination = extract(@"destinationhost:([^,]+)", 1, RawData),
         country = extract(@"country:([^,]+)", 1, RawData)
      | where destination != "samplehost"
      | where sourcehost != ""
      | summarize event_count=count() by latitude, longitude, sourcehost, label, destination, country

 Change the visualization to map, and make the map size to Full to get a bigger map. In the map settings, you can change the settings to show the latitude, longitude, label for the countries, etc. 

 You are all done! You need to leave the VM running to keep the script running and it should start populating with attacks at different locations in just a few hours.
