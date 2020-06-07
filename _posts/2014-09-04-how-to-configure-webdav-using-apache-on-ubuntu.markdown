---
layout: post
title: "How to configure WebDAV using Apache on Ubuntu"
date: 2014-09-04 23:18
comments: true
categories:
---

[WebDAV](https://en.wikipedia.org/wiki/WebDAV) is an extension of the HTTP protocol that allows users to manage files on servers. There are many ways to manage files on a remote server. WebDAV has several benefits over other solutions such as [FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol) or [Samba](https://en.wikipedia.org/wiki/Samba_(software)). In this article, we will go through how to configure your Apache server on Ubuntu 14.04 to allow native WebDAV access from Windows, Mac and Linux with authentication.

<!-- more -->

## Why WebDAV?

WebDAV offers several advantages.

 - Native integration on all major OSes(Windows, Mac, Linux). There is no need to install third party software to use WebDAV.

 - Supports partial transfers.

 - More choices for authentication. Being on HTTP means [NTLM](https://en.wikipedia.org/wiki/NT_LAN_Manager), [Kerberos](https://en.wikipedia.org/wiki/Kerberos_(protocol)), [LDAP](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol), etc are possible.

Depending on your situation, WebDAV may be the best solution for your needs.

## Why Apache?

There are many web servers around that supports WebDAV on Linux. However, Apache has the most compliant implementation of the WebDAV protocol out there. At the time in writing, WebDAV on [Nginx](http://nginx.org/) and [Lighttpd](http://www.lighttpd.net/) work but only partially.

## Install Apache

Let us get [Apache](https://httpd.apache.org/) installed.

```
sudo apt-get update
sudo apt-get install apache2
```

The Apache web server should be installed and running.

## Setting up WebDAV

There are 3 steps to set up WebDAV. We designate a location, enable the necessary modules and configure it.

### Step 1: Preparing the directories

We need to designate a folder for serving WebDAV. We are creating a new directory in `/var/www` for that. You will also need to change the owner to `www-data`(or whichever your Apache user is) in order to allow Apache to write to it.

```
sudo mkdir /var/www/webdav
sudo chown www-data:www-data /var/www/
```

### Step 2: Enabling modules

Next we enable the modules using [a2enmod](http://man.he.net/man8/a2enmod)

```
sudo a2enmod dav
sudo a2enmod dav_fs
```

The Apache modules are found under `/etc/apache2/modules-available`. This creates a symbolic link from `/etc/apache2/modules-available` to `/etc/apache2/modules-enabled`.

### Configuration

We open the configuration file at `/etc/apache2/sites-available/000-default.conf` using your favorite text editor. Add the following configuration.

```
DavLockDB /var/www/DavLock

<VirtualHost *:80>
    Alias /webdav /var/www/webdav

    <Directory /var/www/webdav>
        DAV On
    </Directory>
</VirtualHost>
```

The [DavLockDB](https://httpd.apache.org/docs/2.4/mod/mod_dav_fs.html) directive designates the name of the DAV Lock database. It should be a path to a file. The file does not need to be created. The directory should be writeable by the Apache server.

The [Alias](https://httpd.apache.org/docs/2.4/mod/mod_alias.html) directive maps requests to `http://your.server/webdav` to the `/var/www/webdav` folder.

The [Directory](https://httpd.apache.org/docs/current/mod/core.html#directory) directive tells Apache to enable WebDAV for the `/var/www/webdav` folder. You can find out more about [mod_dav](https://httpd.apache.org/docs/2.4/mod/mod_dav.html) from the Apache docs.

If you restart the Apache server, you will have a working WebDAV server without authentication.

Restart the Apache server like this

```
sudo service apache2 restart
```

The WebDAV server should be found at __http://<your.server.com>/webdav__. Try logging in without any credentials.

## Adding authentication

A WebDAV server without authentication is not secure. In this section we'll add authentication to your WebDAV server. There are many authentication schemes available. We are only going to touch on the 2 simplest schemes: Basic and Digest authentication.

### Which to use? Basic or Digest authentication?

Take a look at this table which illustrates the compatibility of the various authentication schemes on different operating systems. Note that if you are serving HTTPS, we are assuming your ssl cert is valid(not self-signed).

![WebDAV compatibility](http://i.imgur.com/Q01EN3F.png)

If you are using __HTTP__, use [Digest authentication](https://en.wikipedia.org/wiki/Digest_access_authentication) as it will work on all operating systems. If you are using __HTTPS__, you have the option of using [Basic authentication](https://en.wikipedia.org/wiki/Basic_access_authentication).

We're going to cover the __Digest__ authentication version since it works on all the operating systems without the need for a SSL cert.

### Digest authentication

Let us generate the file(called __users.password__) that stores the password for the users. In Digest authentication, there is the __realm__ field which acts as a namespace for the users. We will use __webdav__ as our __realm__. Our first user will be called __alex__.

To generate the digest file, we have to install the dependencies.

```
sudo apt-get install apache2-utils
```

After that, we generate the file.

```
sudo htdigest -c /etc/apache2/users.password webdav alex
```

There should be a password prompt for the password of __alex__.

For subsequent addition of users, you should remove the __c__ flag. Another example adding a user called __bob__.

```
sudo htdigest /etc/apache2/users.password webdav bob
```

We also need to allow Apache to read it. So we change the owner of the file.

```
sudo chown www-data:www-data /etc/apache2/users.password
```

After the password file is created, we should make changes to the configuration at `/etc/apache2/sites-available/000-default.conf`.

```
DavLockDB /var/www/DavLock

<VirtualHost *:80>
    Alias /webdav /var/www/webdav

    <Directory /var/www/webdav>
        DAV On
        AuthType Digest
        AuthName "webdav"
        AuthUserFile /etc/apache2/users.password
	Require valid-user
    </Directory>
</VirtualHost>
```

The [mod_authn](https://httpd.apache.org/docs/current/mod/mod_authn_core.html) module contains the definitions for the authentication directives. In essence, we instruct Apache that for the `/var/www/webdav` directory, there should be authentication using the __Digest__ scheme. The realm should be called __webdav__. Find the password from the file at __/etc/apache2/users.password__. Only valid users who authenticate themselves is able to acess that directory.

Finally, enable the digest module and restart the server for the settings to take effect.

```
sudo a2enmod auth_digest
sudo service apache2 restart
```

## Testing it

We'll demonstrate how to access your WebDAV server from the native file browsers of Mac, Windows and Linux(Ubuntu).

### Mac

On Mac, open __Finder__. On the menu bar, find __Go__ and select the option __Connect to Server__.

![WebDAV Mac Step 1](http://i.imgur.com/q6rsU9q.png)

Enter the server address. It should be __http://<your.server>/webdav__. Press __Connect__.

![WebDAV Mac Step 2](http://i.imgur.com/h4mFZoK.png)

You will be prompted for a username and pssword. Enter them and press __Connect__.

![WebDAV Mac Step 3](http://i.imgur.com/BkhcR7I.png)

Once you have connected, the directory should appear in __Finder__.

![WebDAV Mac Step 4](http://i.imgur.com/HcuBGmq.png)

### Windows

On Windows, open __File Explorer__. On the left sidebar, you should find the __Network__ icon.

![WebDAV Windows Step 1](http://i.imgur.com/KhKCetD.png)

Right click on the __Network__ icon. It should show the context menu with the option __Map network drive__. Click on that.

![WebDAV Windows Step 2](http://i.imgur.com/KYLEwSv.png)

Enter the server address in the folder field. It should be __http://<your.server>/webdav__. Select the __Connect using different credentials__ if your login is different. Press __Finish__.

![WebDAV Windows Step 3](http://i.imgur.com/PbYhFXr.png)

You will be prompted for a username and password. Enter them and press __OK__.

![WebDAV Windows Step 4](http://i.imgur.com/d57Cul7.png)

Once you have connected, it should appear as a network drive on the left sidebar of your __File Explorer__.

![WebDAV Windows Step 5](http://i.imgur.com/CTPOwvI.png)

### Linux(Ubuntu)

We are using Ubuntu 14.04 as our Linux desktop operating system. On Ubuntu, open __Files__. THere is a __Connect to Server__ option on the left sidebar. Click on that.

![WebDAV Linux Step 1](http://i.imgur.com/XqbJm4t.png)

Enter the server address. It should be __dav://<your.server>/webdav__. Press __Connect__.

![WebDAV Linux Step 2](http://i.imgur.com/M3VuEE5.png)

You will be prompted for a username and password. Enter them and press __Connect__.

![WebDAV Linux Step 3](http://i.imgur.com/Sa5037A.png)

Once you have connected, the directory should appear under the __Network__ listing.

![WebDAV Linux Step 4](http://i.imgur.com/FQtteVI.png)

## Conclusion

In this article, we have gone through how to set up a WebDAV server using Apache on Ubuntu 14.04. We have also discussed how to configure Digest authentication to secure the server. Lastly, we have shown you how to connect to the WebDAV server from all 3 major operating systems using their native file browsers.
