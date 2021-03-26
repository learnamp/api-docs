# Verbs

## Verb Description

In Learn Amp, each Activity has an associated Verb. Verbs are a concept in the TinCan/xAPI specification. To simplify integration with xAPI, our verbs are taken from the [Tin Can Registry](https://registry.tincanapi.com/).

## View All Verbs

> View all available verbs:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/verbs' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Verbs
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(filters)
      response = self.class.get("/verbs}", { headers: headers })
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

verbs = Learnamp::Verbs.new(token).all
```

View all verbs

`GET https://api.learnamp.com/verbs`

Response will be paginated [see pagination](#pagination)


> 200 OK - successful response:

```json
{
    "verbs": [
        {
            "id": 1,
            "name": "accepted",
            "tinCanId": "http://activitystrea.ms/schema/1.0/accept"
        },
        {
            "id": 4,
            "name": "added",
            "tinCanId": "http://activitystrea.ms/schema/1.0/add"
        },
        {
            "id": 100,
            "name": "arranged",
            "tinCanId": "http://id.tincanapi.com/verb/arranged"
        },
        {
            "id": 12,
            "name": "attended",
            "tinCanId": "http://activitystrea.ms/schema/1.0/attend"
        },
        {
            "id": 139,
            "name": "clicked",
            "tinCanId": "http://adlnet.gov/expapi/verbs/interacted"
        },
        {
            "id": 73,
            "name": "completed",
            "tinCanId": "http://adlnet.gov/expapi/verbs/completed"
        },
        {
            "id": 108,
            "name": "downloaded",
            "tinCanId": "http://id.tincanapi.com/verb/downloaded"
        },
        {
            "id": 140,
            "name": "enrolled",
            "tinCanId": "http://adlnet.gov/expapi/verbs/registered"
        },
        {
            "id": 76,
            "name": "failed",
            "tinCanId": "http://adlnet.gov/expapi/verbs/failed"
        },
        {
            "id": 38,
            "name": "listened to",
            "tinCanId": "http://activitystrea.ms/schema/1.0/listen"
        },
        {
            "id": 135,
            "name": "logged in",
            "tinCanId": "https://brindlewaye.com/xAPITerms/verbs/loggedin/"
        },
        {
            "id": 136,
            "name": "logged out",
            "tinCanId": "https://brindlewaye.com/xAPITerms/verbs/loggedout/"
        },
        {
            "id": 44,
            "name": "read",
            "tinCanId": "http://activitystrea.ms/schema/1.0/read"
        },
        {
            "id": 141,
            "name": "received cerificate for",
            "tinCanId": "http://activitystrea.ms/schema/1.0/receive"
        },
        {
            "id": 56,
            "name": "started",
            "tinCanId": "http://activitystrea.ms/schema/1.0/start"
        },
        {
            "id": 65,
            "name": "updated",
            "tinCanId": "http://activitystrea.ms/schema/1.0/update"
        },
        {
            "id": 133,
            "name": "viewed",
            "tinCanId": "http://id.tincanapi.com/verb/viewed"
        },
        {
            "id": 67,
            "name": "watched",
            "tinCanId": "http://activitystrea.ms/schema/1.0/watch"
        }
    ]
}
```