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
        <li><a href="#http">HTTP Digest Authentication</a></li>
        <li><a href="#https">HTTP Compression</a></li>
        <li><a href="#set">SetHandler and Server Status</a></li>
        <li><a href="#php">Installing PHP</a></li>
        
    
        
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

<section id="http">
    <h2>HTTP Digest Authentication</h2>
  
**HTTP authentication is a method used to secure specific parts of a website by requiring additional credentials such as usernames and passwords. It is commonly employed to restrict access to admin panels, backend services, and other sensitive resources. When used in conjunction with IP-based restrictions and HTTPS, it can provide robust security for web-based resources.**

Let's explore how to utilize HTTP authentication to protect a directory. There are two types of HTTP authentication: Basic and Digest. In this example, we will focus on digest authentication, which is considered more secure and recommended over basic authentication.
  
◍**To begin, we need to generate a password file using the htdigest utility provided by the apache2-utils package. Ensure the package is installed by running apt install apache2-utils. Then, use the following command to generate the password file:**  
  
                                          htdigest -c /etc/apache2/.htpasswd myserver Johnny

**In this command, the -c option creates the password file if it doesn't exist or overwrites it if it does. The path specified after -c is the location of the password file, and myserver represents the realm domain that should match the AuthName directive in the Apache config file. Finally, Johnny is the username for authentication.**

◍ Next, we need to configure the protected directory in Apache. You can add the required directives either in a directory section within the virtual host config file or in a .htaccess file located in the target directory. However, it is recommended to avoid using .htaccess when you have access to the main Apache config file.

Open the virtual host config file of your website and add a new Directory section for the target directory, for example:

                                               <Directory /var/www/linuxpro.store/protected>
                                                                 AuthType Digest
                                                              AuthName "myserver"
                                                            AuthDigestProvider file
                                                       AuthUserFile /etc/apache2/.htpasswd
                                                               Require valid-user
                                                                 </Directory>

In this example, we specify the directory path, set the AuthType to Digest, provide the AuthName that matches the realm domain from the htdigest command, specify the AuthDigestProvider as file, and indicate the path to the password file using AuthUserFile. Finally, the Require valid-user directive ensures that only authenticated users can access the directory.

**Save the file and exit the editor. Then, enable the auth_digest module by running**
               
                                       a2enmod auth_digest

**Restart the web server for the changes to take effect:
  
                                     systemctl restart apache2

![Screenshot 2023-06-04 225916](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/c49bee3d-1797-446e-88ac-11767ed371e6)
 
To test the setup, create a directory called protected within the website's DocumentRoot and place an index.html file inside. Access the directory using a browser, and you will be prompted to enter credentials. Provide the correct username and password, and you will be granted access to the directory's contents.

**Keep in mind that you can combine HTTP authentication with host or IP-based authorization for enhanced security.**                                      
                                                 
                                                   mkdir /var/www/linuxpro.store/protected

                           echo "This File is Protected by HTTP Digest Auth" > /var/www/linuxpro.store/protected/index.html

  ![Screenshot 2023-06-04 230650](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/6cff41d2-b97b-4dea-b128-2d03d50822f7)

  ![Screenshot 2023-06-04 230708](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/fe1890ad-8134-4191-b285-b73590fae94a)

   
     
 <section id="https">
    <h2>HTTP Compression</h2>
   
 The recommended Apache module for data compression is called mod_deflate. This module allows the web server to compress the output before delivering it to the client, resulting in faster loading times for web pages. Compressing the contents of a web page also reduces bandwidth usage, which can positively impact the site's performance and page rank in search engine evaluations.

By using mod_deflate, text files such as HTML, TXT, XML, JavaScript, JSON, and others can be compressed to approximately 20 to 40% of their original sizes. However, it is important for the web server to ensure that the client receiving the compressed data can understand and render it correctly.

To enable mod_deflate, use the command 
                                                      a2enmod deflate. 
   
However, in many cases, mod_deflate may already be enabled. The next step is to configure the module to specify which files should be compressed and customize other options.

Open the configuration file for the deflate module 
   
                                            vim /etc/apache2/mods-enabled/deflate.conf. 
   
 The existing options in the file specify the file types that will be delivered in a compressed format. You can add more AddOutputFilterByType directives to include additional file types.

Note that it is redundant to compress image and multimedia files like JPEG, PNG, GIF, or MKV, as they are already compressed. Additionally, files such as PDF, Word, or Excel documents are not suitable candidates for compression.

The official Apache documentation provides examples of enabling compression for all document types except for GIF, PNG, and JPEG files using AddOutputFilterByType directives.

Another directive called DeflateCompressionLevel allows you to specify the compression level. A higher value results in better compression but requires more CPU resources. The value can range from 1 (least compression) to 9 (most compression), with a default value of 6. For example, you can set the default compression level to 7 using DeflateCompressionLevel 7.

To test the newly configured module, open the virtual host file of your site and add directives that log useful information about compression, such as the compression ratio. Define a log format and specify the log file where the compression information will be stored. Remember to comment out any other custom log directives.

Save the virtual host file and restart the web server for the changes to take effect.

To verify that compression is working, you can download a large text file or any file of your choice to the DocumentRoot directory. Access the file using a browser, and then check the compression ratio in the log file specified earlier. The compression ratio indicates how effectively the file was compressed.

It's worth noting that there is another module called mod_gzip that can be used for compression. However, mod_deflate is generally considered better and recommended for compression purposes. 
  
 ![Screenshot 2023-06-04 231504](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/08bb58a3-de2e-48ad-b4dc-44dd60a1f85e)

 <section id="set">
    <h2>SetHandler and Server Status</h2>
   
 ◍**Monitoring the web server is essential for gathering information about events and server performance. While server logs provide historical data, they often lack real-time information about the server's current state, including client requests and resource consumption. To address this, Apache Web server offers the status module, which exposes server metrics in an easily readable HTML format.**

To enable the mod_status module, use the command a2enmod status. If it's already enabled, you can proceed to configure it.

Open the configuration file for the mod_status module, typically located at 
                                                 
                                                   /etc/apache2/mods-available/status.conf.
   
                    
 In this file, you'll find various directives to configure the module.

One important directive is the <Location> directive, which limits the scope of enclosed directives based on the URL. For example, if the URL contains /server-status, the directives inside that section will be applied. Note that the server-status location operates independently of the filesystem, so there's no need for an actual directory or file named server-status. Within this section, the SetHandler server-status directive enables an internal and predefined action in Apache. It's crucial to restrict access to the server-status resource to prevent unauthorized access and protect sensitive server and system information.

To restrict access, you can use the Require directive with the ip parameter followed by the IP address you want to whitelist. For example, Require ip <your_public_ip_address>. Add this directive within the <Location> section to limit access to the server status page.

Save the configuration file and restart the web server for the changes to take effect.

To access the server statistics, open a web browser and enter the URL 
   
                                                http://<your_domain_or_server_name>/server-status. 
   
 This will display useful information about the server and its connections. If there are no concurrent connections, you will only see the client that requested the server status.

To simulate concurrent connections for benchmarking purposes, you can use a tool called ab (ApacheBench). Run the command
   
                                                 ab -n 100 -c 10 https://localhost/
   
   , which simulates 100 requests with a maximum of 10 requests running concurrently. Refresh the server status page in your browser, and you will observe information about the clients and requested resources.

Additionally, if your browser supports automatic refresh, you can add ?refresh=n to the server status URL, where n represents the number of seconds between each refresh. For example, 
    
                                                               http://<your_domain_or_server_name>/server-status?refresh=2 
   
 will automatically refresh the page every two seconds.

By following these steps, you can effectively monitor your server's performance using mod_status and gather real-time information about its connections and resource usage.
  
   
 <section id="php">
    <h2>Installing PHP</h2>   
   
◍ **To install PHP, one of the most widely used server-side programming languages, follow these steps:**

Update the package lists and install the necessary PHP packages by running the command: 
   
                                        apt update && apt install php php-mysql libapache2-mod-php.


 Once the packages are installed, restart Apache to load the PHP module: 
   
                                                      systemctl restart apache2.

To verify the PHP installation and check the installed version, run: **php -v**. You should see the PHP version displayed (e.g., PHP 7.4.3).

To test if Apache and PHP are configured correctly to handle dynamic content, follow these steps:

Create a PHP file in the DocumentRoot directory of your virtual host. For example: 
   
                                                    vim /var/www/linuxpro.store/test.php.

Add some PHP code to the file. Note: I'm not providing specific PHP code examples here, but you can write PHP code according to your needs.
   
                                                        <?php
                                                       phpinfo();
                                                            ?>


Save the file and access it in your browser by entering the URL http://your_domain/test.php.

If the PHP code is executed correctly and you see the expected information on the web page, it means that PHP and Apache are working together and configured properly. However, if you encounter issues such as seeing the PHP code as plain text or the browser trying to download the PHP file, there may be a problem that needs to be troubleshooted and fixed.

   
![Screenshot 2023-06-04 234138](https://github.com/MrAAGO/Web-And-Dns-server-Deployment/assets/86381942/2009a75d-dfb8-48f4-ab77-862aac633a7a)

   
   </body>
</html>


