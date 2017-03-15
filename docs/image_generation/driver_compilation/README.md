## Driver compilation
The currently used WiFi dongle, "D-Link DWA-171", needs the rtl8812au driver to
work on linux. The driver needs to be compiled for each kernel version. Because
you need to compile the whole Linux kernel before compiling the driver, the
compilation will be very slow on the Raspberry Pi (RPi). Therefore it is best
to cross compile. To do this you can use the script located at 
`ansible-probes/roles/driver/files/compile_driver.sh`.

This script will download the required cross compilation toolchain, collect the
required files from a running RPi and compile the WiFi driver for it. It needs
the following arguments:

1. The address of the RPi running the kernel version you want to compile the
 driver for. This will be the same address as the one you use to SSH into it,
so either a DNS record or IP address.
2. The kernel version the RPi is running. This can be retrieved by running
 `uname -r` on the RPi itself. The output it gives should be the argument.
3. The location where you want the script to save the compiled driver.

Do note that the compilation can take some time. On my (average) laptop it
takes about 1 hour.

Also, there should already be a compiled driver for the 4.1.9 (4.1.9-v7 for
rpi2/3) kernel version in the image_generation directory. So if that is the 
kernel version being used, you can just use that instead of compiling a new one.
