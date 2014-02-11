---
layout: proximitykit
---

# Proximity Kit admin API

# Authorization

## API Key

The API Key is passed via the Authorization header:

    Authorization: Token token="secret"

The API Key is associated with your account and has access to all the resources associated with your account. Account-specific API keys have different permissions than the web login users that can interact with the dashboard, and the access may be different.

Example:

    curl -H 'Authorization: Token token="sandbox"' "http://proximitykit.com/api/v1.json"

If you do not have an API key, [please contact us](mailto:support@radiusnetworks.com?subject=Proximity Kit API Token Request) for one.

Note: Per [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec2.html#sec2.2) the Authorization Header's token needs to be surrounded by double quotes (`"`).

# API Root

    GET /api/v1

The list of links for the resources avaliable to your account.

Root Document:

```
{
  "links": {
    "kits_url": "http://proximitykit.com/api/v1/kits.json",
    "ibeacons_url": "http://proximitykit.com/api/v1/ibeacons.json"
  }
}
```

# Kits

## Listing Kits

```
GET /api/v1/kits
```

This will return a list of kits, ibeacons and associated attributes.

```
{
  "kits": [
    {
      "name": "My Kit",
      "id": 1,
      "url": "http://proximitykit.com/api/v1/kits/1.json",
      "ibeacons": [
        {
          "id": 1,
          "name": "My First Beacon",
          "uuid": "d16eae19-6fce-4198-a5a5-469c9599b709",
          "major": 1,
          "minor": 1,
          "url": "http://proximitykit.com/api/v1/ibeacons/1.json",
          "attributes": []
        },
        {
          "id": 2,
          "name": "My Second Beacon",
          "uuid": "d16eae19-6fce-4198-a5a5-469c9599b709",
          "major": 1,
          "minor": 2,
          "url": "http://proximitykit.com/api/v1/ibeacons/2.json",
          "attributes": []
        },
      ]
    }
  ]
}
```

## Creating a Kit

To create a kit make a post request to the `kits_url`.

    POST /api/v1/kits

Example:

    {"kit": { "name": "My New Name" } }

Required Parameters

* `name` The name of the kit

## Updating a Kit

To update a kit make a put request to the `kits_url`.

```
PUT /api/v1/kit/{kit_id}
```

Example:

```
{"kit": { "name": "My New Name" } }
```

Parameters

* `name` The name of the kit

## Deleting a Kit

To delete a kit make a delete request to the individual `kit_url`.

```
DELETE /api/v1/kit/{kit_id}
```

# iBeacons

## Listing iBeacons

```
GET /api/v1/ibeacons
```

This will return a list of all beacons and associated attributes.

```
{
  "ibeacons": [
    {
      "id": 1,
      "name": "My First Beacon",
      "uuid": "d16eae19-6fce-4198-a5a5-469c9599b709",
      "major": 1,
      "minor": 1,
      "url": "http://proximitykit.com/api/v1/ibeacons/1.json",
      "attributes_url": "http://proximitykit.com/api/v1/ibeacons/1/attributes.json",
      "attributes": [
        {
          id: 1,
          key: "venue",
          value: "arena",
	  url: "http://proximitykit.com/api/v1/ibeacons/1/attributes/1"
        }
      ]
    },
    {
      "id": 2,
      "name": "My Second Beacon",
      "uuid": "d16eae19-6fce-4198-a5a5-469c9599b709",
      "major": 1,
      "minor": 2,
      "url": "http://proximitykit.com/api/v1/ibeacons/2.json",
      "attributes": []
    },
  ]
}
```

## Creating a ibeacon

To create a ibeacon make a post request to the `ibeacons_url`.

```
POST /api/v1/ibeacons
```

Example:

```
{
  "ibeacon": {
    "kit_id": 1,
    "name": "My New Name",
    "uuid": "d16eae19-6fce-4198-a5a5-469c9599b709",
    "major": 1,
    "minor": 1
  }
}
```

Required Parameters

* `kit_id` The ID of the associated kit
* `uuid` The iBeacon UUID

Parameters

* `name` The name of the iBeacon
* `major` The major iBeacon identifier
* `minor` The minor iBeacon identifier

## Updating a ibeacon

To update a ibeacon make a put request to the `ibeacons_url`.

```
PUT /api/v1/ibeacon/{ibeacon_id}
```

Example:

```
{
  "ibeacon": {
    "kit_id": 1,
    "name": "My New Name",
    "uuid": "d16eae19-6fce-4198-a5a5-469c9599b709",
    "major": 1,
    "minor": 1,
  }
}
```

Required Parameters

* `kit_id` The ID of the associated kit

Parameters

* `uuid` The iBeacon UUID
* `name` The name of the iBeacon
* `major` The major iBeacon identifier
* `minor` The minor iBeacon identifier

## Deleting a ibeacon

To delete a ibeacon make a delete request to the individual `ibeacon_url`.

```
DELETE /api/v1/ibeacon/{ibeacon_id}
```

```
# iBeacons Attributes

## Listing iBeacons Attributes

```
GET /api/v1/ibeacon_attributes
```

This will return a list of ibeacon attributes.

```
{
  "ibeacon_attributes": [
      {
        id: 1,
        key: "venue",
        value: "arena",
        url: "http://proximitykit.com/api/v1/ibeacons/1/attributes/1.json"
      }
    ]
  }
}
```

## Creating an attribute

To create an attribute make a post request to the iBeacon's `attributes_url`.

```
POST /api/v1/ibeacons/{ibeacon_id}/attributes
```

Example:

```
{
  "ibeacon_attribute": { "beacon_id": 1, "key": "venue", "value": "arena" }
}
```

Required Parameters

* `beacon_id` The ID of the associated iBeacon

Parameters

* `key` The key value
* `value` The data associated with the key


## Updating an attribute

To update an attribute make a put request to the `ibeacon_attribute_url`.

```
PUT /api/v1/ibeacons{ibeacon_id}/attributes/{ibeacon_attribute_id}
```

Example:

```
{
  "ibeacon_attribute": { key: "venue", value: "arena" }
}
```

Parameters

* `key` The key value
* `value` The data associated with the key


## Deleting an attribute

To delete an attribute make a delete request to the individual `ibeacon_attribute_url`.

```
DELETE /api/v1/ibeacons/{ibeacon_id}/attributes/{ibeacon_attribute_id}
```
