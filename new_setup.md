Linux Mint 22 setup Instructions as a Server running Docker Containers via DockSTARTer
----

Seting up server on fresh baremetal

0. Install Linux Mint (tested on 21 and 22 - 20250101)

0. Setup IP and Routing Table to 192.168.1.190 manually on your router to reserver 190 ip for the server

0. Install NoMachine for Remote Access

0. Install VSCode

0. Install Psensor

0. Automount disk and Folder Prep

   > Go to disks, choose the disk which will be mounting automatically. \
   Switch off User Defaults \
   Display Name: Data \
   Mount Point: /mnt/Disk Name \
   Identify as: /dev/disk-by-label/Disk Name

   Copy folders and data needed.

0. Install and Configure Samba

        sudo apt-get update; sudo apt-get install samba

   Copy Samba config file and edit folder path locations
        
        sudo cp ~/Desktop/smb.conf /etc/samba/smb.conf 
        sudo nano /etc/samba/smb.conf
        sudo service smbd restart

0. Install Docker

        # Install required packages
        sudo apt-get update
        sudo apt-get install ca-certificates curl gnupg

        # Add Docker's official GPG key
        sudo install -m 0755 -d /etc/apt/keyrings
        sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
        sudo chmod a+r /etc/apt/keyrings/docker.asc

        # Add the Docker repository with the correct Ubuntu code name
        echo \
        "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
        jammy stable" | \
        sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

        # Update the package index
        sudo apt-get update

        # Install Docker
        sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

    Separately Execute

        sudo groupadd docker
        sudo usermod -aG docker $USER
        newgrp docker

        sudo systemctl enable docker.service
        sudo systemctl enable containerd.service

0. Create Docker mymacvlan network 

        user@server:~/.docker$ docker network ls
        NETWORK ID     NAME              DRIVER    SCOPE
        78716734b523   bridge            bridge    local
        cc299dac7f01   compose_default   bridge    local
        758b47a9aa52   host              host      local
        513d89b730ee   none              null      local

        docker network create -d macvlan -o parent=enp3s0 \
        --subnet 192.168.1.0/24 \
        --gateway 192.168.1.1 \
        --ip-range 192.168.1.200/29 \
        --aux-address 'host=192.168.1.254' \
        mymacvlan

    Add Shim to be executed at the Boot time via cron
    
        sudo systemctl status cron.service
        sudo systemctl enable cron.service

        user@server:~/.docker$ docker network ls
        NETWORK ID     NAME              DRIVER    SCOPE
        78716734b523   bridge            bridge    local
        cc299dac7f01   compose_default   bridge    local
        758b47a9aa52   host              host      local
        e111dfe3d5de   mymacvlan         macvlan   local
        513d89b730ee   none              null      local

0. Copy backup folders

        /home/user/.config/appdata
        /home/user/.docker

0. Setup git

        cd ~/.docker
        git config --global user.name "Gunay Anach"; git config --global user.name
        git config --global user.email "anach@live.com"; git config --global user.email
        git checkout master

    > user@server:~/.docker$ git checkout master \
    A       new_setup.md \
    Already on 'master' \
    Your branch is up to date with 'origin/master'.

0. Edit ~/.docker/compose/.env to reflect the Data/Media Drive paths

        DOCKERSTORAGEDIR='/media/user/DT01ACA200'

0. Install [Python3.9 as CoralTPU PyCoral works on it](https://forums.linuxmint.com/viewtopic.php?f=42&p=2103213)

    Switch to Python3.9 as default

        pyenv versions
        pyenv shell 3.9

        user@server:~/OneDrive/Development/DockSTARTer$ 
        python --version Python 3.9.21
      
0. Install Coral PCIe - [If errors](https://github.com/google-coral/edgetpu/issues/866)

        # ensure the Coral PCIe is detected
        lspci -nn

    > 04:00.0 System peripheral [0880]: Global Unichip Corp. Coral Edge TPU [1ac1:089a]


        lsb_release -a
        uname -r
	    lsmod | grep apex

        echo "deb https://packages.cloud.google.com/apt coral-edgetpu-stable main" | sudo tee /etc/apt/sources.list.d/coral-edgetpu.list
        curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add - ; sudo apt update

        sudo apt install dh-dkms gasket-dkms libedgetpu1-std devscripts git debhelper -y #prolly redundant packages
        git clone  https://github.com/google/gasket-driver; cd gasket-driver
        debuild -us -uc -tc -b -d

        sudo sh -c "echo 'SUBSYSTEM==\"apex\", MODE=\"0660\", GROUP=\"apex\"' >> /etc/udev/rules.d/65-apex.rules"
        sudo groupadd apex
        sudo adduser $USER apex

        after a reboot or '
        modprobe -a gasket # should be silent
        ls /dev/apex_0

    [If fails follow this](https://github.com/jnicolson/gasket-builder)

0. Ensure Coral TPU recognised in frigate container logs

        docker logs frigate

    > 2025-01-03 12:47:58.532332992  [2025-01-03 12:47:58] frigate.detectors.plugins.edgetpu_tfl INFO    : Attempting to load TPU as pci \
    2025-01-03 12:47:58.605419549  [2025-01-03 12:47:58] frigate.detectors.plugins.edgetpu_tfl INFO    : TPU found


0. Run DockSTARTer Initialisation

        git checkout master; git pull 
        cd ~/.docker
        bash main.sh

    Reboot

        cd ~/.docker
        ds

    Should pickup existing apps just confirm settings and let it rebuild docker-compose

0. Check if all apps are running in portainer

    > 192.168.1.190:9000



