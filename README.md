kube-lvm-flexvolume
===================

Enhanced version of [LVM](https://github.com/kubernetes/kubernetes/blob/master/examples/volumes/flexvolume/lvm)
[flexVolume](http://kubernetes.io/docs/user-guide/volumes/#flexvolume) driver for Kubernetes.


Description
============

lvm-flexvolume provides a LVM driver with Thin Provisioning support. It will allocate LV
from a previously created VG/Thinpool and mount it for usage in a container.


Changes prior to Kubernetes version
===================================

*  can create LV if does not exist
*  support for [thin provisioned](http://man7.org/linux/man-pages/man7/lvmthin.7.html) logical volumes
*  support for optional [mount](http://man7.org/linux/man-pages/man8/mount.8.html) options


Installation
============

*  Make sure kubelet is running with `--enable-controller-attach-detach=false`
*  Create the directory `/usr/libexec/kubernetes/kubelet-plugins/volume/exec/sysoperator.pl~lvm`
*  Install driver as `/usr/libexec/kubernetes/kubelet-plugins/volume/exec/sysoperator.pl~lvm/lvm`

Create a Kubernetes Pod such as:

```
cat examples/nginx.yaml | kubectl apply -f -
```

The driver will create a logical volume and the volume will also be mounted as /data inside the container.


Options
=======

Following options are required:

*  volumeID - Name of logical volume.
*  size - Size to allocate for the new logical volume. Accepts any value supported by --size parameter of [lvcreate](http://man7.org/linux/man-pages/man8/lvcreate.8.html).
*  volumegroup - Name of volume group.

Optional options may be passed:

*  thinpool - Name of thin pool.
*  mountoptions - Additional options passed to mount. (e.g. noatime)

