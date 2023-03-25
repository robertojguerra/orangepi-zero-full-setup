# orangepi-zero-full-setup
How to setup the orange pi zero completely

Contents
1. Get your orangepi zero, power supply and USB-serial
2. Install Armbian
3. Start interfacing with Serial, Ethernet or Wifi
4. Use the USB ports
5. Make a 3d printed case
6. Read and write digital inputs and outputs with C and Python
7. Build your own image with added TV output driver, 3D acceleration
8. Install the x and openbox windows managers and xlde desktop
9. Activate 3D graphics acceleration and test
10. Activate and use analog audio
11. Install video codec acceleration and test
12. Make your own audio/video apps with Python

<H1>1. Get your hardware 😍 🛠️</H1>

<p float="left">
<img width=250 src="https://user-images.githubusercontent.com/3515329/227690645-c09604de-25f1-4790-aaf6-da139cda9219.png" />
<img width=500 src="https://user-images.githubusercontent.com/3515329/227690554-845e365b-dbbe-42bf-8b02-f3ec1da0d3d3.png" />
<img width=250 src="https://user-images.githubusercontent.com/3515329/227691726-dd3f12f7-fad9-4eb5-9e47-4e8c6020b8bc.png" />
</p>

<p float="left">
<img width=300 src="https://user-images.githubusercontent.com/3515329/227691083-cb2ad85b-2124-4a5e-9fd1-5fc7d331b45f.png" />
<img width=400 src="https://user-images.githubusercontent.com/3515329/227697353-2728ee0d-f895-4b5b-a377-f16db92bbab8.png">
</p>

The orangepi zero is available from many sellers in Aliexpress, eBay and Amazon
https://www.aliexpress.us/item/3256805178614559.html
https://www.amazon.com/Laiaouay-Orange-Development-Protective-Open-Source/dp/B09TZV6Q2R/
https://www.ebay.com/itm/334144569380

USB-Serial: https://www.amazon.com/HiLetgo-CP2102-Converter-Adapter-Downloader/dp/B00LODGRV8/

Some of them have the H3 CPU, some H2+ CPU, but they all work the same.

Some have 256MB RAM, and some have 512MB... all are suitable for light server applications and python programs.

Get a microSD card of "Class 10", 16 GB or larger, to have acceptable speed and space to grow.

Wifi works slowly, so it is preferable to use an ethernet cable.

The USB charger should be 1.0 amp or greater

<H1>2. Download and Install Armbian 💾 🔥 </H1>

<img width=200 src="https://user-images.githubusercontent.com/3515329/227698435-1595b1e4-ac6e-4932-a19a-ff6c6e506247.png">

Visit armbian.com, click "Download", "armhf" (it means arm 32 bit hard float, a class of ARM CPUs).

<img src="https://user-images.githubusercontent.com/3515329/227692998-df69bdbc-5d09-4102-b90c-997e1e97727a.png">

Scroll down to "Orange Pi Zero". Not Zero+. Click.

Look "Supported", "Armbian 23.02 Bullseye", "Direct Download" or "Torrent Download". Click.
* 23.02 is the Armbian version. Bullseye is the Debian version. Kernel 5.15 is the Linux kernel version.

You should see a file Armbian....img.xz starting to download, 424 Mbyte approx.

Download "Balena Etcher"

https://www.balena.io/etcher

<img src="https://user-images.githubusercontent.com/3515329/227697052-ab88a251-2823-4606-a594-0bdc0cbdc8d9.png">

Select the downloaded file, select the destination disk (the microSD), and start writing the Linux filesystem in the microSD.

After verification, the microSD is ready to be inserted in the OrangePi Zero :happy:

<b>This paragraph is valid ONLY if you run Linux in your laptop</b><br>
By default, the system you just burned in the microSD card will connect to the wired ethernet network, and get an automatic DHCP address.<br>
If you want to configure a fixed address and wifi network, look into the microSD's  "/boot/armbian_first_run.txt.template"<br>
Duplicate it in the same folder without the ".template". Look at the instructions in that file.

Insert the microSD card in the OrangePi Zero


<H1>3. Start interfacing with your OrangePi Zero :lips: :telephone:</H1>
<H2>3.1 Serial (most reliable)</H2>
Connect the USB-serial adapter in the OrangePi Zero and your laptop.

<img width=400 src="https://user-images.githubusercontent.com/3515329/227698605-c79c2747-7a31-4553-9a1d-e3c4891726a1.png">

In your laptop download and run Putty (Windows) or Screen (Linux). The serial settings are 115200,8N1. The serial port *MAY* be COM3 or /dev/ttyUSB0

In Linux, run "screen /dev/ttyUSB0 115200"

Connect the USB cable to the charger and to the OrangePi Zero microUSB port. You should see the booting text messages in the Putty or screen window

<H2>3.2 Ethernet (the fastest and most flexible)</H2>
Connect the OrangePi Zero to your home router. Connect the USB chager.

Enter your home router "Network Status" page. Look for the list of DHCP clients.

Look for "orangepizero.local" or "orangepizero.lan" and take note of the IPv4 address (for example 192.168.1.xxx)

From your laptop's command line, connect via SSH protocol: "ssh root@192.168.1.xxx"

<img width=400 src="https://user-images.githubusercontent.com/3515329/227702326-f2a6eac0-c697-4f20-be45-7f4aee0f4dda.png">

Press Y to accept the SSH connection for the first time to this SSH server (the OrangePi Zero). This step ensures that no other device pretends to be this OrangePi Zero in the future.

<H2>3.3 Wifi (if you can't use cables)</H2>
Connect the USB charger.

Look for the OrangePi Zero address in the home router (same way way as the ethernet method).

Connect with SSH.

<H2>3.4 Once connected:</H2>

At first connection, the OrangePi Zero will ask you to create a root password, a user, and a user password.

Also, you can create "locales" if you want Linux in another language. For simplicity, choose "No" and "153 Skip"

Log out the root user by typing "exit". Login again as your normal user (it is safer to only use "root" only when necessary).

If you want to change the network or Wifi settings, type "sudo nmtui" (superuser do Network Manager Text User Interface).

<H1>4. USB Ports :electric_plug:</H1>

There is 1 type A USB port ready to USE. Let's check that it works.

With no USB devices connected, type "lsusb". Look at the result.

Plug in one device, or more with a USB hub.

<img width=700 src="https://user-images.githubusercontent.com/3515329/227699898-fe165788-cc21-489e-92ab-58e5e382f43c.png">

Notice in my case: one hub, one keyboard, one mouse and one webcam.

The values are the USB bus, device, the VID:PID, and the device name.

The nice thing about the OrangePi Zero is that there is a lot of functionality in the metal pins. If you use jumper wires, you can get 2 more USB ports:

<img width=500 src="https://user-images.githubusercontent.com/3515329/227700485-48298e69-502a-4772-87a7-34a294f407b0.png">

*usb cable photo here*

Also, if you get a USB OTG (on-the-go) cable, you can use the microUSB as a 4th USB port (you need to power the OrangePi through the metal pins)

<H1>5. Make a 3D Printed Case :factory: :hammer:</H1>

You have a running mini Linux server, but it is bare and exposed to metals, dirt, water, etc. We need to protect it.

If you have a 3D printer. There's a very nice OrangePi Zero case in Thingiverse.com. It even has the opening for the 2x13 array of metal pins facing to the side 🙂

https://www.thingiverse.com/thing:5424615

And here is the case that I designed, if you like fast and light prints, with sharp edges and editable in FreeCAD

*insert thingiverse link to my case*

*insert photo*

<h1>Read and write digital inputs and outputs with C and Python</h1>

*coming soon*

<h1>Parts 7 to 12 in Part 2... <a href=README2.md>GO! :scream: :rocket:</a></H1>
