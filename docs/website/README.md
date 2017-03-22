# Website
The website is the front-end a regular user will use to add and administer his
probes and information like database credentials (for the database(s) the probe
will send measurement results to). The website's uses are roughly:

For administrators:

- Add new users
- Edit/remove existing users

For regular users:

- Download probe image
- Add new probes, more specifically:
    - Name, wlan0 MAC address, location
    - Network configuration (SSID, anonymous ID, username, password, wpa
      certificate)
    - Script/test configurations (For each script: the interval they should be
    run at (in minutes), and whether they are enabled or not, i.e. whether they
    should be run at all)
- Edit/remove probes
- Add database information
    - Only necessary if the user does not want to use UNINETT's provided elastic
      search instance
- Follow link to see data in Kibana

For probes (see [Probe identification API](docs/website/probe_identification/README.md)):

- Register their public SSH key and host key
- Receive a port for SSH tunnel

The website's back-end is written in Python 3 and uses the Flask web framework. The
front-end uses templated HTML files (Jinja2) and the Bootstrap CSS framework.
All data submitted to the website will be saved in an SQLite3 database.

## Complete registration procedure
See the Instructions tab on the website for instructions on the how register
probes.

## View collected data
If the default database is used (UNINETT's ElasticSearch instance), users can
view the collected data in Kibana by clicking on the MAC address in the Probes
tab on the website. This will redirect them to Kibana.
