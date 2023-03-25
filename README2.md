*warning: these steps require an intermediate experience with Linux commands*

<h1>7. Building your own system</h1>
<h2>7.1 Create a Linux virtual machine</h2>

* Download and install VirtualBox: https://www.virtualbox.org/wiki/Downloads

* Download a Server Ubuntu "Jammy Jellyfish" system: https://releases.ubuntu.com/jammy/ (recommended compilation OS on March 2023)

* Create a virtual machine with 1 GB RAM and "bridged networking"

* Start the VM, install Ubuntu Jammy Jellyfish.

<img width=600 src="https://user-images.githubusercontent.com/3515329/227738861-54c57422-7d6b-4c90-95f4-d84e4d5936b3.png">

* Login as a regular user: find its IP with "ip a", and from Putty or Linux terminal, connect with SSH. In the example below, the address is 192.168.1.131

<p>
<img width=600 src="https://user-images.githubusercontent.com/3515329/227739034-d7d1b334-622b-4b24-8ce8-aba7292baa07.png">
<img width=400 src="https://user-images.githubusercontent.com/3515329/227739119-a6a0d731-ab1e-4f89-b2e8-2eab8b38ed81.png">
</p>

<h2>7.2 Download the Armbian OS building script from Github</h2>

* mkdir armbian-2023.05

* cd armbian-2023.05

* git clone --depth=1 --branch=main https://github.com/armbian/build

* cd build

* ./compile.sh

* You will be presented the Armbian "TUI" (text user interface)

<img width=600 src="https://user-images.githubusercontent.com/3515329/227739289-a34b98e2-4902-4c95-850a-4e5a630c34b0.png">

* Keep the default option: "Do not change the kernel configuration"

* In the next screens, select"orangepizero", "Choose a kernel: current ... LTS kernel", "Bullseye Debian 11", and "Standard image with console interface"

<img width=600 src="https://user-images.githubusercontent.com/3515329/227739379-b118881f-2af0-4eb5-840c-b75b82b738b0.png">

* Then the script will start preparing the packages, sources, patches.

* It will take a long time and it may need broadband if there's large downloads.

* Once it finishes, the OS image is written in ../output/images. Use Balena Etcher.

* Run this OS in the OrangePi Zero, to make sure you have a good OS building system. Test that you can login with SSH.

* Congratulations! Now you have your own standard Armbian OS, which you built yourself!

<h2>7.3 Add the TV encoder driver to the Linux kernel source</h2>

* Download these patches:

  * <a href="sunxi-6.1/0036-wip-h3-h5-cvbs-armbian.patch">sunxi-6.1/0036-wip-h3-h5-cvbs-armbian.patch</a> : makes additions to the "dts", which tells the kernel where are the new devices. Adds kernel code to interact with the tv encoder. With my modifications, now it is applicable to Armbian (this patch came from the LibreElec github).

  * <a href="sunxi-6.1/zzzz2-tv.patch">sunxi-6.1/zzzz2-tv.patch</a> : by Armbian user "gleam2003", adds directives to make sure that the dtbo (device tree binary overlay) is compiled

  * <a href="sunxi-6.1/zzzz3-tv.patch">sunxi-6.1/zzzz3-tv.patch</a> : more additions to the "dts" and "dtsi" (like C include files), which I noticed were included in "yam" patch, but missing from the LibreElec patch
  * Reference: https://forum.armbian.com/topic/16804-solved-orange-pi-pc-h3-armbian-focal-5104-sunxi-av-tv-out-cvbs-enable/page/2/ and https://forum.armbian.com/topic/22226-orange-pi-zero-lts-tv-out-in-2022/

* Move them to ~/armbian-2023.05/build/userpatches/kernel/archives/sunxi-6.1

* Run ./compile.sh again. In the "TUI", <b>ask to CHANGE KERNEL CONFIGURATION</b>. Continue with the rest of configurations as before.

* In Kernel Config, go to "device drivers", "graphics support", find "lima" and select it

*insert photos of kernelconfig screens*

* Since you have new user patches and new kernel module selected, the armbian script will download _THE LINUX KERNEL SOURCE VERSION 6.1_ This may take 1 hour.

* You will see patches being applied to several files. This is the part that will fail if a patch is mis-formatted, or there are errors in the content if the patch. The script will tell you which patch, and what part was bad.

* Then you will see the u-boot compilation

* Then you will see the *_LINUX KERNEL_* compilation. This may take 2 hours, and it may fail if the C code is bad due to the patches

* If your downloaded Linux kernel is not version 6.1, you may need to manually fix the patches, to harmonize with the new Linux source. Sometimes, the line numbers change. Sometimes a function gets moved or deleted. Sometimes the parts of patch is "mainlined" (added to the official kernel), so the patch would have to have that part removed.

* You will end up with a ...img.xz file. Write it on the microSD with Balena Etcher.

<h2> 7.4 First time with your OWN Armbian OS, with your OWN changes 🤟</h2>

* Test it in the OrangePi Zero. Repeat the same account configuration as the first time, though serial or SSH.

* Run "armbian-config". In the "TUI" select "System", "Hardware". Find "tve" and select it.

* Press "enter". Do not reboot yet. Select "Boot environment". In "console=serial", replace "serial" with "both". This allows the kernel to print on the TV screen during startup.

<h2> 7.5 You will need a new video cable </h2>

* Modify a composite video cable (yellow AV cable) with 2 female "Dupont" connectors and a 50 ohm resistor as shown below

*insert photo of video cable for orangepi zero*

* Connect them to the pins "GND" and "TVOUT" in the OrangePi Zero. Connect the AV connector to the TV, and turn the TV on.

* Reboot with "sudo reboot"

* You will be greeted with the Linux boot up screen on the TV 🙂

* The text is overflowing to the left, right, upper, lower edges of the TV, due to "overscan".

* This flaw was not present in Kernel 4.x. This is a flaw in the current driver that we as a community should try to resolve.

<h1>8. Install the xinit and xlde desktop</h1>

* apt install xinit xterm xauth x11-apps xserver-xorg g3dviewer mesa-util lightdm

* Reboot Orangepi Zero

* You should see a graphical logic screen

* The lxde desktop will be "overscanned" beyond the edges of the TV. You will have to live with this until someone comes up with a solution.

<h1>9. Activate 3D graphics acceleration and test</h1>

* The OrangePi Zero ARM CPU, _Allwinner Sun8i H3_ has an embedded _Mali 400 GPU_.

* Fortunately, the open source _Lima.ko_ driver for this GPU is already included by default in Armbian 😄

* Run "sudo modprobe lima" and check that this kernel module, driver for the Mali 400 GPU, is loaded in Linux.

* Run "lsmod" and check that the resulting lines have "lima" and "gpu_sched". This means that the 3D driver is loaded.

* Log out of the lxde session, and log back in.

* Test the 3d graphics by opening a terminal: "glxinfo" to confirm that the "lima" 3D driver is in use.

* Type "glxgears" to see a demo of the OrangePi Zero 3D acceleration 🌠

<h1>10. Activate and use analog audio</h1>

* Enter "armbian-config" again. Select "System", "Hardware". Check the first item "analog-codec"

<img width=400 src="https://user-images.githubusercontent.com/3515329/227737255-d5c87f7e-4a7d-4eb7-9519-51b0638dfa91.png">

* Reboot and prepare another AV cable with 2 coax connector, ideally white and red, and terminate it with "Dupont" connectors as shown below:

*insert photo of audio cable*

* Connect the OrangePi Zero to the TV audio inputs or speakers (amplified, not a speaker by itself)

* Configure the audio hardware.

* Install audio application: "sudo apt install mpg123"

* Test with a WAV file: _aplay yoursoundfile.wav_

* Test with a MIDI file: _aplaymidi yourmidifile.midi_

* Test with an MP3 file: _mpg123 yourmp3file.mp3_

<h1>11. Install and test H264 video codec hardware acceleration</h1>

* The OrangePi Zero ARM CPU, _Allwinner Sun8i H3_ has an embedded _video encoder and decoder_, and H264 is one of the most useful codecs in there.

* Fortunately the open source _sunxi-cedrus.ko_ which handles the codec, is already included in Armbian.

* Type "sudo modprobe sunxi-cedrus" to load the kernel module. Type "lsmod" to see the several kernel modules that were loaded at the same time.

* Install non-accelerated video players, to measure the initial performance: "sudo apt install mpv ffmpeg".

* Test the non-accelerated video playing: "mpv yourh264file.mp4" and "ffplay yourh264file.mp4".

* With another terminal window, run "htop" and see the CPU load (the H3 has 4 CPU cores). Also notice the video smoothness.

* Install the accelerated video players: <b>NOT DONE YET</b>
