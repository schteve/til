# Super slow on Windows 10

When spinning up a virtual machine using VirtualBox on Windows 10, you may find that the VM runs super slow for no apparent reason. Of course you already checked things like CPU / RAM / disk allocation.

This may be because Hyper-V is enabled. Apparently, this causes problems because Hyper-V essentially turns your normal operating system into a virtual machine. Since the virtualization hardware in your CPU (Intel VT-x / AMD-V) can only be used by the top-level host, and your main OS is using it for Hyper-V, VirtualBox can't make use of the hardware assistance and has to fall back to emulation, which is of course significantly slower.

The solution then is to disable Hyper-V. Run this from an admin command line:
> `bcdedit /set hypervisorlaunchtype off`

Note that this may prevent you from running Docker containers as they rely on Hyper-V being enabled.

# References
- https://superuser.com/questions/1006788/virtualbox-is-very-slow-in-windows-10
- https://superuser.com/questions/1208850/why-cant-virtualbox-or-vmware-run-with-hyper-v-enabled-on-windows-10
- https://learn.microsoft.com/en-us/troubleshoot/windows-client/application-management/virtualization-apps-not-work-with-hyper-v
