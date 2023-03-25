*warning: these steps require an intermediate experience with Linux commands*

<h1>7. Building your own system</h1>
<h2>7.1 Create a Linux virtual machine</h2>

* Download and install VirtualBox: https://www.virtualbox.org/wiki/Downloads

* Download a Server Ubuntu "Jammy Jellyfish" system: https://releases.ubuntu.com/jammy/

* Create a virtual machine with 1 GB RAM and "bridged networking"

* Start the VM, install Ubuntu.

* Login as a regular user, find its IP with "ip a", and login through SSH

<h2>7.2 Download the Armbian OS building script from Github</h2>

* mkdir armbian-2023.05

* cd armbian-2023.05

* git clone --depth=1 --branch=main https://github.com/armbian/build

* cd build

* ./compile.sh

* You will be show the Armbian "TUI" (text user interface)

* Select "Do not change kernel configuration", "orangepizero", "current ... LTS kernel", "Bullseye", "Standard"

* Then the script will start preparing the packages, sources, patches.

* It will take a long time and it may need broadband if there's large downloads.

* Once it finishes, the OS image is written in ../output/images. Use Balena Etcher.

* Run this OS in the OrangePi Zero, to make sure you have a good OS building system. Test that you can login with SSH.

<h3>7.3 Add the TV encoder driver to the Linux kernel source</h3>

* Download these patches:

 * 0036-....patch : makes additions to the "dts", which tells the kernel where are the new devices. Adds kernel code to interact with the tv encoder. With my modifications to be applicable to Armbian (the patch came from the LibreElec github)

 * zzzz2-....patch : by Armbian user "gleam2003", adds directives to make sure that the dtbo (device tree binary overlay) is compiled

 * zzzz3-....patch : more additions to the "dts" and "dtsi" (like C include files), which I noticed were missing from the LibreElec patch

* Move them to ~/armbian-2023.05/build/userpatches/kernel/archives/sunxi-6.1

* Run ./compile.sh again. Repeat same selections in the "TUI"

* Because you have user patches, the armbian script will download *_THE LINUX KERNEL SOURCE VERSION 6.1_* This may take 1 hour.

* You will see patches being applied to several files. This is the part that will fail if a patch is mis-formatted, or there are errors in the content if the patch. The script will tell you which patch, and what part was bad.

* Then you will see the u-boot compilation

* Then you will see the *_LINUX KERNEL_* compilation. This may take 2 hours, and it may fail if the C code is bad due to the patches

* If your downloaded Linux kernel is not version 6.1, you may need to manually fix the patches, to harmonize with the new Linux source. Sometimes, the line numbers change. Sometimes a function gets moved or deleted. Sometimes the parts of patch is "mainlined" (added to the official kernel), so the patch would have to have that part removed.

* You will end up with a ...img.xz file. Write it on the microSD with Balena Etcher.

* Test it in the OrangePi Zero. Repeat the same account configuration as the first time, though serial or SSH.

* Run "armbian-config". In the "TUI" select "System", "Hardware". Find "tve" and select it.

* Press "enter". Do not reboot yet. Select "Boot environment". In "console=serial", replace "serial" with "both". This allows the kernel to print on the TV screen during startup.

* Modify a composite video cable (yellow AV cable) with 2 female "Dupont" connectors.

* Connect them to the pins "GND" and "TVOUT" in the OrangePi Zero. Connect the AV connector to the TV, and turn the TV on.

* Reboot with "sudo reboot"

* You will be greeted with the Linux boot up screen on the TV 🙂

* The text is overflowing to the left, right, upper, lower edges of the TV, due to "overscan".

* This flaw was not present in Kernel 4.x. This is a flaw in the current driver that we as a community should try to resolve.
