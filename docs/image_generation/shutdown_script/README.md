## The WiFi dongle shutdown script
Because the Raspberry Pi has no off-switch, there's no easy way to shut it down
when not connected to a monitor, and when a tty is not available. It is still
important to shut it down properly though, to firstly avoid file corruption,
but also to make sure the SSH tunnel to the server is closed properly.

To achieve this, the WiFi dongle itself is used as an off-switch. With the help
of a udev script, the RPi will shut down when a USB device with a specific
combination of IDs is *physically ejected*. Those IDs are set to the model ID
and vendor ID of the D-Link DWA-171 WiFi dongle. For information on how to
retrieve these IDs, and what the script looks like, consult the
generate_image.sh script file.

So in essence: to shut the probe down, physically eject the WiFi dongle and
wait until the green light shines constantly (ususally takes 10-15 seconds).
