# tk1-torch-install
Up to date installation procedure for Torch7 with CUDA 7 on TK1. 

This is based on the excellent tutorial: [torch7 scientific computer framework with cudnn nvidia jetson tk1](http://jetsonhacks.com/2015/05/20/torch-7-scientific-computer-framework-with-cudnn-nvidia-jetson-tk1/) which is unfortunately out of date as the 'ezinstall' file they recommend is deprecated in addition to requiring the installation of cudnn from the R2 branch of soumith's [cudnn.torch repo](https://github.com/soumith/cudnn.torch) rather than using the luarocks 

## Install the latest JetPack
As of time of writing (Early June 2016) the latest JetPack for L4T (Linux for Tegra) is 2.1. The version of L4T in this version of JetPack is 21.4 for the TK1. The same JetPack installer contains L4T 23.2, and associated software, for the TX1. You can download the JetPack from

https://developer.nvidia.com/embedded/jetpack-archive
  
choose JetPack 2.1 to successfully follow along with this tutorial

Please note that this should be downloaded to a host Linux PC and **not** your TK1. 

The installer will give you instructions at the relevant point, but you will need to have the TK1 plugged in using the min USB cable and you will need to start the TK1 in 'force recovery' mode. 

This installation could take an hour or more depending on how much resources your host machine/VM has available to it.

### Mac OSX
If you are running Mac OS X you can create a Linux Virtual Machine, but you will need to install the VirtualBox extension to allow use of the USB ports in the virtual machine.

http://download.virtualbox.org/virtualbox/5.0.20/Oracle_VM_VirtualBox_Extension_Pack-5.0.20-106931.vbox-extpack
  
**Note** : 
You will need to allow the Linux virutalbox access to you macbook USB ports. I found that it was not possible to do this before botting up the Linux VM and that I needed to add a USB filter once the machine was running as I had no idea how to create a filter. Once you have added the port filter you will need to remove it if you wish to run the Linux VM after you have flashed the TK1 or otherwise the default filter you added will grab all USB ports, including Bluetooth, Web camera etc. which will mean you cannot use external keyboards or trackpads.

## Torch7 installation
The old tutorial recommends using an 'ezinstall' script, which is now deprecated. Instead follow the steps recommended on the Torch website:

  [Torch - Getting started](http://torch.ch/docs/getting-started.html#_)

    # in a terminal, run the commands WITHOUT sudo
    git clone https://github.com/torch/distro.git ~/torch --recursive
    cd ~/torch; bash install-deps;
    ./install.sh
  
You may need to install git

    sudo apt-get install git
    
You can then install some libraries required for deep-learning

    sudo luarocks install cutorch
    sudo luarocks install cunn
  
### cuDNN for CUDA 6.5
The original tutorial recommends installing cuDNN torch bindings using luarocks, however this will install a version of cuDNN that requires CUDA 7.5 as a dependency. We will need to fetch and build the R2 branch from the cudnn.torch bindings repo

  [cudnn.torch repo](https://github.com/soumith/cudnn.torch) 
  
We will use luarocks to build and install cudnn.torch

    cd ~
    git clone https://github.com/soumith/cudnn.torch.git -b R2
    cd cudnn.torch
    luarocks make
    # you're done - no need to follow up with 'sudo luarocks install cudnn'

This will both build and install your cudnn.torch bindings, there is no need to try and follow the original tutorial as that will delete the cudnn binding you just installed and try to install CUDA 7.5 based binfdings - which will fail. I know this from experience - I am not a clever man :)
  
**Note**: if you are having trouble with luarocks installation you may have installed luarocks before installing torch (assuming you didn't start installing immediately after flashing your TK1). In this case you should urun your luarocks commands from the ~/torch folder and even use the full path to the luarocks executable ~/torch/install/bin/luarocks

## Does it work?
Well, you should now be able to run torch and require cudnn

    th
    require 'cudnn';
    # no errors!



