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
   	     
3. Configure DNS server 

          sudo vi /etc/named.conf
4. Edit the line "listen-on port 53 { 127.0.0.1; }"

          listen-on port 53 { 127.0.0.1; 192.168.2.4; }; 
5. Go to the bottom of the file before “include” line and add:

          zone "lab.local" IN { 
type master; 
file "forward.lab"; 
allow-update { none; }; 
}; 
zone "1.168.192.in-addr.arpa" IN { 
type master; 
file "reverse.lab"; 
allow-update { none; }; 
}; 
include "/etc/named.rfc1912.zones"; 
include "/etc/named.root.key"; 
1. check the package installed on the system.

          rpm -qa | grep bind
1. check the package installed on the system.

          rpm -qa | grep bind
1. check the package installed on the system.

          rpm -qa | grep bind
