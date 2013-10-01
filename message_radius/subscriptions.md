---
layout: message_radius
---

## Subscribing to Beacon-Device Events


Message Radius makes it easy to be notified when registered mobile devices are detected by our system. We handle this through the use of subscriptions. You can register a device, and Message Radius will fire a webhook when that device is initially seen ("added") or is not longer seen ("dropped") by our system. In other words, notification occurs when the device is first noticed and when it leaves.

If you would like more information on how you can integrate this into your Mobile App, please see our [Mobile SDK Documentation](/docs/api/mobile).

A subscription works by monitoring all of the beacons in your account against a specific list of mac addresses (recommended for Android and other non-Apple devices) or Apple identifier for advertisers (recommended for iOS devices) that you provide.  When one of those identifiers is detected, we will fire a webhook callback to your backend.

### Beacon Events

Beacons can report three types of events for a device: `added`, `dropped`, and `changed`.

Using wifi, the beacon keeps an internal cache of devices, and as packets from devices are detected, we will continue to increment the time-to-live in our cache. After the cache expires, we fire the dropped event.
Using the iBeacon signal over bluetooth, the Message Radius client library embedded in your app reports proximity to beacons to the server, at which time we fire the added event.  When the server has not received a report for a device after an extended period, we fire the dropped event.

### Webhook POST

The following is an example JSON payload that would be posted as a result of a subscription:

    {
     "beacon_id":"10030",
     "devices"=>[
         {"event":"added",   "rssi":"-63", "device_token":"c1582e87c802221899199e286ead9a7ed13eb3b5e3827be6cc149fb82a9e04f7", "id_type":"idfa", partner_id:"12ae76-34"}
         {"event":"dropped", "rssi":"-80", "device_token":"8080dd0e596549287647c334a159d5c176fc365c999ddf75d34b95097768964e", "id_type":"mac",  partner_id:"9456-12e2"},
          ...
        ]
    }

The JSON payload will contain at least these fields:

<table class="table table-bordered">
  <tr>
    <th>Field</th> <th>Description</th>
  </tr>
  <tr> <td>`beacon_id`</td>    <td>This is the Beacon ID.  It is the same identifier you would see for a beacon in the dashboard.</td> </tr>
  <tr> <td>`devices`</td>      <td>An array of devices detected by the Beacon.</td> </tr>
  <tr> <td>`device_token`</td> <td>Device Token is a unique identifier for a specific device that has been detected.</td> </tr>
  <tr> <td>`id_type`</td> <td>Id Type is the type of identifier that was detected.  Its value can be "mac" for a mac address or "idfa" for an Apple Identifier For Advertising</td> </tr>
  <tr> <td>`partner_id`</td> <td>Partner Id is an optional client-supplied string associated with the device.</td> </tr>
  <tr> <td>`event`</td>        <td>Will be "added", "changed", or "dropped"; indicating that the Device has just appeared or left the list of devices being tracked by the Beacon.</td> </tr>
  <tr> <td>`rssi`</td>         <td>Radio Signal Strength between the Beacon and Device.</td> </tr>
</table>

A changed event is sent when the rssi value crosses a beacon zone boundary.  Three beacon zones are defined in the dashboard, representing a strong, moderate and weak device signal.  Because signal strength is proportional to device distance, this can be used to receive a webhook call as a device gets closer and further away from a beacon.


## Subscriptions

### List Subscriptions

None of the following URLs should be constructed or hard-coded and are subject to change (in the event we have to change hostnames as the API backend is segregated and scaled). All of the URLs should be followed starting at the root document and via the links.

Request:

    rel: subscriptions
    method: GET

Follow the `subscriptions` link from the root document.

Response:

    {
      "subscriptions" : [
        {
          "id": "50295392639184ab15000001",
          "webhook_url": "http://requestb.in/15jt2uj1",
          "account_id": "1",
          "devices": [
            {
              device_token: "1827044dcec0a3856f993ca80bc4f37f5edbd04a406c941fd42ee763a9b8df88", 
              id_type:"mac", 
              partner_id:"53c7a-6"
            }
          ],
          "links": {
            "self": { "href": "https://api.messageradius.com/api/subscriptions/50295392639184ab15000001.json" },
            "delete": { "method":"delete", "href": "https://api.messageradius.com/api/subscriptions/50295392639184ab15000001.json" },
            "update": { "method":"put", "href": "https://api.messageradius.com/api/subscriptions/50295392639184ab15000001.json" }
          }
        },
        {
          "id": "5101b58b433e92879f000001",
          "webhook_url": "http://example.messageradius.com/webhook",
          "account_id": "5034fe38639184d116000001",
          "devices": [
            {
              device_token: "3a273038f956ffd6bd0fda2f75676872325723be14f783914f827f1368cc5811", 
              id_type:"mac", 
              partner_id:"53c7a-6"
            },
            {
              device_token: "5694f4ddbb326cda53dc0f4c6667d5a5bc34305b4dbac1dd14cc08c3b8641fc4", 
              id_type:"mac", 
              partner_id:"9947-c9"
            },
            {
              device_token: "38fbdde984330e50c02382e647c576b71f41cc5c45b193d4f3177e6ee8f22a78", 
              id_type:"idfa", 
              partner_id:"376a-b2"
            },
            {
              device_token: "c1582e87c802221899199e286ead9a7ed13eb3b5e3827be6cc149fb82a9e04f7", 
              id_type:"mac", 
              partner_id:"367a0-4"
            }
          ],
          "links": {
            "self": { "href": "https://api.messageradius.com/api/subscriptions/5101b58b433e92879f000001.json" },
            "delete": { "method": "delete", "href": "https://api.messageradius.com/api/subscriptions/5101b58b433e92879f000001.json" },
            "update": { "method": "put", "href": "https://api.messageradius.com/api/subscriptions/5101b58b433e92879f000001.json" }
          }
        }
      ]
    }


### Create Subscription

Creating a subscription is accomplished by posting json using the following template.

Subscription `webhook_url`s must be unique.  However, if you want to add additional mac addresses to be tracked, you can POST to the create rel and it will merge the list of macs with the existing subscription.

Request:

    rel: create
    method: POST

Follow the `create` link from `subscriptions`.

Request Body:

    {
      "name": "My Webhook",
      "event": ["added"],
      "webhook_url": "http://example.messageradius.com/webhook",
      "devices": [
        {
          "device_id":"b8:f6:b1:13:48:6a", 
          "id_type":"mac", 
          "partner_id":"1001"},
        {
          "device_id":"aebe52e7-03ee-455a-b3c4-e57283966239", 
          "id_type":"idfa", 
          "partner_id":"1002"},
        {
          "device_id":"2765af08-9f45-2270-6a29-736ab90024e6",
          "id_type":"idfa", 
          "partner_id":"1003"},
          

      ]
    }

Example:

    mr-curl -X POST \
     -d '{"webhook_url":"http://example.messageradius.com/webhook","devices":[{"device_id":"b8:f6:b1:13:48:6a","id_type":"mac","partner_id":"1001"}]}' \
     "http://api.messageradius.com/api/subscriptions"


Response:

    {
      "subscriptions:" [
        {
          "webhook_url": "http://example.messageradius.com/webhook",
          "devices": [
            {
              "device_token": "c1582e87c802221899199e286ead9a7ed13eb3b5e3827be6cc149fb82a9e04f7",
              "id_type": "mac",
              "partner_id": "1001"
            }
          ],
          "device_mappings": [
            {
              "device_id": "aa:bb:cc:dd:ee:ff",
              "device_token": "c1582e87c802221899199e286ead9a7ed13eb3b5e3827be6cc149fb82a9e04f7"
            }
          ],
          "links":{"self":{"href":"https://api.messageradius.com/api/subscriptions/5101b58b433e92879f000001"}}
        }
      ]
    }


This response contains a `device_mappings` that gives the mapping between the newly added device ids (either a mac address or an idfa) and their device tokens. If this mapping is important to you you will need to store this information in your system -- we do not store any plain mac addresses or idfas in our system.

Subsequent POSTs will add to the mac list:


    mr-curl -X POST \
     -d '{"webhook_url":"http://example.messageradius.com/webhook","devices":["device_id":"14:6f:b2:22:53:72","id_type":"mac"]}' \
     "http://api.messageradius.com/api/subscriptions"

Will result in the mac list being merged:

    {
      "subscriptions:" [
        {
          "devices": [
            {
              "device_token":"c1582e87c802221899199e286ead9a7ed13eb3b5e3827be6cc149fb82a9e04f7",
              "id_type": "mac",
              "partner_id": null
            },
            {
              "device_token":"8080dd0e596549287647c334a159d5c176fc365c999ddf75d34b95097768964e",
              "id_type": "mac",
              "partner_id": null
            }
          ],
          "device_mappings": [
            {
              "device_id": "14:6f:b2:22:53:72",
              "device_token": "8080dd0e596549287647c334a159d5c176fc365c999ddf75d34b95097768964e"
            }
          ],
        }    
      ]
    }


### Update Subscription

This overwrites an existing subscription.

To update the subscription you need to request the entire subscription resource body, change the JSON, and PUT it back to the resource. This will clobber any existing mac addresses or idfas you had in your devices array, so be sure to PUT the entire document.

Request:

    rel: self (from an embedded subscription resource)
    method: PUT

Follow the `update` link from a `subscription`.

Request:

    {
      "webhook_url": "http://example.messageradius.com/webhook",
      "devices": [
        {
          "device_id": "63:00:77:15:9a:04",
          "id_type": "mac",
          "partner_id": "1009"
        },
	...
      ]
    }

Example:

    mr-curl -X PUT \
     -d '{"devices":[{"device_id":"63:00:77:15:9a:04","id_type":"mac","partner_id":"1009"}]}' \
     "http://api.messageradius.com/api/subscriptions/2"

### Remove Subscription

This destroys a subscription from Message Radius.

Request:

    rel: delete
    method: DELETE

Follow the `delete` link from a `subscription`.

Example:

    mr-curl -X DELETE "http://api.messageradius.com/api/subscriptions/2"
