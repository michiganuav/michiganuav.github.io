---
title: Using kvm, qemu, and virsh for a virtual Ubuntu install
tags: Ubuntu virtualization virsh libvirt qemu Gentoo
article_header:
  type: cover
  image: 
    src: /assets/images/2015/11/background_v2-16.png
license: false
---

This is just a brief description of how to get a virtual machine up
and running that focuses on command line and console use.  The
instructions are written for [Gentoo](https://www.gentoo.org) and
assume a reasonable understanding of linux. They likely can be easily
adapted for other distributions.

# Install libvirt

As root:

```console
raptor ~ # sudo emerge -av libvirt
```

The above will install dependencies (including qemu) may require
adding some keywords.  Ensure you have appropriate qemu targets in
your /etc/portage/make.conf file.  A typical target is **x86_64**.

```console
QEMU_SOFTMMU_TARGETS="x86_64"
QEMU_USER_TARGETS="x86_64"
```

# Set permissions for libvirt

See this link about permissions and libvirt:
[https://wiki.gentoo.org/wiki/QEMU#Permissions](https://wiki.gentoo.org/wiki/QEMU#Permissions)

Note from those instructions that the following needed to be uncommented in the /etc/libvirt/libvirtd.conf file:

```console
auth_unix_ro = "none"
auth_unix_rw = "none"
unix_sock_group = "libvirt"
unix_sock_ro_perms = "0777"
unix_sock_rw_perms = "0770"
```

You should add your non-privileged user to the libvirt group (if it
exists... depends on policykit use flag) and the kvm group.

Also make sure you configure/compile appropriate kernel modules as described:
[https://wiki.gentoo.org/wiki/QEMU#Kernel](https://wiki.gentoo.org/wiki/QEMU#Kernel)


```console
raptor ~ # usermod -a -G libvirt exampleusername
raptor ~ # usermod -a -G kvm exampleusername
```

Adding yourself to a group typically requires you to re-login.

# Start the libvirt daemon

```console
raptor ~ # /etc/init.d/libvirtd restart
```

# Create a virtual hard drive image

```console
foo@bar:~$ qemu-img create -f qcow2 Ubuntu-Focal-Fossa.qcow 20G
```

# Download Ubuntu

```console
foo@bar:~$ wget https://releases.ubuntu.com/20.04/ubuntu-20.04.2-live-server-amd64.iso
```

# Install the image

```console
foo@bar:~$ virt-install --location ubuntu-20.04.2-live-server-amd64.iso --memory 4096 --vcpus 4 \
                        --disk Ubuntu-Focal-Fossa.qcow --nographics --extra-args='console=ttyS0'
```

Complete the installation then shutdown the machine.  **I found that I had to force the shutdown:**

```console
foo@bar:~$ virsh destroy ubuntu20.04 ## Force shutdown of running virtual machine
```

# Start the machine and connect to its console

Once installed, start and connect to the virtual machine console

```console
foo@bar:~$ virsh start ubuntu20.04 ## Start the virtual machine in the background
foo@bar:~$ virsh console ubuntu20.04 ## Connect to the virtual machine in a console mode
```

Note it reads the console is real time.  Hence, you may have to press return to get a login prompt (or similar).

# List virtual machines and their state

```console
foo@bar:~$ virsh list --all
```

# Force shutdown

To force a shutdown the machine (potentially with data loss if files have not yet been written)

```console
foo@bar:~$ virsh destroy ubuntu20.04 ## Force shutdown of running virtual machine
```

# Delete the virtual machine

The following command deletes the machine from virsh but doesn't delete the disk image

```console
foo@bar:~$ virsh undefine ubuntu20.04 ## Delete the virtual machine from virsh (doesn't delete the disk image)
```
