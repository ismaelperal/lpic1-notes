# Boot the system

## The Linux boot sequence

1. BIOS checks the hardware and I/O devices and launches the boot program.
2. The boot program, which is in the first hard drive has the address to the boot loader of the system.
3. The Linux Kerner is load, and the initial RAM disk would be loaded.
4. Then the initialization system is loaded.

### Commands

* ``dmesg`` - *"diagnostic messages"* - shows the kernel ring buffer, details the kernel can see.
* ``journalctl -k`` - *"journal control"* systemd new way to get the dmesg data. For instance, you can see the command line options used on the boot process.

## init

init *"initialization"* - it is a secuential services starter based on the Unix System V init. init is simple but slow. The boad loader launches it from ``/sbin/init``, and init loads its configuration ``/etc/inittab``. The configuration file specifies the run level that should be used. There are 7 run levels (0-6) which are used on different stages of the boot process. System only operates in one level at time

* 0 -> Halt / Stop / Turn off in use when the system starts
* 1 -> Single User Mode the root user is the only one, for maintenance task and repair functionallity
* 2 -> Multi User Mode (**without** Networking)
* 3 -> Multi User Mode (with Networking), servers use to run at this level
* 4 -> Reserved
* 5 -> Multi User Mode (with Networking and Graphical Desktop), workstations use to run at this level
* 6 -> Reboot

### ``/etc/inittab`` contents

```(bash)
<identifier>:<runlevel>:<action>:<process>
id:3:initdefault:
...
si::sysinit:/etc/rc.d/rc.sysinit
...
10:0:wait:/etc/rc.d/rc 0
```

Init scripts in **Debian** based are in ``/etc/initd`` in **RedHat** based in ``/etc/rc.d`` (*rc = run commands*)

* ``rc3.d/`` folder with scripts for run level 3, their execution depends on the filename alphabetical order.
* ``rc.sysinit`` house cleaning before levels
* ``rc.local`` after a run level has been loaded, tipically for custom processes to be loaded.

## upstart

Upstart is a asyncronous services starter develop for Ubuntu, ported to RedHat/Debian based systems. Faster than init. Real time events, can monitor the services that are woring with it.

``/sbin/init`` fires the ``startup`` event, then The start up event will check and mount the drives that make up the file system using the checks the mounted drives with ``mountall``, additionally it loads the ``/etc/init/rc-sysinit.conf``.

The ``telinit`` command is called switching the default run level by ``runlevel`` command. The runlevel program uses the ``/etc/init/rc.conf`` it will load sequentially.

## systemd

systemd is a services starter, uses C compiled code instead of shell scripts. It is compatible with older System V init scripts.

* ``/usr/lib/systemd/system`` -> provided package installations
* ``/etc/systemd/system`` -> Unit file location for administrators (takes precedence over ``/usr/lib/systemd``)
* ``/run/systemd/system`` -> Runtime unit files

### systemd commands

* ``systemctl list-unit-files`` list **ALL** unit files on the system
* ``systemclt cat <topic.unit>`` displais the file of some unit topic.

```(bash)
[Unit]
Description=Multi-User System
Documentation=man:systemd.special(7)
Requires=basic.target
Conflicts=rescue.service rescue.target
After=basic.target rescue.service rescue.target
```

``/sbin/init`` exists for compatibility reasons but it is a symbolic link to ``../lib/systemd/systemd``
