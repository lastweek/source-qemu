# Source-QEMU


## KVM

Source code:

- `accel/kvm/kvm-all.c`

Links:

- https://lwn.net/Articles/658511/


## Device

I think the key to understand device emulation is first
to understand how kernel access devices.
The following are the things I know of:
1) Legacy x86 ports. Some char devices and other devices still use this port.
Writing to IO ports will cause vmexit, and KVM will report such events to QEMU (kvm-all.c)
2) PCIe devices. During boot, the kernel can query IO ports to get the list of available PCIe devices.
The kernel can also query the ACPI tables to get avaiable PCIe devices.
Once get the list, the kernel access PCIe device through Memory-Mapped IO, or MMIO.
So I guess this kind of MMIO memory access can be caught by KVM, right?

I'm also aware that virtio is like the "paravirtualization" technique for device drivers.
The device drivers in the guest are aware that they are in virtual machines, thus some operations
will directly call into the hooks exposed by hypervisor (KVM).

Either way, I want to understand things in this sequence:

1) In the early days when there is no hardware-virt support, how VMware emulate devices.
2) How Xen emulate devices.
3) How QEMU presents a whole machine machine model to the VM.
4) How QEMU catches all the MMIO accesses. Without VIRTIO, how it works.
5) With VIRTIO, how it works.

Links:

- https://developer.ibm.com/articles/l-virtio/
- https://www.redhat.com/en/blog/introduction-virtio-networking-and-vhost-net
