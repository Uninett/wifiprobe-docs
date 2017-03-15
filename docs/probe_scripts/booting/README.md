## Booting the probe
After being shutdown as described in [The WiFi dongle shutdown script][], the
probe will automatically begin measuring. The WiFi dongle must logically
be connected, but an ethernet cable can also be connected if desired. This
can be useful as a safe-guard if e.g. an erroneous config is pushed, which
locks the probe out from the WiFi.

When booting, the probe will first make a ramdisk and copy the WiFi scripts to
it. After that has been done, the control program will start. This is to
mitigate tear on the SD card.
