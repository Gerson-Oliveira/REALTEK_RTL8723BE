# REALTEK_RTL8723BE
A repo for the newest Realtek rtlwifi codes. This code will build on any kernel 4.2 and newer as long as the distro has not modified any of the kernel APIs. IF YOU RUN UBUNTU, YOU CAN BE ASSURED THAT THE APIs HAVE CHANGED. NO, I WILL NOT MODIFY THE SOURCE FOR YOU. YOU ARE ON YOUR OWN!!!!!

Note: If your kernel is 4.17 or newer, AND your card is not an RTL8723DE, then you should NOT be using the external driver. The built-in one is the same.

It includes the following drivers:

rtl8192ce, rtl8192cu, rtl8192se, rtl8192de, rtl8188ee, rtl8192ee, rtl8723ae, rtl8723be, rtl8821ae, rtl8822be and rtl8723de.

Installation instruction You can find <> using lspci | grep Wireless. Afterwards, execute the following lines of codes in your shell:

You will need to install "make", "gcc", "kernel headers", "kernel build essentials", and "git".

If you are running Ubuntu, then

sudo apt-get install linux-headers-generic build-essential git

Please note the first paragraph above.

For all distros: git clone https://github.com/lwfinger/rtlwifi_new.git cd rtlwifi_new make sudo make install sudo modprobe -r <> sudo modprobe <>

Option configuration
If it turns out that your system needs one of the configuration options, then do the following:

sudo vim /etc/modprobe.d/<>.conf

There, enter the line below: options <<YOUR WIRELESS DRIVER CODE>> <<driver_option_name>>=<value>

The ONLY option you are likely to need is "ant_sel". The penny-pinchers at some laptop vendors, mostly HP, decided to save a few cents to install only one antenna. That would not have been a problem as the driver contains the logic necessary to handle this case; however, the idiots failed to correrctly encode that information in the devices ROM. Accordingly, the driver follow the ROM setting and uses the WRONG antenna. Effectively, the device has NO usable antennas. To test if this is the case on your system, please run the following commands (set 1):

DEVICE=$(iw dev | grep Interface | cut -d " " -f2) sudo iw dev $DEVICE scan | egrep "SSID|signal|(on"

If the signal for the AP to which you wish to connect is -60 or less, then you have this problem. The fix is to supply the "ant_sel" option. Run the following commands (set 2):

sudo su - echo "options rtl8723be ant_sel=2" > /etc/modprobe.d/50-rtl8723be.conf exit

If you have an RTL8723DE, make the appropriate adjustments to the above command.

At this point, do a complete shutdown! The device may retain the old setting with a warm reboot. To be safe, do a power off. After the system come back up, rerun the set 1 comamands again. If The signals are now a lot stronger, you are done. If not, repeat command set 2 with "ant_sel=1". If that does not help, I have no idea what is wrong.
