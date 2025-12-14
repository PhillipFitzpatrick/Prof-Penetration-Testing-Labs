# Lab 1: Configuring our lab environment

Estimated Time: 120 Mins

The main assessment for this module is a large practical test during exam week. The assessment will be a simulated penetration test on a small company network, for both their wired and wireless networks. To complete the penetration test you'll need your own laptop, hardware, backups etc. and any tools or resources you might need for the day. Having a fully working attack machine will be essential. 

All of the lab activities and in-class demos will be completed using the setup outlined below, however you are free to use any setup and tools you prefer.

> :warning: While you are free to use your own setups, troubleshooting any issues on different setups outside of the suggested lab environment, won't be possible. 

___

## Recommended Lab Setup:

1. VirtualBox hypervisor 
2. Kali Linux VM  
-- recommended, but any modern linux VM should be fine
___

Download:
VirtualBox: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads) (Download the package for your own OS)  
Kali Linux image (latest version): [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/) (For most students, I'm recommending the pre-built virtual box image)  


## Setup Instructions 


1. Install VirtualBox - Run the downloaded installer and follow the on-screen instructions.
   
2. Extract the Kali VirtualBox image.
> ⚠️ The downloaded image file is a 7zip compressed archive, so you'll first need to extract the VirtualBox image before importing to Virtualbox.  
   
   -- On windows you can download the offical 7zip app to extract the image. [https://www.7-zip.org/](https://www.7-zip.org/)  
   -- On Linux you can type ```7z x filename.7z```  
   -- On Mac the default archive tool should be able to extract the image.  

4. Start VirtualBox and select ‘Add’. We then navigate to the location our VM is downloaded and find the .vbox file.

5. You can change any of the settings to suit your own system, and then start the VM.
   > 💡 Base Memory: Allocate a minimum of 2048 MB RAM (more for better performance).  
   > 💡 Processors: 1 CPU (more for better performance)

> :writing_hand: The default login is kali for the user and kali for the password!

6. Update your Kali to the latest version
   a. Open a terminal and run: ```sudo apt update && sudo apt full-upgrade -y```
   b. run: ```sudo apt --fix-broken install```
   c. run: ```sudo apt autoremove --purge -y```
   d. run: ```sudo apt clean```
   e. run: ```sudo reboot```

7. Shutdown / Power Off the Kali VM and **Take a Snapshot**
> ⚠️ Taking a snapshot of your Kali Linux virtual machine in VirtualBox is highly recommended because it saves the current state of the VM, allowing you to quickly roll back if something goes wrong after updates, tool installations, or configuration changes. This is especially useful for penetration testing and security research, where you often experiment with risky tools or scripts. Snapshots act like version control for your VM, saving time compared to reinstalling the OS or troubleshooting issues. It’s best to take snapshots right after installation, after major updates, and before making significant changes. To create one, open VirtualBox Manager, select your Kali VM, go to the Snapshots tab, click Take Snapshot, and give it a descriptive name like “Fresh Install” or “Post Update.” Restoring a snapshot takes seconds, making it an essential practice for safe experimentation and efficient recovery.
