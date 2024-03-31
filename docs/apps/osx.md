# OSX

<!-- [![Docker Pulls](https://img.shields.io/docker/pulls/adguard/adguardhome?style=flat-square&color=607D8B&label=docker%20pulls&logo=docker)](https://hub.docker.com/r/adguard/adguardhome)
[![GitHub Stars](https://img.shields.io/github/stars/AdguardTeam/AdGuardHome?style=flat-square&color=607D8B&label=github%20stars&logo=github)](https://github.com/AdguardTeam/AdGuardHome)
[![Compose Templates](https://img.shields.io/static/v1?style=flat-square&color=607D8B&label=compose&message=templates)](https://github.com/GhostWriters/DockSTARTer/tree/master/compose/.apps/adguard) -->

## Description

[OSX](https://github.com/sickcodes/Docker-OSX) 

## Install/Setup on Debian

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

## Run on Debian

Once container starts, connect to your Debian server and you should see the QEMU VNC like window loading Mac OS Instller. 