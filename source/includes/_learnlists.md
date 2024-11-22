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

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
{
    "learnlists": [
        {
            "id": 379,
            "title": "Test learnlist",
            "description": "This is the learnlist description"
        },
  ]
}

```
