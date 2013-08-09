---
layout: default
---
## ![Dark Beacon](img/beacon-dark.svg)Beacon Aggregate Data

Beacon data is aggregated into documents containing the data for a given day and for the current status of each beacon.

### List Beacons

Request

    rel: beacons
    method: GET

Follow the `beacons` link from the root document.

Response:

{% highlight objective-c %}
[
  {
    "id": "514728beef578d920f000002",
    "name": "Lobby",
    "address": "3333 Water St, NW, DC",
    "bid": "FPI-1",
    "account_id": "514728beef578d920f000001",
    "active": false,
    "current_device_count": 185,
    "links": {
      "self": { "href": "https://api.messageradius.com/api/beacons/10000.json" },
      "histories": { "href": "https://api.messageradius.com/api/beacons/10000/history.json" },
      "today": { "href": "https://api.messageradius.com/api/beacons/10000/history.json" },
      "status": { "href": "https://api.messageradius.com/api/beacons/10000/status.json" }
    }
  },
  {
    "id": "51438db9ef578df4f4000001",
    "name": null,
    "address": null,
    "bid": "fpi-1",
    "account_id": null,
    "active": null,
    "current_device_count": 0,
    "links": {
      "self": { "href": "https://api.messageradius.com/api/beacons/fpi-1.json" },
      "histories": { "href": "https://api.messageradius.com/api/beacons/fpi-1/history.json" },
      "today": { "href": "https://api.messageradius.com/api/beacons/fpi-1/history.json" },
      "status": { "href": "https://api.messageradius.com/api/beacons/fpi-1/status.json" }
    }
  }
]
{% endhighlight %}

### Show Beacon


Request

    rel: beacon.self
    method: GET

Follow the `self` link from the beacon listing

Response:

{% highlight objective-c %}
{
  "id": "514728beef578d920f000002",
  "name": "Lobby",
  "address": "3333 Water St, NW, DC",
  "bid": "10000",
  "account_id": "514728beef578d920f000001",
  "active": true,
  "current_device_count": 234,
  "links": {
    "self": { "href": "https://api.messageradius.com/api/beacons/10000.json" },
    "today": { "href": "https://api.messageradius.com/api/beacons/10000/history.json" },
    "status": { "href": "https://api.messageradius.com/api/beacons/10000/status.json" }
  }
}
{% endhighlight %}

<a id="status"></a>
### Show Beacon Current Status Data

Request:

    rel: status
    method: GET

Either follow the 'status' link from the Beacon resource.

Response:


{% highlight objective-c %}
{
  "id": "514728be8a29ee1efe1c5cf5",
  "bid": "10000",
  "account_id": null,
  "updated_on": "2013-03-20T15:26:00+00:00",
  "active": true,
  "device_count": 234,
  "offline_seconds": 0,
  "devices": {
    "ac38d33e798e9e40101ef3928eda8e59e106d7c0024be74d0340fd3cac193ebe": {
      "oui": "ba:6b:27", "rssi_latest": -86, "last_timestamp": "2013-03-18 01:46:00 -0400", "first_timestamp": "2013-03-17 21:46:00 -0400"
    },
    "ada62d766ae53b7ca3113a324dec51dcbf523752e92021a1155679fced81a23d": {
      "oui": "ba:63:b1", "rssi_latest": -78, "last_timestamp": "2013-03-18 12:57:00 -0400", "first_timestamp": "2013-03-18 08:57:00 -0400"
    },
    "adb0fd794e84f2c14b02a694f3a73c8afdd92e5104993687ac6eba4b27a7286c": {
      "oui": "2d:26:85", "rssi_latest": -33, "last_timestamp": "2013-03-18 07:55:00 -0400", "first_timestamp": "2013-03-18 07:55:00 -0400"
    },
    ...
  },
  "links": {
    "self": { "href": "https://api.messageradius.com/api/beacons/10000/status.json" }
  }
}
{% endhighlight %}


<a id="history"></a>
### Show Beacon Historical Data

Beacon History represent a day's worth of aggregate data. Each day is broken down into buckets.


<table class="table table-bordered">
  <tr>
    <th>Field</th> <th>Description</th>
  </tr>
  <tr> <td> <code>bid</code></td>        <td>This is the Beacon ID. It is the same identifier you would see for a beacon in the dashboard.</td> </tr>
  <tr> <td><code>day</code></td>        <td>The daystamp from the beacon day. This uses a <code>YYYYMMDD</code> format.</td> </tr>
  <tr> <td><code>devices</code></td>    <td>Array of devices that were tracked during the day. </td> </tr>
  <tr> <td><code>data</code></td>       <td>The day's worth of data. This is broken down into aggregate buckets. </td></tr>
  <tr> <td><code>data.hour</code></td>  <td>The hour bucket. The added events received during a given hour for the beacon are nested under a <code>year.month.day.hour</code> key. The value is the number of added events. </td> </tr>
</table>

Request:

    rel: today
         history {daystamp}
    method: GET

Either follow the 'today' link or use the 'days' link template from the Beacon resource.

Response:

{% highlight objective-c %}
[
  {
    "bid": "10000",
    "total_events": 293,
    "day": "20130320",
    "data": {
      "hour": {
        "2013": {
          "03": {
            "20": {
              "00": 30,
              "01": 41,
              "02": 32,
              "03": 35,
              "04": 11,
              "05": 21,
              "06": 26,
              "07": 25,
              "08": 1,
              "09": 45,
              "10": 26,
              "11": 0
            }
          }
        }
      }
    },
    "devices": {
      "acb7c217aae20500f8888a7dc073e771a59f61a939e705ef0117127d36f1197e": {
        "oui": "af:a3:64", "rssi_latest": -85, "last_timestamp": "2013-03-20 03:26:00 -0400", "first_timestamp": "2013-03-20 01:26:00 -0400"
      },
      "af2fb81f380ae92df4fd640cea4db8030fa2d3039ad7e74f3eb454d096d9ec23": {
        "oui": "bf:96:f4", "rssi_latest": -65, "last_timestamp": "2013-03-20 03:26:00 -0400", "first_timestamp": "2013-03-20 03:26:00 -0400"
      },
      "a2cd166a5ce183c2f15042896f2b883955fa58a1ee072a828fcaea7d248a8998": {
        "oui": "00:1b:e9", "rssi_latest": -85, "last_timestamp": "2013-03-20 03:26:00 -0400", "first_timestamp": "2013-03-20 02:26:00 -0400"
      },
    "links": {
      "self": { "href": "https://api.messageradius.com/api/beacons/10000/history.json" }
    }
  },
  ...
]
{% endhighlight %}


Example:

    mr-curl "http://api.messageradius.com/api/beacons/10000/history"
<a id="day"></a>
For a specific day:

    mr-curl "http://api.messageradius.com/api/beacons/10000/history/20121031"

