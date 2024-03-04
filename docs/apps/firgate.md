# Adguard

[![Docker Pulls](https://img.shields.io/docker/pulls/adguard/adguardhome?style=flat-square&color=607D8B&label=docker%20pulls&logo=docker)](https://hub.docker.com/r/adguard/adguardhome)
[![GitHub Stars](https://img.shields.io/github/stars/AdguardTeam/AdGuardHome?style=flat-square&color=607D8B&label=github%20stars&logo=github)](https://github.com/AdguardTeam/AdGuardHome)
[![Compose Templates](https://img.shields.io/static/v1?style=flat-square&color=607D8B&label=compose&message=templates)](https://github.com/GhostWriters/DockSTARTer/tree/master/compose/.apps/adguard)

## Description

[Adguard](https://www.github.com/AdguardTeam/AdGuardHome) is a network-wide ads
& trackers blocking DNS server.

## Install/Setup

- Install via DS
- Install Mosquitto via DS
- Install HACS onto the HA container - https://hacs.xyz/docs/setup/download
- Configure HACS - https://hacs.xyz/docs/configuration/basic
- In Home Assistant, Settings > Integrations > Add Integration > Frigate
- - Add IP of the Frigate container and it should detect the streams


## Install Coral M.2 TPU with A+E key in Wifi slot
> Install required Software

https://haade.fr/en/blog/frigate-nvr-google-coral-ai-hardware-acceleration

> Add detectors in Frigate

    detectors:
    coral:
        type: edgetpu
        device: pci # or usb depending on your equipment


## Configuration

> Make sure these files are set in ~/.config/appdata/frigate/config/cofig.yml

    mqtt:
        enabled: True
        host: mosquitto
        port: 1883
        topic_prefix: frigate
        # client_id: frigate
        user: ~~admin user name~~
        password: ~~mqtt admin user password~~

    detectors:
    <!-- cpu0:
        type: cpu -->
    coral:
        type: edgetpu
        device: pci # or usb depending on your equipment

    ffmpeg:
    hwaccel_args:
        - -hwaccel
        - vaapi
        - -hwaccel_device
        - /dev/dri/renderD128
        - -hwaccel_output_format
        - yuv420p

    go2rtc:
    hass:
        config: "/hass_config"
    streams:
        secure-lockers: 
        # https://www.reddit.com/r/frigate_nvr/comments/17zv4jt/audio_in_live_stream_but_not_recording_for_a/
        - rtsp://admin:655603@192.168.1.8:554/stream2
        - "ffmpeg:living-room#audio=opus"
    webrtc:
        candidates:
        - 192.168.1.28:8555
        - stun:8555

    birdseye:
    enabled: True
    width: 640
    height: 360
    quality: 1
    mode: objects

    cameras:
    secure_lockers:
        ffmpeg:
        output_args:
            record: preset-record-generic-audio-aac    
        inputs:
            - path: rtsp://127.0.0.1:8554/secure-lockers?video&audio
            input_args: preset-rtsp-restream
            # rtsp://admin:655603@DCS-8515LH-B0C55458CA29:554/stream2
            roles: 
                - record
            - path: rtsp://admin:655603@192.168.1.8:554/stream1
            roles:
                - detect
                - rtmp

        detect:
        width: 640
        height: 360
        fps: 5

        motion:
        mask:
            - 93,0,59,360,0,360,0,0,0,0
            - 640,34,640,0,0,0,0,32
            - 33,360,640,360,640,304,351,275,66,273

        objects:
        track:
            - person
        filters:
            person:
            threshold: 0.1
            min_area: 5000
            max_area: 100000

        snapshots:
        enabled: true
        timestamp: false
        bounding_box: true
        retain:
            default: 10

        record:
        enabled: True
        retain:
            days: 10
        events:
            retain:
            default: 10
            mode: motion


## Usage

Once container is running and no ERRORS in the logs, web UI can be accessed via - http://frigate-ip-address:5000/