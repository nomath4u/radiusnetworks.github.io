---
layout: messageradius
---

## REST API

### Base URL

All URLs referenced in the documentation have the following base:

    https://api.messageradius.com

The Message Radius REST API is served over HTTPS. To ensure data privacy, unencrypted HTTP is not supported.

### Links

All clients should start from the Base URL and use that to navigate to the resource they want via the rel attribute of a link.

Links are contained within a reserved `links` element. The `rel` of the link is simply the key within the hash:

    {
      "links": {
        "subscriptions": { "href": "https://api.messageradius.com/subscriptions" },
        "beacons": { "href": "https://api.messageradius.com/beacons" }
      }
    }

It is strongly encouraged to use this method of discovery for the API to keep your clients flexible and able to adjust to future changes in the Message Radius API. The ability to work over generic links allows us to scale as demands grow.

### RESTful Resources

* [Accounts](accounts.html)
* [Beacon](beacons.html)
  * [Beacon by Day](beacons.html)
* [Subscriptions](subscriptions.html)

