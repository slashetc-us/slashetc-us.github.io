---
layout: post
title: HTTPS EVERYWHERE!!!
date: 2015-04-06 19:47:22.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Apache
- Digital Ocean
- Hosting
- Linux
- Servers
- Ubuntu
tags: []
meta:
  _edit_last: '1'
  _responsive_layout: default
  _syntaxhighlighter_encoded: '1'
  _wpas_done_all: '1'
---
<p>If you haven't noticed over the last 24 hours, SSL has been enabled and enforced throughout the entire site. So I decided to do a little write up on how I went about creating and installing the certificate. What I did first is sign up for a free SSL certificate from StartCOM using their service StartSSL. This is a free Class 1 cert that is trusted with all major browsers, so long as it is installed properly.</p>
<p>To get started with a StartSSL certificate go to the website and sign up and get your account activated. There are different account levels – the free account level (level 1) does not let you generate wildcard certificates, there is a small fee though if you need to do that and you can generate unlimited wildcards. Once your account has been activated you then need to validate your domain, do this from the “Validations Wizard” inside the StartSSL control panel (make sure you select domain validation). Once the domain has been validated you can then continue to the next step.</p>
<p><!--more--></p>
<p>Generate a CSR and key file (this is optional, but I like to do this myself). The following command will generate two files: <code>csr.csr</code> and <code>key.key</code>. You do not need to fill out anything when you are prompted, just keep hitting enter. The CSR data will not be used anyway.</p>

    root@server:/tmp# openssl req -out csr.csr -new -newkey rsa:4096 -nodes -keyout key.key
    Generating a 4096 bit RSA private key
    ...................................................................................++
    ......................................................++
    writing new private key to 'key.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [US]:
    State or Province Name (full name) [Some-State]:
    Locality Name (eg, city) []:
    Organization Name (eg, company) [Internet Widgits Pty Ltd]:
    Organizational Unit Name (eg, section) []:
    Common Name (eg, YOUR name) []:
    Email Address []:
    Please enter the following 'extra' attributes
    to be sent with your certificate request
    A challenge password []:
    An optional company name []:
    root@server:/tmp#

<p>Select the “Certificates Wizard” option from the StartSSL control panel. Select “Web Server SSL” from the “Certificate Target” drop down and click continue. Skip the “Generate Private Key” step – we have done this above. Paste the contents of the “csr.csr” file into the CSR text box and continue. The CSR can be removed, it is no longer needed.</p>
<p>Your SSL certificate will then either be issued instantly or validated. The validation usually takes about 10-15 minutes. Once the certificate has been issued it is time to install it.</p>
<p>Get a copy of the StartSSL CA to avoid errors when clients connect – depending on your account level there is a different CA. In the StartSSL tool box select “StartCom CA Certificates” and download the appropriate one (free accounts are the “Class 1 Intermediate Server CA”). I will stick the certificates in <code>/etc/apache2/ssl</code>/.</p>
<p>Move the <code>/tmp/key.key</code> file to <code>/etc/apache2/ssl/mysslsite.com.key</code>. Select “Retrieve Certificate” from the StartSSL tool box and place that certificate into the file <code>/etc/apache2/ssl/mysslsite.com.crt</code>. Make sure you fix up the permissions so they are not wide open on the private key.</p>
<p>You can then add the SSL vhost to Apache as usual, as an example:</p>

    <VirtualHost 123.123.123.123:443>
      ServerAdmin admin@me.com
      ServerName mysslsite.com
      ServerAlias www.mysslsite.com
      DocumentRoot /home/site/public_html/
      <Directory /home/site/public_html/>
         Options -Indexes SymLinksIfOwnerMatch
         AllowOverride All
         Order allow,deny
         allow from all
      </Directory>
      # Include /etc/apache2/includes/mysslsite.com
      LogLevel warn
      ErrorLog /var/log/apache2/mysslsite.com-error.log
      CustomLog /var/log/apache2/mysslsite.com-access.log combined
      SSLEngine on
      SSLCertificateFile /etc/apache2/ssl/mysslsite.com.crt
      SSLCertificateKeyFile /etc/apache2/ssl/mysslsite.com.key
      SSLCertificateChainFile /etc/apache2/ssl/startssl_class1_intermediate_ca.crt
    </VirtualHost>

<p>If you leave out the SSLCertificateChainFile part from the configuration you will most likely get an error about the SSL certificate not being trusted. This is because the CA certificate is needed to establish the chain of trust so put that in the configuration.</p>
<p>And that's about it. Once the server is restarted you should be able to access your side via HTTPS. If you are on Firefox and receive an OSCP error, <a href="https://forum.startcom.org/viewtopic.php?f=15&amp;t=2654&amp;sid=f4fba7cb1be8a65056d0ce27a8394459" target="_blank">see the startCom forum topic on this</a>, but it will usually resolve itself within 24 hours.</p>
<p>To force your site into SSL open your ${WWWROOT}/.htaccess file and add the following after "RewriteEngine On" and will look something like this:</p>

    <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{SERVER_PORT} 80
    Rewriterule ^(.*)$ https://www.mysslsite.com/$1 [R,L]
    RewriteBase /
    RewriteRule ^index\.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.php [L]
    </IfModule>

<p>Now try to access your site via normal http://www.mysslsite.com you will now see that it is forced over to HTTPS/SSL.... nice...</p>
