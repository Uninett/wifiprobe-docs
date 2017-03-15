## Probe identification API
New probes must identify themselves to the web server before it will push Ansible
configs to them. This is done in a two step process. The main reason for this
process is that probes will most likely be behind NAT, and must therefore
send the server keys - and the server must send the probe an available port
for SSH - before it can start a reverse SSH tunnel (see [The SSH tunnel
procedure][]). Ansible will not be able to push its configuration without this
tunnel.

### Identification
In the first step, the probe sends a POST request to the server (the Probe's
init script does this, see [Probe initialization][]), with it's newly
generated SSH public key and its host key. It uses its wlan0 MAC address to
identify itself. If a probe with the attached MAC address exists in the
database (i.e. has been added through the web interface) and has not already
been registered, the server will save the keys and associate them with that MAC
address from now on.

NB: All references to MAC addresses in this document refer to the WiFi dongle's
MAC address, which will be seen as the MAC address of the wlan0 interface in
Linux. Also note that the internal WiFi card present on the RPi 3 is disabled to
avoid conflicts.

The probe sends a POST request with the following key/value pairs:

- pub_key=[pub_key]
- host_key=[host_key]

The server will return one of the following responses:

| Return value                | Explanation 
| :-------------------------- | :-------------------------------------------------
| invalid-mac                 | The supplied MAC was invalid (in form)
| unknown-mac                 | The MAC was valid, but is not in the database
| invalid-pub-key             | The pub key was invalid (in form)
| invalid-host-key            | The host key was invalid (in form)
| already-registered          | There already exists keys associated with this MAC
| assocation-period-expired   | The identification period has expired and needs to be renewed through the web site
| success                     | The keys were successfully registered/identified with the corresponding MAC address

When a probe is added through the web interface, it gets an initial 40 minutes
of identification time. If no probe identifies with the corresponding MAC address
within that time, then the identification time will expire, and the server will not
accept any keys sent afterwards. This is to prevent a potentially malicious
person from registering his own probe before the real user has gotten time to do it,
e.g. if the user waits a long time before connecting the RPi to the internet
after having registered it.

### SSH port query
The second step will be performed after the probe has been identified, and
consists of a GET request with the probe's MAC address attached in a mac=vale
attribute pair. If an identified probe with that MAC exists in the database,
the server will return a port number from 50000 to 65000 (inclusive). This
port number will be used by the probe to initiate an SSH tunnel.

The probe sends a GET request with the following key/value pair:

- mac=[mac]

The server will return one of the following responses:

| Return value                | Explanation 
| :-------------------------- | :-------------------------------------------- 
| invalid-mac                 | The supplied MAC was invalid (in form)
| unknown-mac                 | The MAC was valid, but is not in the database 
| no-registered-key           | No SSH key has been associated with this MAC, and therefore no port will be sent
| [port]                      | Returns the queried port (a valid MAC was received)

Wher the probe receives the network port, it will construct an SSH command with
it. This command ill be wrapped in a systemd unit file that automatically restarts 
if the script fails, to prevent the SSH tunnel from breaking (as far as possible).

### The SSH tunnel procedure
This section explains the complete procedure the server and probe goes
through to be able to successfuly initiate an SSH connection. It is assumed
that the main server user (the user that will run Ansible) is called MAIN, the
dummy user on the server is called DUMMY, and the main user on the probe is
called PROBE (in reality it will be root for Kali Linux).

1. MAIN's public SSH key is preloaded on a Kali Linux ARM image as an
*authorized key*, and the server's host key is added as a *known host*
2. PROBE generates a key pair on first boot
3. PROBE sends its public SSH key and hosts key to MAIN, together with its
wlan0 MAC address for identification.
4. The received host key is saved as a *known host*, and the received public
key is saved as an *authorized key* for DUMMY

In the end: both machines will be known to each other. PROBE will be
authorized to start a degenerate SSH tunnel to DUMMY (no tty allowed), 
while MAIN will be authorized to use the SSH tunnel to login to PROBE.
