---
layout: post
title:
twitter_text: Installing MJPG Streamer on Raspberry Pi
tags: [Raspberry Pi, MJPG Streamer, Video]
category: Raspberry Pi Tutorials
thumbnail: /thumbs/mjpg-streamer-on-raspberry-pi.png
description: Streaming video output on webserver or media player through camera connected on Raspberry Pi can be used for many applications. You can stream video from Raspberry Pi Camera to Web Browsers, on Android, IOS and Windows!
---
<div class="row">
<div class="intro">
<div class="paragraphs">
  <div class="row">
    <div class="span4">
      <div class="clearfix">
			<img class="pull-left" src="{{ page.thumbnail }}" alt="{{page.title}}">
			<i class="fa fa-quote-left fa-2x fa-pull-left fa-border"></i>
      </div>
      </div>
    </div>
  </div>
</div>
</div>

Motion JPG is a video compression format in which each video frame or video sequence is compressed separately as a JPEG image. MJPG-streamer takes JPGs from compatible cameras or other input plugins and streams them as M-JPEG via HTTP to web browsers and other media players.

* Do not remove this line (it will not be displayed)
{:toc}

## A. Essentials

Requirements:

  - Raspberry Pi
  - Raspberry Pi Camera Module


## B. Connect the Camera Module


![Raspberry Pi Camera Port](/images/raspb-camera-connection.png "Raspberry Pi Camera Port"){: .center-image }*Camera connection from both sides*
  - Locate the camera port and connect the camera as shown.
  - Open the `Raspberry Pi Configuration` Tool from `Preferences` on the main menu

![Raspberry Pi Camera Enable](/images/raspi-camera-config.png "Raspberry Pi Camera Enable"){: .center-image }

  - Enable the `Camera` from `Interfaces` tab if Disabled and Reboot the Pi.


## C. Installing Dependencies

  Install dev version of libjpeg:

{% highlight bash %}
sudo apt-get install libjpeg62-turbo-dev
{% endhighlight %}

  Install make:

{% highlight bash %}
sudo apt-get install cmake
{% endhighlight %}

## D. Installing MJPG Streamer

  Download mjpg-streamer with raspicam plugin:

{% highlight bash %}
git clone https://github.com/jacksonliam/mjpg-streamer.git ~/mjpg-streamer
{% endhighlight %}

  Change directory:


{% highlight bash %}
 cd ~/mjpg-streamer/mjpg-streamer-experimental
{% endhighlight %}

  Compile:

{% highlight bash %}
 make clean all
{% endhighlight %}

  Replace old jpg-streamer:

{% highlight bash %}
sudo rm -rf /opt/mjpg-streamer
sudo mv ~/mjpg-streamer/mjpg-streamer-experimental /opt/mjpg-streamer
sudo rm -rf ~/mjpg-streamer
{% endhighlight %}


## E. Start Streaming


  To Begin streaming type:


{% highlight bash %}
 LD_LIBRARY_PATH=/opt/mjpg-streamer/ /opt/mjpg-streamer/mjpg_streamer -i "input_raspicam.so -fps 15 -q 50 -x 640 -y 480" -o "output_http.so -p 9000 -w /opt/mjpg-streamer/www"
{% endhighlight %}

You can change the above parameters

| Parameter 	|      Point      	|                Details               	|
|:---------:	|:---------------:	|:------------------------------------:	|
|     -i    	|      input      	|           input parameters           	|
|     -o    	|      output     	|           output parameters          	|
|    -fps   	|    framerate    	| video framerate, default 5 frame/sec 	|
|     -q    	|     quality     	|  set JPEG quality 0-100, default 85  	|
|     -x    	|   width/x-axis  	|  width of frame capture, default 640 	|
|     -y    	|  height/y-axis  	| height of frame capture, default 480 	|
|     -p    	|    HTTP port    	|     TCP port for this HTTP server    	|
|     -w    	| web page folder 	|     folder that contains webpages    	|
  
You will see something like this

{% highlight bash %}
MJPG Streamer Version.: 2.0

i: fps.............: 15

i: resolution........: 640 x 480

i: camera parameters..............:

Sharpness 0, Contrast 0, Brightness 50, Saturation 0,

ISO 400, Video Stabilisation No, Exposure compensation 0

Exposure Mode 'auto', AWB Mode 'auto',

Image Effect 'none', Metering Mode 'average',

Colour Effect Enabled No with U = 128, V = 128

Rotation 0, hflip No, flip No

www-folder-path...: /opt/mjpg-streamer/www/

HTTP TCP port.....: 9000

username:password.: disabled

commands..........: enabled

Starting Camera

Encoder Buffer Size 81920
{% endhighlight %}
  Now type the this url in your browser `http://localhost:9000/stream.html` to view the streamed output locally or type the IP address of Raspberry Pi with port like `http://<IP-address>:9000/stream.html` to watch from another computer/device in your network.

#### Find IP address:

To find IP address there are many ways, one of them is by by typing `ifconfig` in terminal

{% highlight bash %}
sudo ifconfig
{% endhighlight %}

  It will be something like this
  
{% highlight bash %}
eth0      Link encap:Ethernet  HWaddr 09:00:12:90:e3:e5  
          inet addr:192.168.1.29 Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe70:e3f5/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:54071 errors:1 dropped:0 overruns:0 frame:0
          TX packets:48515 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:22009423 (20.9 MiB)  TX bytes:25690847 (24.5 MiB)
          Interrupt:10 Base address:0xd020 

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:83 errors:0 dropped:0 overruns:0 frame:0
          TX packets:83 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:7766 (7.5 KiB)  TX bytes:7766 (7.5 KiB)

wlan0     Link encap:Ethernet  HWaddr 58:a2:c2:93:27:36  
          inet addr:192.168.1.64  Bcast:192.168.2.255  Mask:255.255.255.0
          inet6 addr: fe80::6aa3:c4ff:fe93:4746/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:436968 errors:0 dropped:0 overruns:0 frame:0
          TX packets:364103 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:115886055 (110.5 MiB)  TX bytes:83286188 (79.4 MiB)
{% endhighlight %}

In this case `192.168.1.49` is the IP-address
You should enter address like `http://192.168.1.49:9000/stream.html` in your browser to view streaming.

## F. Stop Streaming

  To stop streaming type:
  
{% highlight bash %}
kill -9 'pidof mjpg_streamer'
{% endhighlight %}
