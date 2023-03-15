## Setup VM

### VM install

```
qemu-img create -f qcow2 guest.qcow2 30G
wget https://releases.ubuntu.com/kinetic/ubuntu-22.10-live-server-amd64.iso ubuntu.iso
sudo ./launch-qemu.sh -hda guest.qcow2 -cdrom ubuntu-22.10-live-server-amd64.iso
# XXX: add 'console=tty0 console=ttyS0,115200n8' command line option in grub menu
```

### Build SVSM-compatible guest kernel
- copy config file from the vm

```
scp -P 6666 m@localhost:/boot/config-5.19.0-21-generic .
```

- then build guest kernel
```
./build.sh kernel guest ./config-5.19.0-21-generic
```

## Launch VM
- Normal VM
```
sudo ./launch-qemu.sh -hda guest.qcow2
```
- SVSM
```
sudo ./launch-qemu.sh -hda guest.qcow2 -kernel ./linux/guest/arch/x86_64/boot/bzImage -sev-snp -svsm ../svsm.bin
```

