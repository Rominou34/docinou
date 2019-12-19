# Sysadmin

## Setting up FTPS with ProFTPd

https://www.howtoforge.com/tutorial/install-proftpd-with-tls-on-ubuntu-16-04/

### 1. Install proftpd and openssl

```
sudo apt-get -y install proftpd openssl
```

Add these lines in /etc/proftpd/proftpd.conf for better security:

```
[...]
DefaultRoot ~
ServerIdent on "FTP Server ready."
[...]
```

If you want to use FTP clients like FileZilla, make sure the valid shell check is turned off in `/etc/proftpd/proftpd.conf`:

```
RequireValidShell no
```

### 2. Create the SSL certificate for TLS

Create a ssl folder in `/etc/proftpd`

```
mkdir /etc/proftpd/ssl
```

Then, generate your SSL certificate inside it

```
openssl req -new -x509 -days 365 -nodes -out /etc/proftpd/ssl/proftpd.cert.pem -keyout /etc/proftpd/ssl/proftpd.key.pem
```

Then enter the required fields

```
Country Name (2 letter code) [AU]: <-- Enter your Country Name (e.g., "DE").
State or Province Name (full name) [Some-State]:<-- Enter your State or Province Name.
Locality Name (eg, city) []:<-- Enter your City.
Organization Name (eg, company) [Internet Widgits Pty Ltd]:<-- Enter your Organization Name (e.g., the name of your company).
Organizational Unit Name (eg, section) []:<-- Enter your Organizational Unit Name (e.g. "IT Department").
Common Name (eg, YOUR name) []:<-- Enter the Fully Qualified Domain Name of the system (e.g. "server1.example.com").
Email Address []:<-- Enter your Email Address.
```

### 3. Enable TLS in ProFTPd

In order to enable TLS in ProFTPd, open the `/etc/proftpd/proftpd.conf` and uncomment this line:

```
[...]
#
# This is used for FTPS connections
#
Include /etc/proftpd/tls.conf
[...]
```

Then open `/etc/proftpd/tls.conf` and edit it like this:

```
<IfModule mod_tls.c>
TLSEngine                  on
TLSLog                     /var/log/proftpd/tls.log
TLSProtocol TLSv1.2
TLSCipherSuite AES128+EECDH:AES128+EDH
TLSOptions                 NoCertRequest AllowClientRenegotiations
# Your certificale file
TLSRSACertificateFile      /etc/proftpd/ssl/proftpd.cert.pem
# Your certificate key file
TLSRSACertificateKeyFile   /etc/proftpd/ssl/proftpd.key.pem
TLSVerifyClient            off
TLSRequired                on
RequireValidShell          no
</IfModule>
```

### 4. Add an FTP user

Create a new user and set its password

```
useradd --shell bin/false rominou
passwd rominou
```

This will add the user "tom" with the shell /bin/false. This shell ensures that he can login by FTP but not by SSH. The home directory of a user is /home/[USERNAME] by default, in our case /home/tom. ProFTPD is configured to jail the user to his home directory, so he can not access system files outside of /home/tom.

If you like to set a different home directory, use the command below:

```
usermod -d /var/www/website user_name
```

### 5. Final step

Uncomment the last line of `proftpd.conf`, it apparently makes proftpd to the 2222 port

```
#Include /etc/proftpd/conf.d/
```

See more here: https://serverfault.com/a/778755

## 5. Restart your FTP server

```
systemctl restart proftpd.service
```
