# Ansible
Ansible is what actually converts the RPis to WiFi probes (i.e. installs the
required programs, transfers scripts etc.). When an SSH tunnel
has been established, and the user presses the "Push configuration to probes" button on
the website, the data saved in the SQL database will first be converted to
Ansible-friendly YAML config files, and then Ansible is run.

All file and directory references will be relative to the Ansible project
directory (i.e. the ansible-probes directory in the root directory of the
website project).

When setting up Ansible for the first time, some configs need to be changed.
See [Website Setup](../website/setup/README.md) for details.
