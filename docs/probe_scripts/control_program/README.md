## The control program
The control program is a Python 3 program whose main purpose is to call the
WiFi scripts and specified intervals, and send their results to the specified
database(s). It will read the script_configs file exported by Ansible (see
[Data exporting][]), and from that make an instance of a Script class for each
entry in the configuration. It mainly contains information about the script
name, the interval it should be run at, and whether it should be run at all (is
enabled or not).


The program first does some initialization (e.g. makes sure the probe is
connected to the internet), before running this endless loop:

- Run all ready scripts (scripts whose inner timer has an elapsed time higher
  or equal to the interval specified in the config). NB: The scripts are run
serially (i.e. not in parallel).
- Read the results_report file. If it is not empty:
    - Parse its content to a database friendly format
      (for the moment ElasticSearch is used) and send it to the database.
    - Clear the results_report file
- Wait for 2 seconds and repeat.
