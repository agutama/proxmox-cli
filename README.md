# proxmox-cli

## Basic

- Download ubuntu cloud image
```
wget -c https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img
```

- Install libguestfs-tools
```
sudo apt install libguestfs-tools -y
```

- Resize image disk size
```
qemu-img resize jammy-server-cloudimg-amd64.img 16G
```

- Customize image
```
sudo virt-customize -a jammy-server-cloudimg-amd64.img --install qemu-guest-agent --truncate /etc/machine-id
```

- Create vm
```
sudo qm create 1000 --name ubuntu22-cloud --core 1 --memory 1024 --net0 virtio,bridge=vmbr0
```

- Import and attach disk
```
sudo qm disk import 1000 jammy-server-cloudimg-amd64.img local
sudo qm set 1000 --scsihw virtio-scsi-pci --scsi0 local:1000/vm-1000-disk-0.raw
```

- Config option
```
sudo qm set 1000 --boot c --bootdisk scsi0
sudo qm set 1000 --agent 1
sudo qm set 1000 --serial0 socket
sudo qm set 1000 --vga serial0
sudo qm set 1000 --hotplug network,usb,disk
sudo qm set 1000 --ipconfig0 ip=dhcp
```

- Prepare cloudinit
```
sudo qm set 1000 --ide2 local:cloudinit
sudo qm set 1000 -ciuser aguswibawa
sudo qm set 1000 -cipassword P@ssw0rd
sudo qm set 1000 --sshkeys ~/.ssh/id_rsa.pub
sudo qm set 1000 --ciupgrade 0
```

- Create new template
```
sudo qm template 1000
```

- Clone new template to VM
```
sudo qm clone 1000 200 --name ubuntu22-demo --storage local -full
```

- Start VM
```
sudo qm start 200 
```

- Stop VM
```
sudo qm stop 200 
```

- Destroy VM
```
sudo qm destroy 200 
```