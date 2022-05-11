# Channels

## Channel Description

In Learn Amp, a Channel is a collection of content (items, learnlists, events etc), arranged into a learning pathway.

## View All Channels

> View all channels in your account:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/channels' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Channels
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(filters)
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/channels?#{filters_query}", { headers: headers })
      response.parsed_response
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

filters = {
}
teams = Learnamp::Channels.new(token).all(filters)
```

View all channels

`GET https://api.learnamp.com/v1/channels`

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
{
    "channels": [
        {
            "id": 379,
            "title": "Test Channel"
        },
  ]
}

```
