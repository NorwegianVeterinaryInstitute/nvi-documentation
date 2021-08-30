# Virtualbox - biolinux tricks

## Sharing data via a shared folder
We have been working for some time with the biolinux virtualmachine on our Windows laptops. 
One thing that makes life a lot easier is when we can share files between the Windows host and the linux guest system.

We have been working for some time with the biolinux virtualmachine on our Windows laptops. One thing that makes life a lot easier is when we can share files between the Windows host and the linux guest system.

Below you find instructions on how to set-up a shared folder that can be used to transfer files between the host and the guest system and vice versa. The first part will be done on your windows host and the second part is inside the virtualmachine.

### Setting up the host
* Create a folder on your Windows host that you want to use for sharing files. I call it `VM_shared_folder` and I have it in the directory: `temp` at the root of the `C: drive`.
* Now allow the folder to be shared. I use a right mouse click and set this folder to be shared.
* Start virtualbox and select the virtualmachine for which you want to set-up a shared folder.
* Open `Settings` and go to the box `Shared Folders`.
* On the right you see a little folder sign with a green plussign on it, click that.
* Add your folder: `VM_shared_folder`, to the Folder Pathbox.
* Click the `Auto-mount` and Make Permanent boxes.
* Close the window by clicking `OK`. Your shared folder is now set-up on the host.
