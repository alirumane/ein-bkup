---
layout: post
title: Install LIGHTTPD web server on Raspberry Pi
tags: [Raspberry Pi, lighttpd, server]
category: Raspberry Pi Tutorials
thumbnail : /thumbs/install-lighttpd-on-raspberry-pi.png
description: Lighttpd is a webserver optimized for speed and reduced CPU-load. It provides setting up a web server without loading the limited processing capability which is ideal for providing web access to the Raspberry Pi as a monitoring tool, or as a lightweight webserver for a personal use.
---

<p><span class="image left"><img src="{{ page.thumbnail }}" alt="" /></span> <i class="fa fa-quote-left fa-2x fa-pull-left fa-border"></i></p>

[Lighttpd](http://www.lighttpd.net/) is an open-source web server optimized for speed-critical, high-performance environments while maintaining standards-compliant, secure and flexible.

It has a very low memory footprint compared to other web servers and takes care of CPU-load.
Running a light webserver on Linux with Lighttpd and Raspberry Pi:

## A. General

Overview and comparison of Memory Usage and Connection Requests.

#### Memory Usage

![Webserver Memory Usage Graph]( {{site.url}}/images/Webserver_memory_graph.png "Webserver Memory Usage Graph" )

 From the above image it is clearly seen that Lighttpd uses less memory compared to others which is quite good for Raspberry Pi.


#### Connection Request

![Webserver Request Graph]( {{site.url}}/images/Webserver_requests_graph.png "Webserver Request Graph" )

Also the requests per second for Lighttpd is suitable for  applications running on Raspberry Pi.


## B. Setting up

#### 1. Installing lighttpd

To install the lighttpd web server type the following commands in terminal

```bash
sudo apt-get install lighttpd
```

This will install the web server and other required dependencies.

#### 2. Enabling CGI

CGI and FastCGI are not necessary though.

{% highlight bash %}
sudo lighttpd-enable-mod cgi
{% endhighlight %}


#### 3. Enabling FastCGI

{% highlight bash %}
sudo lighttpd-enable-mod fastcgi
{% endhighlight %}


## C. Configuring Server


You need to change the default location of html in web-directory.  Type the following command in terminal


{% highlight bash %}
 sudo nano /etc/lighttpd/lighttpd.conf
{% endhighlight %}

you will see


```apacheconf
server.document-root        = "/var/www/html"
server.upload-dirs          = ( "/var/cache/lighttpd/uploads" )
server.errorlog             = "/var/log/lighttpd/error.log"
server.pid-file             = "/var/run/lighttpd.pid"
server.username             = "www-data"
server.groupname            = "www-data"
server.port                 = 80
```

Change

server.document-root        = "/var/www/html"


to


{% highlight bash %}
 server.document-root        = "/var/www/"
{% endhighlight %}


## D. Restarting Server


You can restart the server with any two commands below


OR


* Direct Reload


{% highlight bash %}
 sudo service lighttpd force-reload
{% endhighlight %}


* Stop-Start Reload


{% highlight bash %}
sudo /etc/init.d/lighttpd stop

sudo /etc/init.d/lighttpd start
{% endhighlight %}


## E. Set permissions on the web directory

Change the permissions on the `www` directory to allow user to update the webpages without needing to be root.




Change the directory owner and group

{% highlight bash %}
 sudo chown www-data:www-data /var/www
{% endhighlight %}

To allow the group to write to the directory type the following in terminal

{% highlight bash %}
 sudo chmod 775 /var/www
{% endhighlight %}

 To add the pi user to the www-data group

{% highlight bash %}
 sudo usermod -a -G www-data pi
{% endhighlight %}

 > It is necessary to logout current user and login again to make the group permissions.
 It is better to `restart` the Raspberry Pi to set the permissions.

## E. Testing the server

Once the setup is complete you can access the web page by typing the IP address of the Raspberry Pi in web browser.

 You have successfully installed and configured Lighttpd web server on Raspberry Pi!
