# About this document

This document is meant as a technical documentation on how the WiFi probing
system works as a whole. It is meant to give an overview of the system, and not
necessarily explain the minute details of each operation. For that you can
check out the code (in a separate repo for the time being), as it also contains
some documentation.

The project mainly consists of the following parts:

- Linux image generation
- Various scripts for probe initialization
- Website for adding and administering probes
- Ansible setup for pushing configs inserted in website
- Scripts for probing the WiFi, and a program for controlling the scripts and
  submitting measured data

Each of these parts are explained below. Except when otherwise noted, all file
references will be relative to the root directory of the probe website 
project / git repository.
