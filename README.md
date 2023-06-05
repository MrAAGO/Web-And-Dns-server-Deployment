# Web-And-DNS-Server-Deployment in Cloud.

![screenshot-app excalidraw com-2023 04 26-10_23_27](https://user-images.githubusercontent.com/86381942/234655195-1800fbb6-8d9f-40f1-b4b4-c51e031f88e7.png)


<!DOCTYPE html>
<html>
  <head>
</head>
  <body>
    <h1>Project Journey</h1>
    <nav>
      <ul>
        <li><a href="#introduction">Introduction</a></li>
        <li><a href="#Server">Running Server in Cloud</a></li>
        <li><a href="#dns">Getting a Domain Name</a></li>
        <li><a href="#bind">Installing a DNS Server(BIND9)</a></li>
        <li><a href="#auth">Setting Up the Authoritative BIND9 DNS Serve(BIND9)</a></li>
        <li><a href="#ap2">Installing a Web Server (Apache2)</a></li>
        <li><a href="#up">Setting Up Virtual Hosting (Apache2)</a></li>
        <li><a href="#ssl">Securing Apache with OpenSSL and Digital Certificates(Apache2))</a></li>
        <li><a href="#access">Access Control by Source IP Address</a></li>
        <li><a href="#file">The <Files> Directive</a></li>
        <li><a href="#ht">The .htaccess File</a></li>  
    
        
   </ul>
 </nav>
    
 <section id="introduction">
    <h2>Introduction</h2>
      

  <p>In this Projects, we will learn how to set up, configure, and secure DNS, Web, and SQL servers to run a web application like WordPress. We will use a Linux server on a cloud provider like Digital Ocean and acquire a domain name, "projectlinux.store". We will install and configure BIND 9 as the authoritative DNS server. Then, we will install Apache as the web server and PHP as the scripting language for the backend. MySQL will be used as the SQL server to store data. These software components together form the LAMP stack. Finally, we will install WordPress and secure it, which is essential. WordPress is the most widely used web application, with more than a third of all websites running on it. By the end of this project, we will have a running WordPress website, and we will have learned how to set up and manage a LAMP stack.</p>
    
   <section id="Server">
     <h2>Running Server in Cloud</h2>
        
   <p>In this section we will set up a Linux server using a virtual private server (VPS) on Digital Ocean. We will create a droplet (Digital Ocean's term for a VPS), choosing an operating system and data center region, setting up SSH authentication, and connecting to the server using an SSH client.</p>
     
  <b>Running A Linux Server In Cloud </b>
     
     
  ![Screenshot 2023-04-26 105241](https://user-images.githubusercontent.com/86381942/234662041-8cf4d9e8-bc40-4c92-9faf-5100b5c26a72.png)

  <b>Connect through SSH and update and upgrade the server </b>
     
         apt update && apt full-upgrade -y
     
   ![Screenshot 2023-04-26 110111](https://user-images.githubusercontent.com/86381942/234663526-2809aeca-6154-4622-9eab-21e02d74b791.png)

 
 <section id="dns">
     <h2>Getting a Domain Name</h2>
     
 <p>Now We purchase a domain and also change DNS server to Personal Dns Server and adding host and ip address with hostname ns1.linuxpro.store and provide public ip address of our Ubuntu server Note that it is recommended to have a second server to serve as the secondary DNS server for that domain.The secondary DNS server is authoritative for the domain as well and is also known as the slave DNS server, the first one being the primary or the master server.For this lab, I don't have a second DNS server, although in a production environment, it is highly recommended and because namecheap requires two authoritative DNS servers for the domain I'll add the second host with the same IP address .</p>
     
  **PERSONAL DNS SERVER**
   
  ![screenshot-ap www namecheap com-2023 04 26-20_05_11](https://user-images.githubusercontent.com/86381942/234749410-ec4d965d-0dd2-4b17-bc89-9a66ea09459f.png)
   
   
![Screenshot 2023-04-26 205410](https://user-images.githubusercontent.com/86381942/234755387-69d736ae-adca-4145-ad60-2711c0503d70.png)
   
   <p><b>I'll use domain name servers Custom DNS and designate these as the authoritative servers for my domain. I'll also add the two name servers we created in the earlier step: ns1.linuxpro.store  as well as ns2.linuxpro.store.If you purchase a second name and want it to be hosted on the same reliable server, Simply add these name servers to the domain's custom DNS servers on the DNS server.</b></p>  
   
![Screenshot 2023-04-26 205831](https://user-images.githubusercontent.com/86381942/234756108-e9fe8e57-01cc-46cb-b273-c00396c94fb0.png)

<p><b>Now, any DNA Querry for the linuxpro.store  domain will reach the name server called ns1linuxpro.store or ns2.linuxpro.store , which has the IP address we have provided in the advanced DNS page. That's the IP address of the Ubuntu Droplet.</b></p>     
 
  <section id="bind">
     <h2>Installing a DNS Server (BIND9)</h2>
    
 <p>Now we create DNS server using the Bind version 9 software on Ubuntu. Bind, or Berkely Internet Name Domain, is an open source, flexible and full feature to DNS software that is widely used due to its stability and high quality.
Let's get started by installing the Bind9 DNS server on the Ubuntu VPS that runs in the digital ocean</p>
    
    
       'apt update && apt install bind9 bind9utils bind9-doc'    
    
    
To Check BIND 9 status run the below command
        
              systemctl status bind9
    
    
 ![Screenshot 2023-04-26 212751](https://user-images.githubusercontent.com/86381942/234759711-6f1f0610-4fe1-4601-9392-75099ffd03d1.png)

 **After installing BIND, let's set it to IPv 4 mode, since we are using only IPv4**
    
                    vim/etc/default/named
 
**I'm adding -4 to the end of the OPTIONS Parameters**
    
![Screenshot 2023-04-26 213525](https://user-images.githubusercontent.com/86381942/234762406-fcc20dc6-35b9-4f1d-ae17-5e452272e240.png)
    
 **Now Restart the the bind 9 service**
    
                   systemctl reload-or-restart bind9
    
**To check the process Running on named**

                   ps -ef | grep named 
    
 
  ![Screenshot 2023-04-26 213833](https://user-images.githubusercontent.com/86381942/234763122-2df95b75-8606-4d9c-b22d-a65d1ee81c94.png)
  
    
 **Main Configuration File of bind9 server**    
     
                   cat /etc/bind/named.conf
    
![Screenshot 2023-04-26 214040](https://user-images.githubusercontent.com/86381942/234763183-fb08c844-b96d-49d3-afb3-cce6ba7945b2.png)

    
 <h4 align="center"> <a href="https://medium.com/@Beepin/the-importance-of-dns-understanding-how-it-works-and-why-it-matters-a59f96cfabe0">More Information About DNS Server Check out this Blog Post </a> </h4>
    
Adding 2 forwarder to our DNS server. In the OPTION file I'll set the public DNS servers of Google as forwarder for my server. 
    
                           vim/etc/bind/ named.conf.options
    
 ![Screenshot 2023-04-26 214713](https://user-images.githubusercontent.com/86381942/234764364-45cc99ef-c555-4534-a698-86f6d5955b4c.png)
    
 **Now Restart the the bind 9 service**
    
                   systemctl reload-or-restart bind9
    
  **Lets check our DNS Server**
    
                   dig @localhost -t a parrotlinux.org
 
    
![Screenshot 2023-04-26 214944](https://user-images.githubusercontent.com/86381942/234764808-73e48ac2-bf34-4653-9805-78aed326b8b5.png)


 <section id="auth">
     <h2>Setting Up the Authoritative BIND9 DNS Serve(BIND9)</h2>
     
 Let's get started with configuring the DNS server we've just installed as being authoritative for the linuxpro.store domain
 
This is the master DNS server, which holds the master copy of the zone file; the zone file is the file that contains the information about the domain, such as subdomains, IP addresses and so on.This information is called DNS Records, and any of the changes are made only on this server. To add a new domain for which the server is authoritative I'm editing the following file with following config.

                 vim  /etc/bind/named.conf.local
                 
![Screenshot 2023-04-30 192757](https://user-images.githubusercontent.com/86381942/235406507-631958c4-e78e-482b-9a6b-3f4b46e9b88b.png)
   
 **In this config i created a new zone and specify the master zone.**  
           
**The next step is to create the zone file. Instead of creating the zone file from scratch, I'll use a zone template file which exists in /etc/bind.**
   
                    cp /etc/bind/db.empty /etc/bind/db.linuxpro.store
   
 ![Screenshot 2023-04-30 193014](https://user-images.githubusercontent.com/86381942/235408661-28ddbbea-3c93-4af8-8b38-a10a4be1bcc6.png)
   
   
![Screenshot 2023-04-30 193333](https://user-images.githubusercontent.com/86381942/235408715-fda92c8e-6fba-4168-a671-180c9b9fe816.png)
   
**Let's take a look at its contents,** 
The zone file contains three types of entries: comments which start with a semicolon, directives which start with a dollar sign, and resource records, eg DNS records. There is only one directive in the file,
The TTL directive defines in seconds the default time to leave value for the zone. This is the time a DNS record can be cached on other DNS servers. Or when a nonauthoritative DNS server is asking these authoritative DNS servers for a record, the server will send it to the record and tell the other server to store it in its cache for this amount of seconds.

**Let's talk about DNS records.**
The first DNS record in the file is called the SOA or the Start of Authority record. The second DNS record is called the NS or the name server record. It indicates the authoritative DNS servers, master, and slaves for this domain. There could be more NS records in a zone file. These two records SOA and NS are mandatory for each domain.  

**Now we are going to configure DNS record as below**
   
 ![Screenshot 2023-04-30 211226](https://user-images.githubusercontent.com/86381942/235409395-e7b32581-21c1-4bc3-ad09-10d1ac7edbfa.png)

**NOTE @=yourdomainname  IN = Internet (DNS Class) and we add different Sub Domains like ns1 have server IP if you have ns2 specify IP and mail for mail server and external server and public DNS and Save the File**
   
Before restarting the server, Check if there are any syntax errors in the configuration files.
        
                        named-checkconf
                        named-checkzone linuxpro.store /etc/blind/db.crystalmind.academy
   
![Screenshot 2023-04-30 211253](https://user-images.githubusercontent.com/86381942/235409894-b8cadaa1-a149-48b6-b908-9b3f37dcae97.png)
   
![Screenshot 2023-04-30 211304](https://user-images.githubusercontent.com/86381942/235409920-7add7830-7d8d-4641-92a4-1e6282567018.png)

![Screenshot 2023-04-30 211515](https://user-images.githubusercontent.com/86381942/235409945-bdd7e0fd-3872-4843-8420-01a8a728083f.png)
   
Now Test Authoritative BIND9 DNS Server

                       dig@localhost -t ns linuxpro.store
                       dig@localhost -t a www.linuxpro.store

![Screenshot 2023-04-30 211610](https://user-images.githubusercontent.com/86381942/235410116-d7e37d21-ec38-48bf-9699-a17950da96f1.png)

Also check the DNS server from others client Devices.
 
![Screenshot 2023-04-30 212022](https://user-images.githubusercontent.com/86381942/235410419-7a951428-7d2b-4a20-8eb5-2c7883cb8bcd.png)

 <section id="ap2">
    <h2>Installing a Web Server (Apache2)</h2>

To run Web applications, we need to install a Web server.In this project we use Apache for Web Server.Apache is available within Ubuntu’s default repositories so we can install it by simply running this command :

                       apt update && apt install apache2
                        
                      systemctl status apache2
                      
                      systemctl is-enabled apache2 [if it's enabled to start at boot time ]
    
**The next step is to adjust the firewall so that the incoming connections to the Web server are allowed.This is necessary only if there's a firewall enabled. By default, on Ubuntu ufw,is inactive** 
    
                        ufw status
                        
         
 **IF it is active then run this command**
   
                       ufw allow 'Apache Full'
                       
Now you can access your web server from IP or domain name .

![Screenshot 2023-04-30 231005](https://user-images.githubusercontent.com/86381942/235415014-f00c5523-2210-4d1d-98fd-7f9403351a9e.png)

 <section id="up">
    <h2>Setting Up Virtual Hosting (Apache2)</h2>

   
If you want to host more than one domain on an Apache Web server that has a single IP address, then you have to configure something “virtual hosting”. By default Apache on Ubuntu serves files to the clients from a directory called DocumentRoot that is by default in /var/www/html.This is document root.
   
   ◍ **Creating a new directory for the first website**
                             
                                  mkdir /var/www/linuxpro.store
  
   ![Screenshot 2023-05-15 104338](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/da8e8831-94ec-4aae-8b43-f0a4003521df)


   ◍ **Let's find out the user under which Apache runs**

                                  ps -ef | grep apache2 
   
   ![Screenshot 2023-05-15 104854](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/ab0966d2-f884-42de-a047-acbe6451a2ba)

   ◍ **Set the ownership of this directory**
                                 
                              chown -R www-data.www-data /var/www/linuxpro.store
   
   ◍ **Set the Permissions of this directory**                            
                               
                              chmod 755 /var/www/linuxpro.store
                              
   ◍ **I have created a html index files for the website linuxpro.store**  
   
                             vim /var/www/linuxpro.store/index.html
  
   
   ![Screenshot 2023-05-15 104917](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/bc55793d-5246-4e89-9c52-259e33f3fa2d)

   ◍ **Creating a new virtual host file for Apache 2**  
   
                            cd /etc/apache2
   
                            vim sites-available/linuxpro.store.conf
   
  ◍ **In the conf file i created below content for the host file** 
   
    
![Screenshot 2023-05-15 104642](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/b2f0db0a-2b37-490d-9618-8472526c819c)

   
                    
   ![Screenshot 2023-05-15 104942](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/bda7caa6-bd65-4e5a-8b57-e41abda90b48)

 ◍ **Enable Virtual host**
   
                          a2ensite linuxpro.store
                     
                         systemctl reload apache2
   
   ![Screenshot 2023-05-15 105443](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/d184a85d-b61e-4df9-ba3d-0b9403a0e6c1)

   
    ◍ **Now lets check our Website**

   
   ![Screenshot 2023-05-15 105611](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/6d812002-92a3-4d9e-a80b-4ff14e957c1e)

     
   <section id="ssl">
    <h2>Securing Apache with OpenSSL and Digital Certificates(Apache2)</h2>
     
  ![screenshot-www javatpoint com-2023 06 04-21_39_31](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/3f05d6a1-6d9c-46b5-86fd-c1457094e4a1)

This set up consists of two parts: getting a digital certificate from a certificate authority for the server and configuring the Apache web server to use the certificate and encrypt the connections
to the clients using OpenSSL and other cryptographic protocols.In this projecet we'll use Let's Encrypt as the certificate authority because it offers a free certificate for your website.     
It also simplifies the process by providing a software client called the Certbot that attempts to automate the most of the required steps. Currently, the entire process of obtaining and installing a certificate is fully automated on both.

Apache and Nginx.     
 ◍ **Installing Certbot and the Python plugin**

                `apt update && apt install certbot python3-certbot --apach`  /etc/apache2]
     
 ◍ **request the Certificate Authority and issues a certificate to domain**
                 
                 certbot -d linuxpro.store
     
                 webmaster@linuxpro.store
       
                 A
     
                 y
      
                 2
     
![Screenshot 2023-05-17 104822](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/670a097d-b1a2-414a-842a-6d5aa9196812)

![Screenshot 2023-05-17 104913](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/a5ba0cde-a882-4023-bd25-0787768c7a22)
     
 
 ◍ **Successfully deploy our certificate**  
![Untitled](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/48f9d940-33bd-4005-8169-01446c8974b9)
     
![Screenshot 2023-06-04 220222](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/fd6683cf-41a0-41ae-b44d-1365a5e197d7)
     
 <section id="access">
    <h2>Access Control by Source IP Address</h2>
   
◍The Apache Web server allows us to control access to our website based on the visitors' source IP address. We can control the access to directories, files, or URLs. This is common and useful because most web applications, such as blogs, online stores, and content management systems, have an administration section that can only be accessed from specific IP addresses, such as the IP address of the administrator.

Let's see an example of how to allow access to a specific directory of the website only to visitors that are coming from some specific source IP addresses. This is a common practice because it improves security. By default, access is allowed to all clients. First, we'll create the directory that stores the protected contents.

Here are the steps involved in creating a directory and restricting access to it based on IP address:

◍ **Create the directory.**
   
                       mkdir /var/www/linuxpro.store/admin

![Screenshot 2023-06-04 221148](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/a00d25c1-e256-4aac-82bf-1bbeedb3a1ff)
   
![Screenshot 2023-06-04 221131](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/e5a420f9-a9b3-41d9-abf5-0458494c3bba)

 ◍ **open the Apache configuration file.**
 ◍ **Add the following lines to the configuration file:**
   
 <Directory /path/to/directory>
Order allow,deny
Allow from 192.168.1.0/24
Deny from all
</Directory>

![Screenshot 2023-06-04 223035](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/9e0bffeb-1864-44d2-b5c7-d4658acaa522)

   
 ◍**Restarting the Apache web server**    
   
                        systemctl restart apache2
   
◍**Accessing the webpage again you got 403 error**.
 
 ![Screenshot 2023-06-04 223103](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/533a9cb0-b189-4694-8fcf-5c4b34ab60df)

 <section id="file">
    <h2>The <Files> Directive</h2>

 Let's consider an example of using the "Files" directive within a virtual host configuration. In the same virtual host, we are adding a "Files" section, where the directives within this section will be applied to any object with a name matching the specified file name.

For instance, let's say we want to restrict access to a file called "report.txt" to only clients from a specific IP address. We can add the following directives:    
      
                                  <Files "report.txt">
                                   Require all denied
                                 Require ip <specific_IP_address>
                                                  </Files>

**Please note that these changes will not take effect until the web server is restarted. Currently, anyone can access the "report.txt" file because the server has not been restarted yet.**

Once the server is restarted, attempting to access the file will result in a "403 Forbidden" error message since the IP address is not whitelisted. To allow access, we can add the IP address to the list of allowed ones using the following directive:   
                                      Require ip <specific_IP_address>

                                    
**To find your public IP address, you can use the command curl ident.me in the terminal.**

After saving the file and restarting the Apache web server, accessing the page again should grant access to the file for the specified IP address.
It's worth mentioning that the "Files" section can be nested inside "Directory" sections to restrict the portion of the filesystem they apply to. This allows for more granular access control.
In addition to specifying exact filenames, wildcards and regular expressions can be used in the "Files" directive. For example, "*.pdf" will match all PDF files, while "?at.pdf" will match "cat.pdf," "bat.pdf," "hat.pdf," and so on. Remember to close the "Files" directive properly.Regular expressions can also be used with the addition of the tilde character (~). Here's an example:
Suppose we want to create an IP address blacklist, allowing anyone to view image files except visitors coming from specific IP addresses. We can use the following "Files" section:
                                    
                                    
                                    <Files ~ "\.(gif|jpe?g|png)$">
                                          Require all granted
                                  Require not ip <specific_IP_address>
                                                       </Files>

In this case, the regular expression will match any string ending with ".gif," ".jpeg," ".jpg," or ".png." The "Require all" directive is necessary here. Inside the "Require all" section, we specify that access is granted, and then we add another directive, "Require not ip," followed by the blacklisted IP address.After saving the file and restarting the server, attempting to access a GIF file, for example, will result in a "403 Forbidden" error if the IP address is blacklisted.To make changes, simply edit the virtual host file again, modify the IP address, save it, restart Apache, and reload the page. Multiple IP addresses can be added on the same line if desired.                                     
                                      
 <section id="ht">
    <h2>The .htaccess File</h2>                                    

**we learned how directives enclosed in a directory section apply to the named directory, its subdirectories, and the files within them. We created a whitelist of permitted IP addresses that can access the directory on the server. However, this approach requires root access to the main configuration file of the web server.**

An alternative method is to use a .htaccess file, which allows configuration changes on a per-directory basis. The .htaccess file is predefined in Apache, but you can customize its name using the AccessFileName directive, although this is not commonly done.

Let's explore how it works:

◍ A file named .htaccess containing one or more configuration directives is placed in a specific directory.
   
◍ The directives in the .htaccess file are applied to that directory, its subdirectories, and the files within them.
   
◍ The use of .htaccess is determined by the AllowOverride directive, which specifies the categories of directives that will be honored if found in a .htaccess file.
   
◍ If you want to disable .htaccess in a directory, set AllowOverride to "none" for that directory.
   
◍ For example, to enable the use of .htaccess for the entire site, add the following directory section in the virtual host file:
   
                                 <Directory /var/www/crystalmind.academy>
                                            AllowOverride all
                                              </Directory>

**Note that the AllowOverride option is allowed only in a directory section. After saving the file, restart the server for the new settings to take effect.
Next, create a directory for your website, along with a .htaccess file. For this example, let's allow access to files with the ".conf" extension for a specific IP address. Modify the .htaccess file as follows:**  
   
                                       <Files "*.conf">
                                        Require all denied
                                    Require ip <specific_IP_address>
                                                </Files>

 Save the file and create a few files in that directory. Accessing the website directory via a browser will now only show the files that match the specified criteria since other IP addresses are denied access.
To make changes, open the .htaccess file and modify the allowed IP address. Save the file and reload the web page to see the updated access restrictions. It's important to note that there is no need to restart the web server for changes in the .htaccess file to take effect. Additionally, .htaccess files should be used when content providers require configuration changes on a per-directory basis but do not have root access to the server system.It's generally recommended to avoid using .htaccess files if you have access to the Apache main server config file. Any directive that can be included in the .htaccess file is better placed in a directory block within the main config file for better performance.Remember, .htaccess files provide a flexible way to manage per-directory configuration but should be used judiciously.
                                         
![screenshot-www udemy com-2023 06 04-22_50_50](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/2b2cea97-a3d6-476c-a8a0-6e47466dadf2)

                                         
   
   </body>
</html>


