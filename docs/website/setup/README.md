## Website Setup
The server requires some configuration before it can be used as a server for
the probes. To do this setup, you can run the script `setup_server.sh`. Note
that this script has only been tested in a Ubuntu environment, and only makes
the server ready to run a development sever, i.e. the one built into Flask. So
for apache/nginx, further configuration will be needed.

The script needs no arguments, but must be run as root (pretty much everything
is does requires root priveliges). The script does the following tasks:

- Install required programs (NB: At least Ansible 2.x is needed. If only Ansible
  1.x is in the repo, Ansible 2.x will need to be manually installed).
- Make a system user called Dummy
- Generate SSH keys for Dummy and move them to his home directory
- Copy get_probe_keys.sh to /usr/bin that will read the web sites SQL database to
  determine known hosts.
- Change sshd_config to make all SSH connections to Dummy only unable to start
  a shell (i.e. forces /bin/false to be run instead)
- Make a config file with the location of the website's SQL database
- Make a config file with the location of Ansible certificate dir

The files settings.py and secret_settings.py in the project directory must also
be created by copying from their corresponding example files and making
required changes.

There's also a lighttpd configuration file in the project directory that can be
used. The paths in the config will then need to be changed.

Furthermore, the files `roles/common/vars/main.yml`, `roles/common/vars/vault.yml`
and `group_vars/all/locations.yml` in the **ansible-project** directory will need
to be created. See their respective example files for what the content should
look like. NB: It's very important that they have the .yml-extension, even
thought the example versions don't have this. This is because ansible reads
the files in alphabetical order, so if the .yml-extension is omitted, the
example files will be read last and overwrite the proper config.

### Default user
When the database is first made, an admin user is automatically added. This
user must then add other users. At the moment it's not possible to make admin
users via the web interface, but it can be done by manually editing the SQL
database and setting a user's 'admin' attribute to 1.

The default admin username/password combination is `admin`/`admin`. The
password can be changed after logging in.
