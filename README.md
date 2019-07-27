# armhf-ceph
Provisioning Ceph Cluster and Deploying on k3s

## Equipment:
- 4x Odroid XU4 SMB's
- 4x 64GB MMC

| Device | 64GB MMC Mount Location |
| --- | --- |
| odroid-01 | ready to provision |
| odroid-02 | ready to provision |
| odroid-03 | /media/mmc |
| odroid-04 | ready to provision |

## Process
1. Prepare the devices.
2. Mount storage, create partition, make filesystem, edit fstab.
3. Deploy k3s to storage cluster.
4. Use Helm to apply ceph-rook deployment to k3s.
5. Verify operation and launch dashboard.


## 0. Preparing the Devices
### A. Removing mounted NFS drives
1. Stop the NFS system service.
   ```bash
   sudo systemctl stop nfs-server
   ```
2. You can verify the NFS service is stopped.
   ```bash
   sudo system status nfs-server
   ```
3. Remove NFS share point from ```/etc/exports``` file.
4. Refresh the NFS exported file system list by running exportfs.
   ```bash
   sudo exportfs -a
   ```
5. Unmount the media. Example:
   ```bash
   sudo umount /media/odroid/sd
   ```
6. Remove drive mount point from ```/etc/fstab``` file.
7. Verify the drive is no longer mounted.
   ```bash
   sudo df
   ```
8. Check the above output and remove any other mounted NFS shares from hosts you will also be changing. Example:
   ```bash
   sudo umount /media/odroid-03/mmc
   ```
9. Repeat for the other devices in your Ceph Cluster.

* Removing NFS Drives Ansible Automation Notes:
  * check exports file then unmount selected drive.


## Research
* Manually deploy ceph on RPi: https://bryanapperson.com/blog/the-definitive-guide-ceph-cluster-on-raspberry-pi/
* Deploy ceph on k8s: https://github.com/danacr/kubernetes-the-fun-way/blob/master/docs/04-rook-ceph.md

---
---

# Ceph Cluster on Atomic Pi
