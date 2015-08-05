System-Snapshot
===============

# Description

The system-snapshot utility creates a point in time recovery snapshot of a system using stantard tools included in Red Hat Enterprise Linux core packages.

It is built to be used by system administrators before making changes to the system such as operating system update or a major change in an application. Currently it is used in large deploymets (>5000 machines).

The point in time recovery snapshot is only performed after all the checks are OK, and it includes a LVM snapshot of the configured volumes, and also a "/boot" backup so the whole rollback is fully consistent.

System-snapshot performs the following tasks:
* Create: Creates a recovery point for "/boot" and the selected LVM devices
* Discard: Removes the "/boot" backup and all the LVM snapshots created
* Rollback: Restores "/boot" partition and performs a rollback of the LVM snapshot to the previous status. It requires a reboot to finish the task.

# Configuration

The main configuration file is located in "/etc/sysconfig/system-snapshot". It can easily be distributed by a config management system such as puppet.

    # System-snapshot config file
    # Include the devices to be snapshotted separated by spaces
    BACKUP_DEVICES="/dev/vg00/lv_root /dev/vg00/lv_usr /dev/vg00/lv_home /dev/vg00/lv_var"
    # Set the directory to host the snapshot and boot backup information 
    BACKUP_DIR="/var/system-snapshot" 
    # Define percentage of the size to be allocated for the snapshot
    SNAPSHOT_SIZE="20"

#Notes

This tool does not work in RHEL5 as the kernel cannot perform the rollback. It should work in RHEL7 but has not been tested.

It is recommended not to include "/var/log" partition in the list of volumes to make a snapshot as it can help debug the upgrade procedure after a rollback

In the git repo you can also find the RPMS for Red Hat Enterprise Linux 6 already built (RHEL6), as well as the SRPMS to rebuld it for your RHEL version.


