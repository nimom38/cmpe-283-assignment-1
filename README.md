# cmpe-283-assignment-1

## Outputs

![Alt text](<Screenshot 2023-12-17 at 12.40.21 PM.png>)

![Alt text](<Screenshot 2023-12-17 at 12.40.39 PM.png>)

![Alt text](<Screenshot 2023-12-17 at 12.40.52 PM.png>)

## Source codes-

Look at the cmpe283 folder of this forked repo- https://github.com/nimom38/linux/tree/master

## Q1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched.

- ## Mohaimin Iqbal Gazi(017454489)

 
  - Setup virtual machine on Compute Engine of GCP
  - Installed the linux packages and dependencies
  - Researched the part of the codebase that was gone through by the Professor, to get a feel of   how to add our own logic.
  - Implemented the logic that Hemang figured out by doing his research on the codebase and Intel SDM Manual
  - Wrote the Readme doc

- ## Hemang Huria(016123146)
  - Figured out that the memory of virtual instance should be big enough (200gb)
  - Researched the definitions for VMX capabilities for VMEXIT and Tertiary Procbased controls from the Intel SDM Manual
  - Researched the definitions for VMX capabilities for VMENTRY, Primary and Secondary Procbased controls from the Intel SDM Manual
  - Figured out that the “dmseg” was outputting less lines than expected and thus ultimately found a bug in the code. (see commits)
  - Improved the doc and suggested we cut the clutter by removing redundant screenshots because all the great open-source tool documentations rightfully do not provide screenshots when demonstrating step by step instructions.


## Q2. Describe in detail the steps you used to complete the assignment.

- Set Up GCP

  - Create GCP account
  - Enable Compute Engine API
  - Create Virtual Machine using the following command on the GCP terminal -

    <b>gcloud compute instances create cmpe-283-vm3 --enable-nested-virtualization --zone=us-west1-b --machine-type=n2-standard-8 --network-interface=network-tier=PREMIUM,subnet=default --create-disk=auto-delete=yes,boot=yes,device-name=instance-1,image=projects/ubuntu-os-cloud/global/images/ubuntu-2004-focal-v20220204,mode=rw,size=200,type=projects/sjsu-spring-2022/zones/us-central1-a/diskTypes/pd-ssd --metadata=ssh-keys=<YOUR_SSH_KEYS></b>

  - SSH into the newly created virtual machine instance using the browser ssh window option

- Install the necessary packages

  - sudo apt install make
  - sudo apt-get update
  - sudo apt-get install gcc
  - sudo apt-get install libncurses-dev
  - sudo apt-get install flex
  - sudo apt-get install bison
  - sudo apt-get install libssl-dev
  - sudo apt install libelf-dev
  - sudo apt install dwarves
  - sudo apt-get install libssl-dev
  - sudo apt-get install libelf-dev

- Fork the repo - https://github.com/torvalds/linux
- Example Forked repo - https://github.com/nimom38/linux/tree/master
- Clone the forked repo to the virtual machine using command-

  git clone git@github.com:nimom38/linux.git

- Upload the two files- "Makefile" and "cmpe283-1.c"(with your own logic embedded in it) to the root directory of the virtual instance

- Now run these commands - (Most of these commands are from Professor Mike Larkin's lecture. However, we ran into few issues which we solved by using few commands from stackoverflow, google searches, chatgpt etc.)
  - cat Makefile
  - make
  - **Get an error**
  - cd linux
  - uname -a
  - make menuconfig
  - cp /boot/config-5.11.0-1029-gcp .config
  - make oldconfig
  - make prepare
  - cd ..
  - make
  - make clean
  - cd linux
  - make -j 8 modules
  - **Get some errors**
  - scripts/config --disable SYSTEM_TRUSTED_KEYS
  - scripts/config --disable SYSTEM_REVOCATION_KEYS
  - make -j 8 modules
  - sudo make INSTALL_MOD_SCRIPT=1 modules_install
  - sudo make install
  - uname -a
  - sudo reboot
  - uname -a
  - make
  - ls \*.ko
  - sudo insmod cmpe283-1.ko
  - lsmod | grep cmpe283
  - dmesg
  - sudo rmmod cmpe283-1
  - dmesg
