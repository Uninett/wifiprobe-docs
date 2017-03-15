## Starting Ansible
After the Ansible inventory, known_hosts and configuration files have been
exported, the web server will start Ansible in a subprocess, and tell it to
pipe all its ouput to a logfile in logs/, with the filename equal to the
username of the logged in user. The server will parse this file each time the
probes webpage loads, and will display the current status.
