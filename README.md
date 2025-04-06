# DNS_Configuration
DNS, is like the phonebook of the internet. It translates human-friendly domain names into IP addresses that computers use to communicate. DNS servers are the computers that store DNS records and handle the translation (resolution) of domain names to IP addresses. DNS works with different types of "records" that store specific information:  

  A Record: Links a domain name to an IPv4 address. 

  PTR Record: Used for reverse lookups (find a domain from an IP). 

  CNAME Record: Alias for another domain.


Here is the tutorial on how to configure a DNS forward zone and reverse zone: 

If you want more information on DNS, and how it works, please vist [here](https://en.wikipedia.org/wiki/Domain_Name_System)

In this tutorial, I have one Centos(Server) and one Ubuntu(Client) set up on Azure. Need to sign in [azure](https://azure.microsoft.com/en-us/get-started/azure-portal)

Here are the two main configuration files for DNS in linux: /etc/named.conf  & /var/named

My domain name for this tutorial is "learninglinux.com". IP address is my private IP address on eth0


Here are the following steps:

  1. check the package installed on the Centos server

          rpm -qa | grep bind



 2. If the package is not installed, here is the command to install the package.
   
   
        sudo dnf install bind bind-utils -y 

<p align="center"> </p>
<img src="https://imgur.com/5iTCCeZ.png" height="80%" width="80%" >
<br />  

3. Configure DNS server 

          sudo vi /etc/named.conf
<p align="center"> </p>
<img src="https://imgur.com/ICGexSW.png" height="80%" width="80%" >
<br />  
  
4. Edit the line "listen-on port 53 { 127.0.0.1; }"

          listen-on port 53 { 127.0.0.1; 192.168.2.4; };
<p align="center"> </p>
<img src="https://imgur.com/NPxEtPp.png" height="80%" width="80%" >
<br /> 

5. Go to the bottom of the file before “include” line and add:

          zone "lab.local" IN { 
                type master; 
                file "forward.learninglinux"; 
                allow-update { none; }; 
          }; 
         zone "2.168.192.in-addr.arpa" IN { 
               type master; 
               file "reverse.learninglinux"; 
               allow-update { none; }; 
         }; 
        include "/etc/named.rfc1912.zones"; 
        include "/etc/named.root.key";
<p align="center"> </p>
<img src="https://imgur.com/jIHL0eB.png" height="80%" width="80%" >
<br />
<p align="center"> </p>
<img src="https://imgur.com/3Sv2uOQ.png" height="80%" width="80%" >
<br />

Create Zone Files
6. Go to the /var/named file

          cd /var/named
7. Create two file in /var/named

          touch forward.learninglinux
          touch reverse.learninglinx

8. modify the forward.learninglinux file

          vi forward.learninglinux
   
9. Modify the newly created Zone files - Forward zone file
   
            @   IN  SOA     masterdns.learninglinux.com. root.learninglinux.com. ( 
                2011071001  ;Serial 
                3600        ;Refresh 
                      1800        ;Retry 
                604800      ;Expire 
                86400       ;Minimum TTL 
           ) 
           @       IN  NS          masterdns.learninglinux.com. 
           @       IN  A           192.168.2.4
           masterdns     IN  A     192.168.2.4
           clienta             IN  A    192.168.2.5
           clientb             IN  A    192.168.1.241 
   
<p align="center"> </p>
<img src="https://imgur.com/k1D2Eyc.png" height="80%" width="80%" >
<br />
          
10. Modify the reverse.learninglinx file

         vi reverse.learninglinux



11.  Modify the newly created Zone files - reverse zone file
    
          $TTL 86400 
          @   IN  SOA     masterdns.learninglinux.com. root.learninglinux.com. ( 
                         2011071001  ;Serial 
                         3600        ;Refresh 
                         1800        ;Retry 
                         604800      ;Expire 
                          86400       ;Minimum TTL 
         ) 
         @                                   IN  NS          masterdns.learninglinux.com. 
         @                                   IN  PTR         lab.local. 
         masterdns                            IN  A           192.169.2.4
         29                                   IN  PTR         masterdns.learninglinux.com. 
         240                                  IN  PTR         clienta.learninglinux.com. 
         241                                  IN  PTR         clientb.learninglinux.com. 
              
<p align="center"> </p>
<img src="https://imgur.com/gvyqxVl.png" height="80%" width="80%" >
<br /> 

4. Edit the line "listen-on port 53 { 127.0.0.1; }"

          listen-on port 53 { 127.0.0.1; 192.168.2.4; };
<p align="center"> </p>
<img src="https://imgur.com/NPxEtPp.png" height="80%" width="80%" >
<br /> 
