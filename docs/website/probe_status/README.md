## Probe status
After adding a probe through the web interface, three statuses will appear (see
image in presentation mentioned in introduction, or just log in to the website).

The first status is for identification. It will be green if the probe has been
identified, i.e. has sent it's SSH key and host key. It will be yellow if it
has not been identified and the server is waiting for identification. It will be
red if the identification period has expired.

The second status show whether the probe is connected to the server, i.e. there
exists an SSH tunnel at the port designated to the probe. It will also show
whether eth0 or wlan0 is up. If only eth0 is up, something is wrong as the probe
will not be able to make measurements. If only wlan0 is up, that's okay, as the
probe doesn't have to connect to the server through eth0 (it can use wlan0 too).

The third status tells whether a probe has been updates or not, and if it is
currently updating. It will show "Not updated" if the probe has never been
updates. It will show "Updating..." if an update is currently in progress.
Otherwise it will show the time since the probe was last updated. If an update
fails, it will say "Failed". This will in most cases make the probe stop doing
measurements.
