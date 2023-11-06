# Creating a Honeypot to Capture Live RDP Cyberattacks on a Exposed VM

In this project, I have configured Azure Sentinel (SIEM) and established its integration with a live virtual machine designed as a honeypot. This setup enables the live monitoring of potential attacks, specifically RDP Brute Force incidents originating from various global locations. To enhance our threat intelligence, a PowerShell script has been developed to extract and investigate the geolocation details of these attackers. The aim is to utilize this information for visualization on the Azure Sentinel Map, providing a geographical perspective of the detected threat origins.

The image below shows my PowerShell script running and getting the location from the event viewer. The script looks for any event ID that is 4625, that event ID is for failed RDP attempts to connect to the virtual machine. This shows brute force attempts to access my VM, and all the wrong username credentials.

![image](https://github.com/Savier5/Creating-a-Honeypot-to-Capture-Live-RDP-Attacks-on-a-Exposed-VM/assets/55478673/204bac96-87ae-4e58-a881-fc76c17922f1)

