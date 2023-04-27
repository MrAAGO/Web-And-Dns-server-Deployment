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
 
 </body>
</html>


