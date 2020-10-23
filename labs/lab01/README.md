## Physical Volumes

- Check the disks at the machine: `fdisk -l`.

- Create a physical volume (init/add a disk to the machine): `pcvreate <path_to_disk>`.

- Check the existent physical volumes: `pvdisplay`.
  
  **NOTE:** With this command, you can see the detailed information of each physical volume, including the volume group it belongs, if belongs to any.

## Volume Groups

- Create a volume group: `vgcreate <group_name> <path_to_disk> /dev/sdb`.

- Check the existent volume groups: `vgdisplay`.

## Logical Volumes

- Create a logical volume in a group volume: `lvcreate --size <size> --name <lv_name> <path_to_volume_group>`.

- Check existent logical volumes: `lvdisplay`.
  
  And also, check the logical volumes of some group volume: `ls -la <path_to_volume_group>`.

### Filesystems

- Create a filesystem for a LVM: `mkfs.xfs <path_to_logical_volume>`.

- Create a mount point for the filesystem: `mkdir /mnt/<filesystem_name>`.

- Mount the partition: `mount <path_to_logical_volume> <path_to_mount_point>`.

- Check the existent filesystems: `df -h`.

### Resizing LVMs

- Expand the storage size of a logical volume: `lvextend -L +<value_to_extend> <path_to_LV>` and use `lvdisplay` to check the new size.

- Resize the filesystem associated with that LVM: `xfs_growfs ./mnt/<filesystem_name>` and use `df -h` to check the new size.

### LVM Snapshots

- Create a snapshot of an LV: `lvcreate -s <path_to_LV> -L <size_of_LV_to_be_copied> -n <snapshot_name>` ans use `lvdisplay`to check it (also check if the snapshot has read and write permissions).

- Create a new moint point: `mkdir /mnt/<filesystem_name>`.

- Mount the partition: `mount -o nouuid <path_to_LV> <path_to_mount_point>`.**NOTE:** the `-o nouuid`option is because we are mounting the FS at the snapshot of the previous LV, this one already has a FS UUID. 

- Umount the FS: `umount /mnt/<filesystem_name>`.

- Generate new UUID for the snapshot volume: `xfs_admin -U generate <path_to_LV>`.

- Mount the volume again: `mount <path_to_LV> /mnt/<filesystem_name>` and check its content.

### LVM Migration

- Add another PV to VG: `vgextend <VG_name> <path_to_PV>` (use `pvdisplay` and `gvdisplay` to check it).

- Umount all the filesystems associated with the created LVMs.

- Migrate data from the old PV to the new one: `pmove <path_to_old_PV> <path_to_new_PV>`.

- Remove the old PV from the VG: `vgreduce <VG_name> <path_to_old_PV>`.

- Check that all LVs still exist, mount the filesystems again and check their content.


