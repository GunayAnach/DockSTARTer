# Mosquitto

[![Docker Pulls](https://img.shields.io/docker/pulls/_/eclipse-mosquitto?style=flat-square&color=607D8B&label=docker%20pulls&logo=docker)](https://hub.docker.com/_/eclipse-mosquitto)
[![GitHub Stars](https://img.shields.io/github/stars/eclipse/mosquitto?style=flat-square&color=607D8B&label=github%20stars&logo=github)](https://github.com/eclipse/mosquitto)
[![Compose Templates](https://img.shields.io/static/v1?style=flat-square&color=607D8B&label=compose&message=templates)](https://github.com/GhostWriters/DockSTARTer/tree/master/compose/.apps/mosquitto)

## Description

Mosquitto is an open source implementation of a server for version 5.0, 3.1.1,
and 3.1 of the MQTT protocol. It also includes a C and C++ client library, and
the `mosquitto_pub` and `mosquitto_sub` utilities for publishing and
subscribing.

## Install/Setup

Enable mosquite in DS. Then create/edit configuration files as specified in .config/appdata/mosquitto/config/

> mosquitto.conf

    persistence true
    persistence_location /mosquitto/data/
    # log_dest file /mosquitto/log/mosquitto.log
    listener 1883
    allow_anonymous false
    # use_identity_as_username false
    # user frigate
    password_file /mosquitto/config/mqtt-auth.conf
    acl_file /mosquitto/config/mqtt-acl.conf

Connect to portainer mosquitto container via sh and generate users and passwords

    mosquitto_passwd -b mosquitto/config/mqtt-auth.conf enter_username enter_password

> mqtt-auth.conf

    frigate:$7$101$QDa46iG.....
    admin:$7$101$QDa566iA.....

>  mqtt_acl.conf

    user frigate
    topic /frigate/#

    user admin
    topic readwrite


## Add Mosquito to Home Assistant

Go to HA > Settings > Devices and Services
Add Integration > MQTT

Broker Options:
Broker: Homeserver
Port: 1883
Username: set in mosquitto.cong - password_file
Password: set in mosquitto.cong - password_file