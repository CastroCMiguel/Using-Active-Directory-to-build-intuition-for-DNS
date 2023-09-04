<p align="center">
<img src="https://i.imgur.com/0SacrRB.jpg" alt="1"/>
</p>

<h1>Understanding DNS</h1>

This lab centers on the practical application of DNS (Domain Name System) and its role in IT. While DNS is a well-documented theoretical concept, this exercise involves hands-on configuration of DNS records to gain practical insights. It builds upon a previous lab where a client was successfully joined to the domain <a href="https://github.com/CastroCMiguel/Configuring-On-premises-Active-Directory-within-Azure-VMs">"mydomain.com"</a> To perform the tasks in this lab, it is essential to be logged in as an administrator, specifically in the personas of Jane Doe, who holds an admin account, on both the client and domain controller VMs.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Active Directory Domain Services
- Command Prompt

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10
  
<h2>Video Demonstration </h2>

### [Building Intuition with DNS](https://www.youtube.com/watch?v=vKIKs8-DiDs)

<h2>DNS Configuration Steps</h2>

<p align="center">
<img src="https://i.imgur.com/wxdcEi9.png" alt="1"/>
</p>

While logged in to the client as an administrator, access the command prompt. Initially, when attempting to ping "mainframe," it will result in failure. Similarly, using "nslookup" for "mainframe" will yield no DNS record found. To address this, a DNS A-record must be established for "mainframe" on the domain controller.

<p align="center">
<img src="https://i.imgur.com/5qYq24j.png" alt="1"/>
</p>

On the domain controller, navigate to the DNS Manager and access the domain created within the Forward Lookup Zones tab, which in this case is "mydomain.com." Right-click on the page and select "New Host." Specify the name as "mainframe," and assign the IP address identical to that of the domain controller, ensuring that ping can resolve successfully. Refresh the DNS server to update the new record.

<p align="center">
<img src="https://i.imgur.com/gpYKtgs.png" alt="1"/>
</p>

Upon returning to the client, attempt to ping "mainframe" once more to observe that it now resolves successfully.

<p align="center">
<img src="https://i.imgur.com/kt3VqPz.png" alt="1"/>
</p>


In this next exercise, we will focus on the DNS cache. On the domain controller, you have modified the record address for "mainframe" to 8.8.8.8 (Google) and refreshed the DNS server. However, when attempting to ping "mainframe" from the client, it will still ping the old IP address. Running the command "ipconfig /displaydns" will reveal that the DNS cache retains the old IP.

<p align="center">
<img src="https://i.imgur.com/9oM1LJT.png" alt="1"/>
</p>

<p align="center">
<img src="https://i.imgur.com/0CRDy3Q.png" alt="1"/>
</p>

To update the cache, it is necessary to clear it. The command "ipconfig /flushdns" will empty the cache. Once the cache is cleared, pinging "mainframe" again will display the updated IP address on the client side. The new IP address of the record will be reflected in the ping results.

<p align="center">
<img src="https://i.imgur.com/H2zRnGY.png" alt="1"/>
</p>

Next, let's create a CNAME record on the DNS server that will direct "search" to Google. Follow these steps:

1. In the DNS Manager, go to the Forward Lookup Zones tab and open the tab that corresponds to the domain, which in your case is "mydomain.com."

<p align="center">
<img src="https://i.imgur.com/vgS7cB5.png" alt="1"/>
</p>

2. Create a new CNAME record named "search" and set it to point to Google.

<p align="center">
<img src="https://i.imgur.com/Mxnf3Hs.png" alt="1"/>
</p>

3. Refresh the DNS server to save the changes.

<p align="center">
<img src="https://i.imgur.com/z9wjMXC.png" alt="1"/>
</p>

Once these DNS configurations are in place, on the client, pinging "search" and using "nslookup" will return the results of the CNAME record, directing it to Google.

<h2>Summary</h2>

This lab has provided me with a clearer understanding of how DNS operates both on an individual computer and within a network. It emphasizes that DNS records can change over time, potentially causing connectivity issues. The DNS cache, which stores these records, may retain old information and needs to be cleared to update to new records. The "ipconfig /flushdns" command, often used in IT troubleshooting, became more comprehensible in terms of its function and purpose through this lab.

In the context of the lab's initial task involving pinging "mainframe," I learned that the DNS cache is the first place checked for a resolution. If no result is found there, the host file is consulted. Only if there are still no results does the system turn to the DNS server. By creating a record on the DNS server, I ensured that a ping to "mainframe" could resolve successfully.

Furthermore, I grasped the concept of a CNAME (Canonical Name) record, which maps an alias name to another domain name. In this case, "search" was used as an alias for Google, showcasing how DNS can be used to create such associations.
