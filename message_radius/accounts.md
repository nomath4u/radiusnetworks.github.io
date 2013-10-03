---
layout: message_radius
---

## Accounts Resource

The account resource is listed at the base url. This resource contains information about your account as well as links to the pertinent resources you have access to.

Request:

    GET https://api.messageradius.com/

Request the `base` url; the account is contained in the root document.

Response:

    {
      "account_id": "5034fe38639184d116000001",
      "name": "Radius Networks",
      "links": {
        "subscriptions": { "href": "https://api.messageradius.com/api/subscriptions" },
        "beacons": { "href": "https://api.messageradius.com/api/beacons" }
      }
    }

Example:

    $ mr-curl "https://api.messageraidus.com/"
