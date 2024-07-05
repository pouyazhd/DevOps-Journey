# Systemd

## Location

The system init process uses 3 places to keep unit files

1. `/etc/systemd/system/` --> Most priority
2. `/run/systemd/system/`
3. `/lib/systemd/system/` --> Least priority

* The unit files in `/etc/` has the most priority.
* Package managers are usually install their files in `/lib/` directory.
* The unit files can be overwrite. So becareful to change them.

## Types of units

* Service
* Socket
* Target
* Time --> Also work like `cron jobs`
* Mount --> Also work like `fstab`
* Automount

## Commands

The general command is:

```bash
sudo systemctl [command] [unit-name]
```

The general commands are:

* start
* stop
* status
* restart: stop and then start the service
* reload: only reload configs
* edit: edit the unit file

> CAUTION: Unit files are case sensitive. The keywords must start with Capital letter.

Some commands are not taking `[unit-name]` section.

* daemon-reload: make sure all changes we done are applied to systemd.
* list-units: shows the list of units

## Service file example

```service
[Unit]
Description=Nexus repository manager  service
After=network.target

[Service]
Type=forking
Restart=on-abort
RestartSec=1
User=root
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
LimitNOFILE=65536
 

[Install]
WantedBy=multi-user.target
```

## Refrences

* [Systemd Deep-Dive(video)](https://youtu.be/Kzpm-rGAXos)
