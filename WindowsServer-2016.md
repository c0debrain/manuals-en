# How to install Passwork on Windows Server 2016

**1. Set hostname, install IIS 10, open 5353 port.**

Press `Win+X`, then `Y` and change default hostname. 
Change server hostname to `passwork` to enable `passwork.local` domain in your private network.

![alt text](./images/WS2016_01.png)

To complete changes reboot the server.


Open “Server Manager” and add “Web Server (IIS)” role.

![alt text](./images/WS2016_02.png)

Allow inbound UDP connection on 5353 port in firewall settings.

![alt text](./images/WS2016_03.png)

![alt text](./images/WS2016_04.png)

![alt text](./images/WS2016_05.png)


**2. Install MongoDB database.**

Open link [https://www.mongodb.com/download-center](https://www.mongodb.com/download-center) choose “Community Server”, “Windows”, select “MSI” package and click on “Download”. Then click “Save”. MongoDB installation package will be downloaded. Click “Run” to start installation process.

Notice: Reduce the security level of IE before downloading. Enable "Active Scripting" and "Download" options.

![alt text](./images/WS2016_06-php7.png)

Click “Next”, then accept terms, again click “Next”. Select “Complete” installation.

![alt text](./images/WS2016_07-php7.png)

Uncheck the “Install MongoD as a Service” if prompted about it. Click “Next”.

![alt text](./images/WS2016_08_01-php7.png)

Tick off “Install MongoDB Compass”. Click “Next” then “Install” to start installation process.

![alt text](./images/WS2016_08_02-php7.png)

Click “Finish” after installation ends.


**Fix the Windows firewall.**

Go to your “Control Panel” and then click on “System and Security”. Once there, click on “Windows Firewall”.

You should now see your Windows firewall like this:

![alt text](./images/WS2016_09.png)

Click on “Allow an app or feature trough Windows Firewall”, your window will change. Click on “Allow another app.” -> click “Browse” and add MongoDB Database Server application "C:\Program Files\MongoDB\Server\4.0\bin\mongod.exe", then click "Add" and "Ok".

![alt text](./images/WS2016_10-php7.png)


**Configure a Windows Service for MongoDB Community Edition.**

Create directories for your database and log files:
Open Command Prompt. Press “Win+X”, then press “a” key. Run:

```
md \data\db
```


and 

```
md \data\log
```


commands.


**Create a configuration file.**

Create a file “C:\Program Files\MongoDB\Server\4.0\mongod.cfg” that specifies both systemLog.path and storage.dbPath:

```
systemLog:
    destination: file
    path: c:\data\log\mongod.log
storage:
    dbPath: c:\data\db
```


**Install the MongoDB service.**

Run all of the following commands in Command Prompt with “Administrative Privileges” (open command prompt. Press “Win+X”, then press “a” key).

Run:

```
"C:\Program Files\MongoDB\Server\4.0\bin\mongod.exe" --config "C:\Program Files\MongoDB\Server\4.0\mongod.cfg" --install
```

Notice: Please change the paths if the mongodb version different from 4.0.

**Start the MongoDB service.**

```
net start MongoDB
```


![alt text](./images/WS2016_11-php7.png)


**Verify that MongoDB has started successfully.**

Verify that MongoDB has started successfully by checking the log file at c:\data\log\mongod.log for the following line: [initandlisten] waiting for connections on port 27017


**3. PHP installation.**

We will use Web Platform Installer for PHP installation, so open IE and go to [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx) URL to install the extension.

![alt text](./images/WS2016_12.png)

Then click “Run”.

![alt text](./images/WS2016_13.png)

Accept terms and press “Install”. Press “Finish” after installation ends.


Open “Server Manager”, go to “Tools” and click on “Internet Information Services (IIS) Manager”, select server and double click on “Web Platform Installer”.

![alt text](./images/WS2016_14.png)

If prompted about "Web Platform Installer 5.1 Update Installation" accept the installation.

Go to “Products” tab and search for PHP. Select PHP 7.0.30 from the list and press “Add”. Then press “Install” and accept third party license terms.

![alt text](./images/WS2016_15-php7.png)

You will see PHP Manager for IIS failed to install, this notice can be ignored. Click “Finish”. Click “Finish”.


**4. Installing the MongoDB PHP Driver, Phalcon Framework and enabling additional extensions and options.**

Go to [https://windows.php.net/downloads/pecl/releases/mongodb/1.5.3/php_mongodb-1.5.3-7.0-nts-vc14-x86.zip](https://windows.php.net/downloads/pecl/releases/mongodb/1.5.3/php_mongodb-1.5.3-7.0-nts-vc14-x86.zip) to download MongoDB PHP Driver.

Go to [https://github.com/phalcon/cphalcon/releases/download/v3.4.1/phalcon_x86_vc14_php7.0_3.4.1-1751_nts.zip](https://github.com/phalcon/cphalcon/releases/download/v3.4.1/phalcon_x86_vc14_php7.0_3.4.1-1751_nts.zip) to download Phalcon Framework.

Extract the “php_mongodb-1.5.3-7.0-nts-vc14-x86.zip” archive and copy “php_mongodb.dll” to “C:\Program Files (x86)\PHP\v7.0\ext”.

Extract the “phalcon_x86_vc14_php7.0_3.4.1-1751_nts.zip” archive and copy “php_phalcon.dll” to “C:\Program Files (x86)\PHP\v7.0\ext”.

Open “C:\Program Files (x86)\PHP\v7.0\php.ini” in Notepad and add to the [Extension List] section the following lines:

```
extension=php_mongodb.dll
extension=php_phalcon.dll
extension=php_ldap.dll
```


Save modifications and close the Notepad.

Download and install [Visual C++ Redistributable Packages for Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=40784). Choose "vcredist_x86.exe" version.

Restart “IIS” to reload PHP extensions.

![alt text](./images/WS2016_15_01-php7.png)

**5. Download and install Passwork.**

Open repository URL in IE browser [https://passwork.download/](https://passwork.download/) Sign in using provided Login and Password.

If login failed add “https://passwork.download” to the trusted sites in IE settings (settings>security tab>trusted sites).

![alt text](./images/WS2016_16.png)

Choose “passwork” repository.

![alt text](./images/WS2016_17.png)

Switch to `V3` branch.

![alt text](./images/WS2016_18_01-v3.png)

Then click on download icon and select ZIP option.

![alt text](./images/WS2016_18_03-v3.png)

Extract archive and copy content of passwork folder to “C:\inetpub\wwwroot\” directory.

![alt text](./images/WS2016_19.png)


**Create config file.**

Create a copy of the “C:\inetpub\wwwroot\app\config\config.example.ini” in “C:\inetpub\wwwroot\app\config\” and rename it to “config.ini”.


**Set correct permissions.**

Open IIS Manager, then right click on website and click “Edit permissions”.

![alt text](./images/WS2016_20.png)

Press “Edit”, then “Add” and search for “IUSR” object. Select “IUSR” and grant “Write” permissions then click “Ok” twice.

![alt text](./images/WS2016_21.png)


**Change website physical path.**

Right click on “Default Web Site” > “Manage Website” > “Advanced Settings”. Change website physical path to “C:\inetpub\wwwroot\public\”, then click “Ok”.

![alt text](./images/WS2016_22.png)


**Restore MongoDB database.**

Open Command Prompt. Press “Win+X”, then press “a” key.

Run the following commands:

```
cd C:\Program Files\MongoDB\Server\4.0\bin
```
```
mongorestore --db pwbox C:\inetpub\wwwroot\dump\pwbox
```


The passwork database will be created.


**Rewrite rules**

Double click on “URL Rewrite” icon. Click “Import rules” and select .htaccess file from website root directory, then click “Open” and “Import” buttons.

![alt text](./images/WS2016_24.png)

Click “Apply” button once imported.


Open [http://passwork.local](http://passwork.local) to access website.

![alt text](./images/WS2016_25-php7.png)

**Sign up**

Fill in login and password for the first user. This user will become an owner and administrator.
Click Upload keys and select `.lic` and `reginfo.json` file.

**Legacy License installation**

If you have `reginfo.php` instead of `reginfo.json` then you can upload the keys manually.
Extract archive with registration keys and move `.lic` and `reginfo.json` (or `reginfo.php`) to "/var/www/app/keys/" directory.


If login failed add “http://passwork.local” to the trusted sites in IE settings (settings>security tab>trusted sites).


**6. Configure SSL certificate on IIS 10.**

Open “Server Manager”, go to “Tools” and click on “Internet Information Services (IIS) Manager”, select server and double click on “Server Certificates”, then import certificate.

![alt text](./images/WS2016_26.png)

Go to sites, select your site and click “Bindings”. Select “https” protocol from drop-down list. Enter hostname “passwork.local”. Select SSL certificate from the list (in our case cert name is “https”).

![alt text](./images/WS2016_27.png)

Then click “Close”.

Check SSL connection by going to [https://passwork.local](https://passwork.local).


**7. SMTP server install.**

Open “Server Manager” and add “SMTP Server” role.

![alt text](./images/WS2016_28.png)

Open IIS 6.0 Manager and configure SMTP server.

![alt text](./images/WS2016_29.png)

Open “Delivery” tab click on “Advanced” button and set “Smart host”. For Gmail it is “smtp.gmail.com”.

![alt text](./images/WS2016_30.png)

Click on “Outbound connections” and set outbound TCP port to “587”.

![alt text](./images/WS2016_31.png)

Click on “Outbound Security” button, select “Basic Authentication” and fill in user name and password fields. Tick on “TLS Encryption”.

![alt text](./images/WS2016_32.png)

Click on “Access” tab > “Connection” and add 127.0.0.1 ip to granted computers list.

![alt text](./images/WS2016_33.png)

Click on “Access” tab > “Relay” and add 127.0.0.1 ip to granted computers list.

![alt text](./images/WS2016_34.png)


**Enable SMTP Server start on boot.**

Open Command Prompt - Press “Win+X”, then press “a” key. Run following commands:

```
powershell
```
```
set-service smtpsvc -StartupType Automatic
```


SMTP configuration completed.
