<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.
Practicing adding DNS A-records and CNAME.
Command lines in Powershell.,<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell (run as administrator)

<h2>Operating Systems Used </h2>

- Windows Server 2022 (DC-1)
- Windows 10 (22H2) (Client-1)

<h2>High-Level Deployment and Configuration Steps</h2>

***Prereqs: Create an Azure VM DC-1 (Windows Server 2022) and Create an Azure VM Client-1 (Windows 10 (22H2))***

***A-Record***
- Step 1: Remote Desktop and 'Log into' the DC-1 as your domain admin account (mydomain.com\jane_admin). And also, Log into Client-1 as an admin (mydomain\jane_admin)
- Step 2: From Client-1, Open Powershell (as an admin) and try to (ping “cherry”) and notice that it fails. Type: (Nslookup “mainframe”) and notice that it fails (no DNS record)
- Step 3: Create a DNS A-record on DC-1 for “cherry” and have it point to DC-1’s Private IP address. To (ping "cherry"), 
- Step 4: Go back to Client-1 and try to ping it. Observe that it works.

***Local DNS Cache Exercise***
- Step 1: Go back to DC-1 and change cherry’s record address to 8.8.8.8. 
- Step 2: Go back to Client-1 and ping “cherry” again. Observe that it still pings the old address
- Step 3: Flush the DNS cache (ipconfig /flushdns). Observe that the cache is empty (ipconfig /displaydns). Attempt to ping “cherry” again. Observe the address of the new record is showing up.

***CNAME Record Exercise***
- Step 1: Go back to DC-1 and create a CNAME record that points the host “search” to “www.google.com”
- Step 2: Go back to Client-1 and attempt to ping “search”, and observe the results of the CNAME record
- Step 3: On Client-1, nslookup “search”, observe the results of the CNAME record
  

<h2>Deployment and Configuration Steps</h2>


***A-Record***
<p>
<p>
Step 1: Open Remote Desktop and 'Log into' the DC-1 as your domain admin account (mydomain.com\jane_admin). Also, 'Log into' Client-1 as an admin (mydomain\jane_admin)
  
![image](https://github.com/user-attachments/assets/7f8a7d70-c2ac-4d4d-8608-4b95372e4a18)


![image](https://github.com/user-attachments/assets/f07aa873-0ad7-41a4-ac6b-238969b79ff4)

</p>

</p>
<br />

<p>

Step 2: From Client-1, On "Type here to search" located on the bottom left corner next to the 'Windows icon', Type: Powershell. Select: 'run as administrator' and select 'Yes'.

![image](https://github.com/user-attachments/assets/3efc85ac-a7b6-4bb7-a792-45533a528339)

       Type: (ping cherry) in PowerShell. Observe that it fails.(This is due to no DNS record)

![image](https://github.com/user-attachments/assets/6e86d39d-687f-47a3-a7a0-bdf1e330477e)

       Type: (Nslookup “cherry”) and notice that it fails. (This is due to no DNS record) 

![image](https://github.com/user-attachments/assets/9f04346a-b4b3-4f26-9873-50f910f9fd05)

</p>

</p>
<br />

<p>
  
Step 3: Select the 'Windows icon' at the bottom left corner. Select 'Windows Administrative Tools' and on the drop categories, select 'DNS'. The "DNS Manager" window appears. Select the 'DC-1' file, then select the 'Foward Lookup Zones'file, then select the 'mydomain.com' file. 

![image](https://github.com/user-attachments/assets/c83062af-9dd2-4010-aa12-abd92ec7e611)

      To (ping "cherry"), we need to create a DNS A-record on DC-1. Use right-click in the file screen and select ' New Host (A) or (AAA)...' 

![Screen Shot 2025-01-09 at 2 13 43 PM](https://github.com/user-attachments/assets/2876e8dc-ff27-43e9-b551-c9b29255cbd3)

      Under 'Name', type: cherry. Under 'IP Address' Type: 10.0.0.4 (DC-1’s Private IP address). Select the box 'Create associated pointer (PTR) record'. Click on 'Add Host'.

![image](https://github.com/user-attachments/assets/67c7e507-cf2c-4812-b611-dda2f4de4978)

      Observe the 'cherry' record has been added.
![image](https://github.com/user-attachments/assets/c1e9f91e-ef0c-4784-b8d4-f2fa2d9bbc6f)


</p>
<p>

<br />

<p>
Step 4: Go back to Client-1 and try to (ping cherry). Observe that it works.
</p>
  
![ping works](https://github.com/user-attachments/assets/bf351fa5-3948-4ae4-97b0-0dd9e083c22a)

</p>

<br />

***Local DNS Cache Exercise***

<p>
Step 1: Return to 'DNS Manager" by Selecting the Windows icon, and selecting the 'Windows Administrative Tools'. Select the 'DNS' option.
        Select the 'DC-1' file, then select 'Foward Lookup Zones', and select 'mydomain.com'.
        Double-click on the cherry a-record. Change the IP address to (8.8.8.8.) Select the box for 'Update associated pointer (PTR) record'. Hit 'Apply' and 'Ok'. View changes.
</p>

![changed IP ](https://github.com/user-attachments/assets/bc6c45b1-8dac-4885-80f3-de404e868a1f)

![image](https://github.com/user-attachments/assets/d8c61d58-8a36-46c6-80d8-aa725d0e517e)

  
</p>

<br />

<p>
Step 2: Go back to Client-1. Open Powershell (run as administrator). Type: ping “cherry”. Observe that it still pings the old (10.0.0.4) IP address. 
  
![image](https://github.com/user-attachments/assets/55b26964-cf31-4341-9822-34305e80a403)

      This is due to the Local Cache stored on the host. To view this Type: "ipconfig /displaydns" in Powershell. Observe the cherry record's IP address is 10.0.0.4.

![image](https://github.com/user-attachments/assets/12aa9271-9a68-49b7-9d88-d8481f1ed76c)

</p>

<br />
<p>
Step 3: In order to view the cherry a-record with it's updated (8.8.8.8) IP address, we must flush the DNS. Therefore, type: "ipconfig /flushdns" (again make sure you are in Powershell and running as administrator). Notice that it was flushed successfully.

![image](https://github.com/user-attachments/assets/dabccc35-285b-47ca-9220-44057dbbd535)

      Next, type: ping cherry. Notice that the cherry record IP address is now 8.8.8.8

![image](https://github.com/user-attachments/assets/de3c5052-b79d-4bf6-a3be-3e143479c5b5)

      Verify results type: "ipconfig /displaydns"

![image](https://github.com/user-attachments/assets/65c393e7-68e8-4a44-8afd-c10538e6e0f7)
           
</p>

<br />
<p>

***CNAME Record Exercise***
  
</p>
Step 1: Return to 'DNS Manager" by Selecting the Windows icon, and selecting the 'Windows Administrative Tools'. Select the 'DNS' option.
        Select the 'DC-1' file, then select 'Foward Lookup Zones', and select 'mydomain.com'. Right-click on the screen and select 'New Alias (CNAME' option. 

![image](https://github.com/user-attachments/assets/6529cf66-44b6-487f-b727-d6e6d5bab73b)

      Next, under 'Alias name (uses parent domain if left blank)' type: 'search'. Under 'fully qualified domain name (FQDN) for target host' type: 'www.google.com'. Select, 'Ok'.
      
![image](https://github.com/user-attachments/assets/7ba93614-250d-430f-a524-be405f47e99e)

Step 2: Go back to Client-1. In Powershell attempt to Type: "ping search”, and observe the results of the CNAME record

![image](https://github.com/user-attachments/assets/b374e920-a058-4882-b9ee-df08dd20169d)

Step 3: On Client-1. In Powershell type: "nslookup search”, observe the results of the CNAME record. (IPv4 and IPv6 addresses for www.google.com are shown)

![image](https://github.com/user-attachments/assets/e22ef6d6-6749-45dc-a520-6d3c533c27ab)

           
</p>

<br />
