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

