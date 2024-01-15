# SoftEther VPN

[![Docker Pulls](https://img.shields.io/docker/pulls/softethervpn/vpnserver?style=flat-square&color=607D8B&label=docker%20pulls&logo=docker)](https://hub.docker.com/r/softethervpn/vpnserver)
<!-- [![GitHub Stars](https://img.shields.io/github/stars/AdguardTeam/AdGuardHome?style=flat-square&color=607D8B&label=github%20stars&logo=github)](https://github.com/AdguardTeam/AdGuardHome) -->
[![Compose Templates](https://img.shields.io/static/v1?style=flat-square&color=607D8B&label=compose&message=templates)](https://github.com/GunayAnach/DockSTARTer/tree/master/compose/.apps/softethervpn)

## Description

[SoftEther VPN](https://www.softether.org/) is an optimum alternative to OpenVPN and Microsoft's VPN servers. SoftEther VPN has a clone-function of OpenVPN Server. You can integrate from OpenVPN to SoftEther VPN smoothly. SoftEther VPN is faster than OpenVPN. SoftEther VPN also supports Microsoft SSTP VPN for Windows Vista / 7 / 8. No more need to pay expensive charges for Windows Server license for Remote-Access VPN function.

## Install/Setup
[docker support page](https://hub.docker.com/r/siomiz/softethervpn/)
Set correct Client User connection credentials in .env file
> SOFTETHERVPN_USERS='username1:password1;username2:password2'

## VPN User Credentials

Add credentials as variables to .env file

## Topology

    ┌─────────────────────────────────┐
    │          internet               │  IP:192.168.1.5 local router IP
    │  ┌───────────────────────────┐  │                 ┌────────────────────────────────┐
    │  │                           │  │◄────────────────┤ Windows: SoftEther VPN Client  │
    │  │       DOCKER              │  │                 │                                │
    │  │  SoftEther VPN Server   ┌─┼──┼────────────────►|  Username1/Password1           │
    │  │                         │ │  │IP:192.168.30.10 └────────────────────────────────┘
    │  │  Username1/Password1    │ │  │   VPN IP - open through port 5555
    │  │  Username2/Password2    │ │  │IP:192.168.30.31 ┌────────────────────────────────┐
    │  │                         └─┼──┼────────────────►│  MacOs: SoftEther VPN Client   │
    │  │  IP: 192.168.30.1         │  │                 │                                │
    │  │                           │  │◄────────────────┤  Username2/Password2           │
    │  └───────────────────────────┘  │                 └────────────────────────────────┘
    │                                 │  IP:192.168.2.5 local router IP
    └─────────────────────────────────┘

## Manual local docker run
    sudo docker run --cap-add=NET_ADMIN -p 5555:5555 -p 992:992 -p 443:443 -e PSK=SECRET -e USERNAME=SECRET_USERNAME -e PASSWORD=SECRET_PWD siomiz/softethervpn

## Connect to the VPN server 
[Step by Step Guide source](https://www.ionos.com/digitalguide/server/know-how/vpn-in-a-docker-container-using-softether/)

In order to connect to the SoftEther VPN server in your Docker container, you will need to download and install the SoftEther client on your desktop or laptop computer.

[Download the appropriate installer from the SoftEther download page and follow the instructions to install the SoftEther client.](http://www.softether.org/5-download)

To configure the VPN connection on Windows, double-click Add VPN Connection.
![Add VPN Connection](https://www.ionos.com/digitalguide/fileadmin/_processed_/2/d/csm_softether-1_c672851ff7.webp)

Fill out the Setting Name, Host Name, User Name, and Password. Everything else can be left at the defaults. Then click OK.
![Setting Name](https://www.ionos.com/digitalguide/fileadmin/DigitalGuide/Screenshots_2020/softether-2.jpg)

Right-click on your VPN connection and choose Connect.
![VPN connection](https://www.ionos.com/digitalguide/fileadmin/_processed_/8/1/csm_softether-3_a2795acb89.webp)

You will be connected to the VPN and your IP address will be displayed