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
# Example of a custom service file for nexus repository manager
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

## Unit section

* **Description**: This directive can be used to describe the name and basic functionality of the unit. It is returned by various systemd tools, so it is good to set this to something short, specific, and informative.
* **Documentation**: This directive provides a location for a list of URIs for documentation. These can be either internally available man pages or web accessible URLs. The systemctl status command will expose this information, allowing for easy discoverability.
* **Requires**: A unit file with a `Requires` dependency means units within this directive ***must be successfully activated*** when an activation signal is, otherwise the current unit will fail. Dependencies with this directive are strict.
* **Wants**: This is a less strict version of Requires. The Wants dependencies are activated upon activation of the containing unit file. This unit will be activated regardless of if the Wants dependencies fail.
* **Conflicts**: Units outlined within this dependency are automatically stopped upon activation of the containing unit file.
* **Requisite**: Units within this dependency must be already active before the unit is activated, otherwise systemd fails on the activation of the unit with the dependency.
* **Before**: This dependency directive explicitly declares ordering. This is important in situations when you’d want to start a unit before another. For this instance, the current unit is activated before the unit under the Before directive.
* **After**: This dependency is also used for ordering dependency activation. It’s the opposite of Before. The unit under this directive is/are activated before activation of the current unit.
* **Conditional dependencies**: This is used to evaluate a truthful or false value of units with it. The current unit won’t be activated if the conditional directive evaluates to false.
* **BindsTo**: This directive is similar to Requires=, but also causes the current unit to stop when the associated unit terminates.
* **Assert**: Similar to the directives that start with Condition, these directives check for different aspects of the running environment to decide whether the unit should activate. However, unlike the Condition directives, a negative result causes a failure with this directive.

## Run levels

* **poweroff.target** maps to runlevel 0 - This target is used for system shutdown.
* **rescue.target** maps to runlevel 1 - This target is used to enter single-user mode.
* **multi-user.target** maps to runlevels 2-4 This target is used to enter the standard multi-user mode without a graphical interface.
* **graphical.target** maps to runlevel 5 - This target is used for multi-user mode with networking and a display manager.
* **reboot.target** maps to runlevel 6 - This target is used to reboot the system.

## Refrences

* [Systemd Deep-Dive](https://youtu.be/Kzpm-rGAXos)(video)
* [systemd deep dive](https://hackmd.io/@mnet-kbb/systemd-deep-dive)(document)
