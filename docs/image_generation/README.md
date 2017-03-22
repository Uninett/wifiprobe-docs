# Image generation
A custom Linux ARM image is generated from a generic Kali Linux ARM image. To
generate the image, you use the script `image_generation/generate_image.sh`.
`generate_image.sh` takes five arguments:

1. The generic Kali Linux ARM image. You must use either the Raspberry Pi 1
or 2/3 version. Links to these images are in
`image_generation/image_url.txt`.
2. Driver for the WiFi dongle. This needs to be compiled for the correct
kernel beforehand. Ansible can later update the driver, but the probe needs a
preloaded driver so it can read the dongle's MAC address. See [Driver
compilation](driver_compilation/README.md) for how to do that.
3. The hostname/address of the web server the probe will connect to. This can
either be a DNS record or an IP address, but make sure to include the port
number if the web server listens on a port other than 80 (format:
`127.0.0.1:12345`).
4. The name of the unprivileged user the probe will start a connection to. If
you used/are going to use the server setup script ([Website Setup](../website/setup/README.md)), this
user will be named `dummy`.
5. The public ssh key of the user that will connect to the probes via
Ansible -> SSH. This will most likely be the same user that runs the web
server.

The script will copy the supplied image to a file with prefix `modified_`. The
modified image differs from the original in that it contains the following
components:

- Probe init script & systemd unit file
- Server user's pub ssh key & server's host key
- Server's hostname/address 
- WiFi driver (for MAC retrieval)
- WiFi dongle shutdown script (see [The WiFi dongle shutdown script](shutdown_script/README.md)
- Connection status script (used to show probe's connection status on the website)

After the image has been generated, it can be burned to an SD card by doing
something like:
```
dd if=modified_<original-image-name> of=/dev/sdf bs=1M conv=sync
```
Given `/dev/sdf` is the name of the SD card. This can be found by runing
`lsblk`.
