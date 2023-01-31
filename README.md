# Alpine for Nephele
The root filesystem in `alpine-v3.13-rootfs.tar.gz` can be deployed to run the
Xen hypervisor and Dom0 with Nephele support as it is.  However, the tools
needed for Nephele are not included and they have to be built. The root
filesystem contains all the packages needed for building the Nephele
components. For more information check the experiments details in
[here](https://github.com/nephele-vm/experiments).

Before deploying the filesystem to a machine do not forget to add your public
key in `/root/.ssh/authorized_keys`.

Command line examples for Xen:

* Using NFS for Dom0:

```
/alpine-v3.13-rootfs/boot/xen-4.16-unstable.gz dom0_mem=2048M,max:2048M dom0_max_vcpus=2 dom0_vcpus_pin intel_iommu=on iommu=on,verbose com1=57600,8n1 console=com1 console_timestamps=boot --- /alpine-v3.13-rootfs/boot/vmlinuz-5.9.0-rc8+ ip=::::::dhcp netboot=nfs nfsroot=<NFS-IP>:/alpine-v3.13-rootfs,nfsvers=3 rw console=hvc0 xenstored=lixs
```

* Using ramdisk for Dom0 (please make sure enough RAM is allocated for Dom0):

```
/alpine-v3.13-rootfs/boot/xen-4.16-unstable.gz dom0_mem=4096M,max:4096M dom0_max_vcpus=2 dom0_vcpus_pin intel_iommu=on iommu=on,verbose com1=57600,8n1 console=com1 console_timestamps=boot --- /alpine-v3.13-rootfs/boot/vmlinuz-5.9.0-rc8+ placeholder ip=::::::dhcp root=/dev/ram0 console=hvc0 --- /alpine-v3.13-rootfs/boot/dom0.cpio
```
