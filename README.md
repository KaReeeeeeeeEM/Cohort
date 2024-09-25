# WEB HOSTING ON VIRTUAL SERVERS

This project is aimed at hosting websites on Virtual Servers (Apache Servers) in a Linux machine using Command Line. Below is a detailed guide to help you host your website.

## Table of Contents

- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [License](#license)

## Installation

Here we shall first learn on how to host your website locally in the Virtual Server using HTTPS on the default port 443.

1. Clone your repository
- The repository name in github has a signature of .git whereas the repository url is supposed to have a format of 'https://github.com/[your-github-username]/[your-repo-name].git'.
  If you do not have git installed, you have to install it first.
     ```bash
      sudo apt install git
     ```
   - Also, you need to have installed 'apache2', 'nginx' or any virtual server of your choice. For this tutorial, we shall focus on 'apache2'. So here is the command to install 'apache2':
     ```bash
      sudo apt install apache2
      sudo apt update
     ```
     
   - Next, you have to clone your repository, you use the command 'git clone' as shown below:
      ```bash
      git clone https://github.com/your-username/your-repo-name.git
      ```
1. You might perform additional steps depending on your project. Here are the additional steps that you may need to do.
      ```bash
      cd your-repo-name
      ```
2. Then Install the necessary dependancies:
   - If you are using npm:
      ```bash
      npm install
      ```
   - If you are using yarn:
       ```bash
      yarn install
       ```

3. Lastly, start your project:
   ```bash
   npm start #if you are using npm
   yarn start #if you are using yarn
   ```

## Configuration
 - This section covers the necessary configurations for your Virtual Server needed by your project. To confifure a virual host for your Virtual Server, you need to follow the following steps:

1. Enable SSL module
      - Enable the SSL module in Apache by using the following command:
        ```bash
           sudo a2enmod ssl #this is to enable ssl in your apache server
           sudo systemctl restart apache2 #you need to restart your apache server for your changes to be applied
        ```
2. Create a Self-Signed SSL Certificate using OpenSSL
      - To use HTTPS, you require an SSL Certificate, but since we are hosting locally on the server, we need to create our own Self-Signed SSL Certificate. Luckily, OpenSSL handles that in just a few            steps as shown below:
         ```bash
            sudo mkdir /etc/apache2/ssl # make a directory where you are going to save your self-signed certificate
            sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache-selfsigned.key -out /etc/apache2/ssl/apache-selfsigned.crt # generate and save the SSL key
         ```
         
3. Create a configuration file for your virtual host on port 443
   - Here one may prefer to use a text editor of his choice, but make sure you have installed your text editor if it does not come as a default with the Operating System of your Virtual Server. The            popular text editors are 'nano', 'vi', 'vim' etc. To install any of these, use the command 'sudo apt install [your-text-editor]' as shown below.
     ```bash
        sudo apt install vim #The command may change depending on the Linux distro of your Virtual Server
     ```

   - To create the configuration file for your Virtual Host, first navigate to the directory '/etc/apache2/sites-available' then create your configuration file e.g '[your-website-domain].conf'
     ```bash
        cd /etc/apache2/sites-available #to navigate to the sites-available specific directory
        sudo vi [your-website-domain].conf #to create the configuration file using vi, you can use nano in place of vi
     ```

     or

     ```bash
        sudo vi /etc/apache2/sites-available/[your-website-domain].conf #create your configuration file in one command
      ```
   - Lastly, add the following configuration to the new file that you have created:
     ```bash
        <VirtualHost *:443>
          ServerName localhost
      
          DocumentRoot /var/www/html/[your-website-domain]
      
          SSLEngine on
          SSLCertificateFile /etc/apache2/ssl/apache-selfsigned.crt
          SSLCertificateKeyFile /etc/apache2/ssl/apache-selfsigned.key
      
          <Directory /var/www/html/[your-website-domain]>
              Options Indexes FollowSymLinks
              AllowOverride All
              Require all granted
          </Directory>
      
          ErrorLog ${APACHE_LOG_DIR}/[your-website-domain]_error.log
          CustomLog ${APACHE_LOG_DIR}/[your-website-domain]_access.log combined
      </VirtualHost>
     ```

4. Enable the New Virtual Host.
   - There is only one command that is used to enable the new virtual host,type or copy the command below and paste in your terminal.
     ```bash
        sudo a2ensite [your-website-domain].conf
      ```

5. Restart Apache
   - Lastly, you need to restart Apache for the changes that you have made to be applied. Type or Copy the command below to restart the apache server.
     ```bash
        sudo systemctl restart apache2
     ```

## Usage
   - After applying the configurations above successfully, you can now access your website locally using HTTPS on your Virtual Server by either 'https://localhost' or 'https://[your-server-ip]'.
     ```bash
        https://localhost # if you are using your own device to host
        https://[your-server-ip] # to access the website using another device
     ```
## License
  - This repository uses an MIT Licence and therefore one is allowed to make a copy or use the content of this repository for his own purposes.

      
  
