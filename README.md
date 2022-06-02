# qemu_kvm_windows_guest_nvidia

    ########################################################################################
    #                                                                                      #
    #      Setup a Windows guest with PCIe passthrough                                     #
    #                                                                                      #
    ########################################################################################
    
Download th virtio ISO : https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/  
Download the Windows ISO from microsoft.

Don't use version 0.1.217-x , it will fail to install. Version used is 0.1.215-2.  

During creation of the VM choose:  

>- Customize configuration before install  
>- Change Overview , Hypervisor details , Firmware --> OVMF_CODE.fd  
>- Change SATA Disk 1 to VirtIO Disk 1  
>- Load Windows ISO in SATA CDROM 1  
>- Add Hardware , Storage , CDROM device (SATA CDROM 2)  
>- Load VirtIO ISO in SATA CDROM 2  
>- Change NIC , Device model --> virtio  
>- Change Video QXL , model to virtio  

During the Windows installation the passthrough SCSI controller needs to be loaded from folder viostor\w10\amd64  

After the installation run virtio-win-guest-tools.exe, this will take care of installation and configuration of virtio components.  

The guest installation procedure is incomplete, manual actions are needed.  
Context : QXL DOD display driver and automatic resize of resolution.
Copy files vgpusrv.exe and viogpuap.exe in folder viogpudo\w10\amd64.  
I placed them in C:\Windows\System32.  
Open cmd.exe as Administrator and execute vgpusrv.exe -i

Change option windows passwordless via regedit:  

>  HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\PasswordLess\Device\DevicePasswordLessBuildVersion  2 --> 0  

Run netplwiz , remove checkbox "Users must enter a user name and password to use this computer", Apply (supply password twice).  

Prevent sending of collected data:  
Go to task scheduler (type "tasks" in search box), Microsoft , Windows , Application Experience , right click task "Microsoft Compatibility Appraiser" , disable.  

Disable adn stop services "Xbox Accessory Management Service" , "Xbox Live Auth Manager" , "Xbox Live Game Save" and "Xbox Live Networking Service".  

Remove apps :  
>- 
>- 

