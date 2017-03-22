# Probe initialization
When a probe boots up for the first time, it will run the initialization (bash)
script preloaded into the Linux image. This script can be found in
image_generation/probe_init.sh. It essentially does the following things:

- Activate WiFi driver
- Install curl & dnsutils (for dig)
- Generate SSH key pair
- Gather wlan0 MAC address
- Register pub SSH key with server (identify with MAC) ([Probe identification API](wifiprobe-docs/docs/website/probe_identification/README.md))
- If registration is successful, receive a port number
- Generate a systemd unit file that runs the create_ssh_tunnel.sh script
    - In addition to connecting to the server via ssh, this script also modifies
      the network routing table to make sure only the connection to the server
      goes over eth0 (if available), while everything else goes over wlan0
      (measurements etc.)
- Initiate SSH tunnel to dummy user on server by starting systemd unit file and idle 
  (the tunnel will be degenerate -> /bin/false as shell)

If identification with the server is not successful (e.g. if the probe has not
been registered yet), the probe will just keep on trying.

At this stage the probe *must* be connected to the internet via ethernet,
because it currently has no WiFi credentials to use.
