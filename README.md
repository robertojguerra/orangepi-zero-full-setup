# orangepi-zero-full-setup
How to setup the orange pi zero completely

Contents
* Get your orangepi zero, power supply and USB-serial
* Install Armbian
* Start interfacing with Serial, Ethernet or Wifi
* Use the USB ports
* Make a 3d printed case
* Build your own image with added TV output, 3D acceleration
* Install the x and openbox windows managers and xlde desktop
* Test it with 3D graphics
* Install video codec acceleration
* Test it H264 videos

<H1>1. Get your hardware :)</H1>

<p float="left">
<img width=200 src="https://user-images.githubusercontent.com/3515329/227690645-c09604de-25f1-4790-aaf6-da139cda9219.png" />
<img width=300 src="https://user-images.githubusercontent.com/3515329/227690554-845e365b-dbbe-42bf-8b02-f3ec1da0d3d3.png" />
<img width=200 src="https://user-images.githubusercontent.com/3515329/227691726-dd3f12f7-fad9-4eb5-9e47-4e8c6020b8bc.png" />
</p>

<p float="left">
<img width=200 src="https://user-images.githubusercontent.com/3515329/227691083-cb2ad85b-2124-4a5e-9fd1-5fc7d331b45f.png" />
<img width=200 src="https://user-images.githubusercontent.com/3515329/227697165-1cceb21c-1e4d-49fd-9c9e-d236038d5b8b.png">
</p>

Note: USB charger should be 1.0 amp or greater

The orangepi zero is available from many sellers in Aliexpress, eBay and Amazon
https://www.aliexpress.us/item/3256805178614559.html
https://www.amazon.com/Laiaouay-Orange-Development-Protective-Open-Source/dp/B09TZV6Q2R/
https://www.ebay.com/itm/334144569380

USB-Serial: https://www.amazon.com/HiLetgo-CP2102-Converter-Adapter-Downloader/dp/B00LODGRV8/

Some of them have the H3 CPU, some H2+ CPU, but they all work the same.

Some have 256MB RAM, and some have 512MB... all are suitable for light server applications and python programs.

Get a microSD card of "Class 10", 16 GB or larger, to have acceptable speed and space to grow.

Wifi works slowly, so it is preferable to use an ethernet cable.

<H1>2. Install Armbian</H1>

Visit armbian.com, click "Download", "armhf" (it means arm 32 bit hard float, a class of ARM CPUs).

<img src="https://user-images.githubusercontent.com/3515329/227692998-df69bdbc-5d09-4102-b90c-997e1e97727a.png">

Scroll down to "Orange Pi Zero". Not Zero+. Click.

Look "Supported", "Armbian 23.02 Bullseye", "Direct Download" or "Torrent Download". Click.
* 23.02 is the Armbian version. Bullseye is the Debian version. Kernel 5.15 is the Linux kernel version.

You should see a file Armbian....img.xz starting to download, 424 Mbyte approx.

Download "Balena Etcher"

https://www.balena.io/etcher
<img src="https://user-images.githubusercontent.com/3515329/227697052-ab88a251-2823-4606-a594-0bdc0cbdc8d9.png">

Select the 
