## Ansible tasks
The Ansible setup is split into two roles (kind of like two parts);
*driver* and *common*. The driver role makes sure the driver is loaded, and
will either load it, install it or compile it, depending on the current state.
The common role is what does everything else. More specifically, it mainly does
the following:

- Install required programs
- Transfer probing scripts & script control program
- Fill in templated configs (WPA etc.) using the previously mentioned Ansible configs
- Transfer WPA certificates
- Transfer systemd unit files for control program and ramdisk creation & enable
  them
