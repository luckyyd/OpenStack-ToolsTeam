Here are some quick tips on creating an OpenStack Glance image based on Windows 2008 R2.  There is a collection of good in-depth how-to’s on the internet, therefore will not be duplicating it here. Also make sure to download the virtio windows driver ISO here.

Create a raw image to install OS into:

    # kvm-img create -f raw windows2008r2.img 15G
    
Install the OS (note, this is recommended to be done inside a GNOME desktop where KVM is installed:

    # kvm -m 2048 -cdrom win2008r2.iso -drive file=windows2008r2.img,if=virtio -drive file=virtio-win-0.1-52.iso,media=cdrom,boot=off -net nic,model=virtio -net user

- See more at: https://www.vvlsystems.com/how-to-create-an-openstack-windows-2008-r2-image/#sthash.KugTzYDH.dpuf

Virtio driver:
    https://fedoraproject.org/wiki/Windows_Virtio_Drivers#Direct_download

Direct downloads are available for the .iso, .vfd, and qemu-ga installers.

• Stable virtio-win iso: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso

• Stable virtio-win x86 floppy: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win_x86.vfd
    
• Stable virtio-win amd64 floppy: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win_amd64.vfd
    
• Latest virtio-win iso: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/virtio-win.iso
    
• Latest virtio-win x86 floppy: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/virtio-win_x86.vfd
    
• Latest virtio-win amd64 floppy: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/virtio-win_amd64.vfd
    
• Latest qemu-ga files: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-qemu-ga/
    
• Full archive: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/
    
• Changelog: https://fedorapeople.org/groups/virt/virtio-win/CHANGELOG
