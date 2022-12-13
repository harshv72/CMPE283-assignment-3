# CMPE283-assignment-3 - Harsh Vaghasiya (016053102) , Rushabh Sheta (016038646)

## Team Members:
1) Harsh Vaghasiya - 016053102
2) Rushbha Sheta - 016038646

## Questions and Answers:
----------------------
1) **For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched. (You may skip this question if you are doing the lab by yourself).**<br><br>
**Answer:** <br><br>
We decided to work together throghout the implementation of project in GCP.

- **Rushabh Sheta:** Built the kernel, understood steps to be performed, Researched about cpuid instruction and cpu leaf node,forked the torvalds/linux repo, understood where to do necessary chnages in files, create vmx.c file, create the test file and update the documentation.<br><br>


- **Harsh Vaghasiya:** Built the kernel, Installed required kernel modules in the VM,learnt about atomic variables, Included the discussed changes in the cpuid.c file, compile the test file and create the documentation.<br><br>


----------------------
2) **Describe in detail the steps you used to complete the assignment. Consider your reader to be someone skilled in software development but otherwise unfamiliar with the assignment. Good answers to this question will be recipes that someone can follow to reproduce your development steps.**<br>
**Note:** I may decide to follow these instructions for random assignments, so you should make sure they are accurate. <br><br>
**Answer:**<br><br>
**IMPORTANT NOTE: We have not included all screenshots in this README file. Please, check out [CMPE283-assignment-3 Screenshots](Screenshots/) for more detailed understanding of flow and procedure)**<br><br>
**Step-1: Created project in GCP.** <br><br>

**Step-2: Enabled Compute Engine API in order to create VM instance. Opened Google Cloud Shell and created VM instance with enabled nested virtualization in this project.** <br><br>

![5 copy](https://user-images.githubusercontent.com/52853300/207207540-f51ea31d-6ccf-4ab3-8c9e-2deb7ac432bb.png)

**To create VM instance with enabled nested virtualization, use following command:**<br>
**Note:** DON'T FORGET to make changes for project specific details in the following commmand such as project name, project id, zone details etc.<br>
>*gcloud beta compute instances create instance-1 --project=cmpe283a3 --zone=us-central1-a --machine-type=n2-standard-8 --network-interface=network-tier=PREMIUM,subnet=default --maintenance-policy=MIGRATE --provisioning-model=STANDARD --service-account=368584009094-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/cloud-platform --min-cpu-platform=Intel\ Cascade\ Lake --enable-display-device --create-disk=auto-delete=yes,boot=yes,device-name=instance-1,image=projects/ubuntu-os-cloud/global/images/ubuntu-2204-jammy-v20221201,mode=rw,size=100,type=projects/cmpe283a3/zones/us-central1-a/diskTypes/pd-ssd --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --reservation-affinity=any --enable-nested-virtualization --threads-per-core=2 --visible-core-count=4*

![2](https://user-images.githubusercontent.com/52853300/207207690-5f30d522-dc19-496a-82cc-aa9d9d56ec4b.PNG)

**Step-3: Logged into the VM instance using SSH.** <br><br>

![8 copy](https://user-images.githubusercontent.com/52853300/207207918-0bdfbe82-ff76-4655-a74f-f8cca856c092.png)

**Step-4: Install Git. And Clone the kernel repository. Verify the contents of Linux Folder.** <br><br>
>*sudo apt-get install git*<br>
>*git --version*<br>
>*git clone https://github.com/Rushabh2711/linux.git*<br>
>*cd linux*<br>
>*ls*<br>
>*git status*<br>
>*git remote -v*<br>

**Step-5: Run following commands to install dependencies.** <br><br>
>*sudo bash*<br>
>*apt-get update*<br>
>*apt install gcc make*<br>
>*apt-get install libncurses-dev gawk flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf*<br>

![5](https://user-images.githubusercontent.com/52853300/207208065-ca05b763-77c5-45f3-8c1d-3042e0c40af2.PNG)

**Step-6: Run "uname -a" command to check the version of current kernel. Copy the config file. Run "make oldconfig" to read the existing .config file that was used for an old kernel. Edit .config file and make chages as shown in the screenshots below.**<br><br>
>*uname -a*<br>
>*cp -v /boot/config-$(uname -r) .config*<br>

![7](https://user-images.githubusercontent.com/52853300/207208165-f6c3d531-d92f-4e00-8a63-be4a83e353a1.PNG)

>*make oldconifg*<br>

- Changes in .config file

![18 copy](https://user-images.githubusercontent.com/52853300/207208726-dd161088-19fe-43d7-adf4-b3aa990b4503.png)

**Step-7: Run following command to build the kernel for the first time. Run "sudo reboot" command to reboot VM instance. Run "uname -a" command again to check the version of updated kernel.**<br><br>
>*make -j 8 modules && make -j 8 && sudo make modules_install && sudo make install*<br>

![8](https://user-images.githubusercontent.com/52853300/207208773-f59b930a-6409-4f9b-b6a1-6e7bbf2516d0.PNG)

>*sudo reboot*<br>
>*uname -a*<br>

**Step-8: cpuid.c file is located at : "/linux/arch/x86/kvm/" folder. And vmx.c file is located at:  "/linux/arch/x86/kvm/vmx/" folder. Make changes to these files as shown below""**<br><br>
- Changes for cpuid.c file:

![9 copy](https://user-images.githubusercontent.com/52853300/207209312-47336457-7e55-4f80-87ee-b6891105db0d.PNG)

![10 copy](https://user-images.githubusercontent.com/52853300/207209507-6d39216e-dab5-47ea-ad67-f622f221f040.PNG)

![11 copy](https://user-images.githubusercontent.com/52853300/207209521-810c929b-f8cf-4ee4-b907-f6e853d1def9.PNG)

- Changes for vmx.c file:

![12 copy](https://user-images.githubusercontent.com/52853300/207209795-8e6a7542-9028-43f5-9e41-7323f02b48fb.PNG)

![13 copy](https://user-images.githubusercontent.com/52853300/207209818-e996698e-eeeb-43aa-8694-9876093ed04d.PNG)

**Step-8: Run following command to build the kernel again for the second time.**<br><br>
>*make -j 8 modules && make -j 8 && sudo make modules_install && sudo make install*<br>

![14](https://user-images.githubusercontent.com/52853300/207209845-c2940dbe-b454-43d3-b186-89a5c8935a17.PNG)
----------------------
## Testing:

**Step-9: To install some basic GUI functionailities on our VM for inner-vm interaction, run following commands:**<br><br>
>*sudo apt update*<br>
>*sudo apt install*<br>
>*sudo apt update*<br>
>*sudo apt install --assume-yes wget tasksel*<br>
>*wget https://dl.google.com/linux/direct/chrome-remote-desktop_current_amd64.deb*<br>
>*sudo apt-get install --assume-yes ./chrome-remote-desktop_current_amd64.deb*<br>

![16](https://user-images.githubusercontent.com/52853300/207210105-a7bb9b41-65eb-42b5-b74d-3e1a790977fc.PNG)

>*sudo tasksel install ubuntu-desktop*<br>
>*sudo bash -c ‘echo “exec /etc/X11/Xsession /usr/bin/gnome-session” > /etc/chrome-remote-desktop-session’*<br>
>*sudo apt-get install xbase-clients*<br>

**Step-10: Run  following commands.**<br><br>
>*sudo systemctl status chrome-remote-desktop@$USER*<br>
>*sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils*<br>

![18](https://user-images.githubusercontent.com/52853300/207210183-2c5ccd0b-abdb-4605-9e40-8d5e53e52ec0.PNG)

>*sudo lsmod | grep kvm*<br>
>*sudo rmmod kvm_intel*<br>
>*sudo rmmod kvm*<br>
>*sudo modprobe kvm*<br>
>*sudo modprobe kvm_intel*<br>
>*sudo lsmod | grep kvm*<br>

![19](https://user-images.githubusercontent.com/52853300/207210223-3156c2a2-32da-41f4-8193-94ca200bec9e.PNG)

>*sudo adduser 'rushabh_sheta' libvirt*<br>
>*sudo getent group | grep libvirt*<br>
>*sudo adduser 'rushabh_sheta' kvm*<br>
>*sudo getent group | grep kvm*<br>

![20](https://user-images.githubusercontent.com/52853300/207210283-e766275a-1260-4fd4-ab5c-a672d1d217e0.PNG)

>*sudo virsh list --all*<br>
>*sudo systemctl status libvirtd*<br>
>*sudo systemctl enable --now libvirtd*<br>

**Step-11: Run code for Debian Linux Computer Setup. Install virt manager. Download and move .iso file to install ubuntu.**<br><br>
>*DISPLAY= /opt/google/chrome-remote-desktop/start-host --code="4/0AfgeXvvXzQRHUn9v7KUz4mxbZCve77kb55VNr3dq1yuMYOpnY-R83XLZusZGxeFclospJw" --redirect-url="https://remotedesktop.google.com/_/oauthredirect" --name=$(hostname)*<br>
>*sudo apt install virt-manager*<br>
>*wget https://releases.ubuntu.com/jammy/ubuntu-22.04.1-desktop-amd64.iso*<br>
>*sudo  mv ~/ubuntu-22.04.1-desktop-amd64.iso /var/lib/libvirt/images/*<br>

**Step-12: Install Ubuntu in remote desktop.**<br><br>

![23](https://user-images.githubusercontent.com/52853300/207210471-1b467db7-1fc7-45d2-aea2-4aa9663387d7.PNG)

**Step-13: Update package installer. Install build-essential and cpuid package.**<br><br>
>*sudo apt-get update*<br>
>*sudo apt-get install*<br>
>*sudo apt-get install build-essential*<br>
>*sudo apt-get install -y cpuid*<br>
----------------------
## Output
**Step-14: Run following commands.**<br><br>

>*cpuid -l 0x4ffffffe*<br>
>*cpuid -l 0x4fffffff*<br>

![26](https://user-images.githubusercontent.com/52853300/207210672-10da1e00-cd86-467e-a169-77bf2cf0c743.PNG)

>*sudo dmesg | tail*<br>

![27](https://user-images.githubusercontent.com/52853300/207210805-bd27b11c-7575-4e42-b0d0-6e9dec1a9c59.PNG)

![28](https://user-images.githubusercontent.com/52853300/207210830-cfb4876b-70f5-4c9f-acde-4d5f198f86ef.PNG)

![29](https://user-images.githubusercontent.com/52853300/207210855-06b57018-0715-4362-b2d9-9b1908fe579b.PNG)

![30](https://user-images.githubusercontent.com/52853300/207210870-9c702311-ac7d-40e2-8d81-7d6375edbcad.PNG)

![31](https://user-images.githubusercontent.com/52853300/207210886-1de28693-4fe7-4313-b3cf-8adff87c22e5.PNG)

----------------------
3) **Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there 
more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?**<br><br>
**Answer:** <br>The frequency of exits kept increasing  at a stable rate but it was not necessarily in linear fashion. We observed significant increase in exit numbers after rebooting the inner VM. There are various VM instructions/operations that induce exits, such as EPT violation, RDRAND, I/O instruction, RDTSCP, and so on. After a full VM boot, approximately 11727090 exits occurred.<br>

----------------------
4) **Of the exit types defined in the SDM, which are the most frequent? Least?**<br><br>
**Answer:** <br>The Most frequent exits are:

<img width="391" alt="image" src="https://user-images.githubusercontent.com/52853300/207227279-713c534b-ec8e-4bac-be35-e8cdb7298cd8.png">

The Least frequent exit types are:

<img width="395" alt="image" src="https://user-images.githubusercontent.com/52853300/207227402-01a6c5ac-cdd5-4c6a-86c0-3e532fb0af6f.png">
<br>


