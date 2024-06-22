# proxmox-cli

## Basic

- Download ubuntu cloud image
```
wget -c https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img
```

- Resize image disk size
```
qemu-img resize jammy-server-cloudimg-amd64.img 16G
```

- Install libguestfs-tools
```
sudo apt install libguestfs-tools -y
```

- Customize image
```
virt-customize -a jammy-server-cloudimg-amd64.img --install qemu-guest-agent --truncate /etc/machine-id

sudo qm set 1000 --scsihw virtio-scsi-pci --scsi0 local:vm-1000-disk-0
```

- Create vm
```
sudo qm create 1000 --name ubuntu22-cloud --core 1 --memory 1024 --net0 virtio,bridge=vmbr0
```

- Import disk
```
 sudo qm disk import 1000 jammy-server-cloudimg-amd64.img local
 ```

- Attach disk and config option
```
sudo qm set 1000 --scsihw virtio-scsi-pci --scsi0 local:vm-1000-disk-0
sudo qm set 1000 --boot c --bootdisk scsi0
sudo qm set 1000 --agent 1
sudo qm set 1000 --serial0 socket
sudo qm set 1000 --vga serial0
sudo qm set 1000 --hotplug network,usb,disk
```




