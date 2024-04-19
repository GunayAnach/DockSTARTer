# OSX

<!-- [![Docker Pulls](https://img.shields.io/docker/pulls/adguard/adguardhome?style=flat-square&color=607D8B&label=docker%20pulls&logo=docker)](https://hub.docker.com/r/adguard/adguardhome)
[![GitHub Stars](https://img.shields.io/github/stars/AdguardTeam/AdGuardHome?style=flat-square&color=607D8B&label=github%20stars&logo=github)](https://github.com/AdguardTeam/AdGuardHome)
[![Compose Templates](https://img.shields.io/static/v1?style=flat-square&color=607D8B&label=compose&message=templates)](https://github.com/GhostWriters/DockSTARTer/tree/master/compose/.apps/adguard) -->

## Description 

> tested on Debian

[OSX](https://github.com/sickcodes/Docker-OSX) 

## Requirements

[Follow Initial Setup Page](https://github.com/sickcodes/Docker-OSX#initial-setup/).

Before you do anything else, you will need to turn on hardware virtualization in your BIOS. Precisely how will depend on your particular machine (and BIOS), but it should be straightforward.

Then, you'll need QEMU and some other dependencies on your host:

    # UBUNTU DEBIAN
    sudo apt install qemu qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virt-manager libguestfs-tools


Then, enable libvirt and load the KVM kernel module:

    sudo systemctl enable --now libvirtd
    sudo systemctl enable --now virtlogd

    echo 1 | sudo tee /sys/module/kvm/parameters/ignore_msrs

    sudo modprobe kvm

## Build Apple Docker manually, configure and extract the image
Execute:

    docker run -it \
    --device /dev/kvm \
    -p 50922:10022 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    sickcodes/docker-osx:monterey

Once container starts, connect to your Debian server and you should see the QEMU VNC like window loading Mac OS Instller. Erase biggest drive (its expanding drive as the needs of the OS, so you can leave at max size in GB)

## Optimize the OSx
These optimizations will boost the system and will make it smooter to use

        # disable heavy login wallpaper
        defaults write com.apple.loginwindow autoLoginUser -bool true
        # disable spotlight
        sudo mdutil -i off -a
        # disable updates
        sudo defaults write /Library/Preferences/com.apple.loginwindow DesktopPicture ""
        # reduce motion smoothness
        defaults write com.apple.Accessibility DifferentiateWithoutColor -int 1
        defaults write com.apple.Accessibility ReduceMotionEnabled -int 1
        defaults write com.apple.universalaccess reduceMotion -int 1
        defaults write com.apple.universalaccess reduceTransparency -int 1
        defaults write com.apple.Accessibility ReduceMotionEnabled -int 1
        # disable screen locking
        defaults write com.apple.loginwindow DisableScreenLock -bool true
        # disable apps going to sleep
        sudo -u "${REAL_NAME}" sudo defaults write NSGlobalDomain NSAppSleepDisabled -bool YES

        # as roots
        sudo su
        defaults write /Library/Preferences/com.apple.SoftwareUpdate AutomaticDownload -bool false
        defaults write com.apple.SoftwareUpdate AutomaticCheckEnabled -bool false
        defaults write com.apple.commerce AutoUpdate -bool false
        defaults write com.apple.commerce AutoUpdateRestartRequired -bool false
        defaults write com.apple.SoftwareUpdate ConfigDataInstall -int 0
        defaults write com.apple.SoftwareUpdate CriticalUpdateInstall -int 0
        defaults write com.apple.SoftwareUpdate ScheduleFrequency -int 0
        defaults write com.apple.SoftwareUpdate AutomaticDownload -int 0

## Personalize the OSx

Start FindMy and login with Apple Credentials
Install FindMySync
Setup FindMySyc to target 

## Shut down the OSx and Extract the IMG with

    sudo docker cp tender_shamir:/home/arch/OSX-KVM/mac_hdd_ng.img .config/appdata/osx/monterey_homeserver.img

