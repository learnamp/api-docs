# Learnlists

## Learnlist Description

In Learn Amp, a Learnlist is a 'playlist' of content. It can be structured, like a course with a specific sequence, or an unordered collection of learning items.

## View All Learnlists

> View all learnlists in your account:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/learnlists' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Learnlists
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(filters)
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/learnlists?#{filters_query}", { headers: headers })
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
teams = Learnamp::Learnlists.new(token).all(filters)
```

View all learnlists

`GET https://{API_BASE_URL}/v1/learnlists`

### Required Scope
This endpoint requires the `learnlists:read` scope.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
{
    "learnlists": [
        {
            "id": 379,
            "name": "Test learnlist",
            "description": "This is the learnlist description"
        },
  ]
}

```

## Show a Learnlist

> Display details for a single Learnlist:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/learnlists/379' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Learnlists
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def find(id)
      response = self.class.get("/learnlists/#{id}", { headers: headers })
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

learnlist = Learnamp::Learnlists.new(token).find(379)
```

Display details for one specific Learnlist.

`GET https://{API_BASE_URL}/v1/learnlists/{learnlistId}`

### Required Scope
This endpoint requires the `learnlists:read` scope.

> 200 OK - successful response:

```json
{
    "id": 379,
    "name": "Test learnlist",
    "description": "This is the learnlist description"
}
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```
