# Determine and Configure Hardware Settings

## Pseudo file systems

Everything in linux is represented as a file: the pseudo file systems are a representation of current status of the running system in RAM, they are created on system start and removed on stop as the RAM does not persist the information.

* [/proc](to-do) - info about all processes running on the system, processes can be find by folders PID, and you can see things like cmdline. Other things are cpuinfo, meminfo, partitions, uptime, version.
* [/sys](to-do) - info about the running system like kernel information, modules running, device information, filesystems in use.
* [/dev](to-do) - info about all devices connected to the system. It contains information about CPU, memory, networking, hard drive, and video card.

## Kernel

The Kernel is the core framework of the operating system, provides operability with hardware, memory, networking and itself.

It handles all memory management and hardware device interaction, extra functionallity can be added/removed by kernel modules.

#### Kernel Commands

* ```uname``` - *"unix name"*, displays information about the current kernel. 
  * ``-m`` -> Architecture (for instance: arm)
  * ``-r`` -> Release information (for instance: 5.14.0-534.21.1.el7.x86_64)
  * ``-a`` -> All information about the kernel information
  * **THEY CAN BE COMBINED**

### Modules

The kernel modules are additional funtionallity that can be load and unload dynamically to the kernel without reboot. For instance, Device drivers use to be third-party kernel modules.

Modules can be found in ``/lib/modules/$(uname -r)`` as ".ko" (kernel object) files.

Permanent manual loads must be done by modifing the follwing files:

* ``/etc/modules-load.d/<module_name>.conf`` must include the name of the module.
* ``/etc/modprobe.d/<module_name>.conf`` must include the list of parameters (each line as parameter_name=value).

Aliases can be done in ``/etc/modprobe.d/<module_alias>.conf`` by adding the following line ``alias <module_alias> <module_name>``

Blacklisting modules to prevent them from been loaded can be done in ``/etc/modprobe.d/blacklist.conf`` by adding the following line ``blacklist <module_name> /bin/false``.

#### Kernel Module Commands

* ```lsmod``` - *"list modules"*, displays a list of loaded modules: *Module Name, Size and Used by*.
* ```modinfo``` - *"module information"*, displays information about an specific module.
  * ``modinfo <module_name>`` -> module_name module information.
* ```modprobe``` - *"module probe?"*, Add and remove modules from the linux kernel, an also modify parameters of the loaded modules.
  * ``-r <module_name>`` -> Remove (or Unload) module module_name.
  * ``<module_name>`` -> Add (or Load) module module_name.

## Hardware

Hardware information is represented in the pseudo file system ``/dev``, all devices are managed by the ``udev`` service which detects the information of the device and add them into ``/dev`` and uses the **D-Bus** (**Device Bus?**) to notify users and system changes on the system hardware.

### Hardware Commands

* ``lspci`` - *"list pci"*, displays a list of PCI devices attached.
  * ``-k`` -> components of the hardware used by the kernel and modules.
  * ``-v`` -> verbose, all the possible information 
* ``lsusb`` - *"list usb"*, displays a list of USB devices attached.
  * ``-k`` -> further information.
  * ``-t`` -> more information of the devies connected to a particular port.
* ``lscpu`` - *"list cpu"*, displays all information a process can need about the CPU.
* ``lsblk`` - *"list block"*, display in *TREE* view all the block device attached to the system, including their partitions: *NAME, SIZES, MOUNTPOINTs*
  * ``-f`` -> shows the FSTYPE, LABEL, UUID, MOUNTPOINT