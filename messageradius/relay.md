---
layout: messageradius
---

## Relay API

This API provides rapid updates of all devices discovered or dropped from each Beacon.  It must be enabled on a per beacon basis to post the information to a receiving URL.  This receiving URL must be configured with the beacons by the Radius Networks engineering team.

Example data:

    {
        "bid": "10026",
        "type": "dreamplug",
        "version": "0.1",
        "scan_type": "delta",
        "devices": [
            {
	        "device_token": "38fbdde984330e50c02382e647c576b71f41cc5c45b193d4f3177e6ee8f22a78",
                "count": 2,
                "first_timestamp": "2012-09-21 09:32:48 -0400",
                "session": "mapcao",
                "last_timestamp": "2012-09-21 09:32:48 -0400",
                "essid": "stc_guest",
                "rssi_max": "-79",
                "rssi_min": "-79",
                "noise_max": "0",
                "noise_min": "0",
                "channel_max": "10",
                "channel_min": "10",
                "subtype_max": "Probe Request",
                "subtype_min": "Probe Request"
            },
            {
	        "device_token": "5694f4ddbb326cda53dc0f4c6667d5a5bc34305b4dbac1dd14cc08c3b8641fc4",
                "count": 2,
                "first_timestamp": "2012-09-21 09:32:48 -0400",
                "session": "mapcao",
                "last_timestamp": "2012-09-21 09:32:48 -0400",
                "essid": "stc_secure",
                "rssi_max": "-87",
                "rssi_min": "-87",
                "noise_max": "0",
                "noise_min": "0",
                "channel_max": "6",
                "channel_min": "6",
                "subtype_max": "Probe Request",
                "subtype_min": "Probe Request"
            },
            {
	        "device_token": "38fbdde984330e50c02382e647c576b71f41cc5c45b193d4f3177e6ee8f22a78",
                "count": 1,
                "first_timestamp": "2012-09-21 09:32:48 -0400",
                "session": "mapcao",
                "last_timestamp": "2012-09-21 09:32:48 -0400",
                "essid": "Daisy",
                "rssi_max": "-87",
                "rssi_min": "-87",
                "noise_max": "0",
                "noise_min": "0",
                "channel_max": "6",
                "channel_min": "6",
                "subtype_max": "Beacon",
                "subtype_min": "Beacon"
            },
            {
	        "device_token": "5694f4ddbb326cda53dc0f4c6667d5a5bc34305b4dbac1dd14cc08c3b8641fc4",
                "count": 7,
                "first_timestamp": "2012-09-21 09:30:20 -0400",
                "session": "mapc6k",
                "state": "dropped",
                "event": "dropped",
                "last_timestamp": "2012-09-21 09:31:42 -0400",
                "essid": "5BDL9",
                "rssi_max": "-89",
                "rssi_min": "-89",
                "noise_max": "0",
                "noise_min": "0",
                "channel_max": "6",
                "channel_min": "6",
                "subtype_max": "Probe Request",
                "subtype_min": "Probe Request"
            },
            {
                "count": 9,
                "first_timestamp": "2012-09-21 09:30:20 -0400",
                "session": "mapc6k",
                "state": "dropped",
                "event": "dropped",
                "last_timestamp": "2012-09-21 09:31:42 -0400",
                "essid": "Shakespeare",
                "device_token": "
                "rssi_max": "-43",
                "rssi_min": "-43",
                "noise_max": "0",
                "noise_min": "0",
                "channel_max": "4",
                "channel_min": "4",
                "subtype_max": "Probe Request",
                "subtype_min": "Probe Request"
            }
        ]
    }
