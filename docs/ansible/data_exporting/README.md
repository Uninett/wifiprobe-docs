## Data exporting
When the button is pressed, the web server first checks which probes are
connected. From that it will generate an Ansible inventory file (a file containing
information about each probe to be updated) in inventory/, and a known_hosts file
for use with SSH. After that it will export some configs for the probes that are
connected and are going to be updated.

Ansible will first look in group_vars/all for config files, before checking
group_vars/[username] and at last host_vars/[probe MAC]. For example will the
default script configs be in group_vars/all, database information will be in
group_vars/username (because db info is local to each user), and probe
specific network configs may be in host_vars/mac (but also in group_vars if the
user has saved the settings as default values).

The data in the SQL database is converted to the following files (and can as
mentioned above be both group and host specific):

| Config filename  | Explanation
| :--------------  | :------------
| database_configs | information about the databases the probe will send data to
| network_configs  | information used by wpa_supplicant to connect to WiFi
| script_configs   | information about which script should be run, how often, and some other attributes. This file will be merged and converted to a JSON file on the probe

WPA certificates are also saved in the certs/ directory, which is divided up into
group_certs and host_certs. Certs saved as default will be saved in the
certs/group_certs/ directory, while host specific certs are saved in the certs/host_certs/
directory.
