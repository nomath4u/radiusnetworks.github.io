---
layout: message_radius
---

## Using the Message Radius API

### With curl

To get the JSON of the base url you could use the following command:

    curl -H "Content-type: application/json" \
         -H "Accept: application/json" -H 'Authorization: Token token="my-token" \
         "http://api.messageradius.com/"

We suggest you use the [mr-curl utility](/docs/api/tools) to keep the command line requests sane.


### Ruby

Simple example to resolve the beacon index url using [faraday](https://github.com/technoweenie/faraday):

    #!/usr/bin/env ruby
    require 'json'
    require 'faraday'

    url = 'http://api.messageradius.com'
    conn = Faraday.new(url: 'http://api.messageradius.com')
    resp = conn.get do |req|
      req.url '/'
      # Note: This header is picky about using double quotes around the actual token
      req.headers['Authorization'] = "Token token=\"sandbox\""
      req.headers['Accept'] = "application/json"
    end

    body = JSON.parse(resp.body)
    beacon_url = body["links"]["beacons"]["href"]
    puts "The beacons index is #{beacon_url}"

