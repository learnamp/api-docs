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

`GET https://{API_BASE_URL}/v1/channels`

### Required Scope
This endpoint requires the `channels:read` scope.

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

## Users Progress

> View progress of all assigned users through a specified channel:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/channels/123/users_progress' \
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

    def users_progress(id)
      response = self.class.get("/channels/#{id}/users_progress", { headers: headers })
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

data = Learnamp::Channels.new(token).users_progress(123)
```

View progress of all assigned users through a specified channel. This is analogous to Learn Amp's Content Log feature, for a single channel.

`GET https://{API_BASE_URL}/v1/channels/{channelId}/users_progress`

### Required Scope
This endpoint requires the `channel_users_progress:read` scope.

This end-point will return a paginated array of users who are assigned the specified channel. Each element will contain the completion percentage of the channel by that user, as well as the datetime (if any) when the channel was completed.

Please note: Only assigned users will be returned. Users who are not assigned the specified channel will not appear in the array.

Also, please note: The completion percentage is a cached value, which is refreshed in the background automatically. The completion percentage shown may therefore take a few minutes to update.

The results are ordered alphabetically by user's last name.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
[
    {
        "user_id": 200321,
        "first_name": "John",
        "last_name": "Abc",
        "email": "jabc@test.com",
        "content_name": "Favourites",
        "content_type": "Channel",
        "content_id": 1,
        "completed": false,
        "completion_percent": 50,
        "completed_at": null
    },
    {
        "user_id": 200018,
        "first_name": "Hannah",
        "last_name": "Baker",
        "email": "hbaker@test.com",
        "content_name": "Favourites",
        "content_type": "Channel",
        "content_id": 1,
        "completed": true,
        "completion_percent": 100,
        "completed_at": "2023-04-04T15:44:04Z"
    },
    {
        "user_id": 199915,
        "first_name": "Brian",
        "last_name": "Cross",
        "email": "bcross@test.com",
        "content_name": "Favourites",
        "content_type": "Channel",
        "content_id": 1,
        "completed": false,
        "completion_percent": 0,
        "completed_at": null
    }
]

```

> 404 Not Found - unsuccessful response when channel ID does not exist:

```json
{
    "error": "Not found"
}
```
