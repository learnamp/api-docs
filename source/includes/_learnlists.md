# Learnlists

## Learnlist Description

In Learn Amp, a Learnlist is a 'playlist' of content. It can be structured, like a course with a specific sequence, or an unordered collection of learning items.

## View All Learnlists

> View all learnlists in your account:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/learnlists' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Learnlists
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

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
curl --location --request GET 'https://{API_BASE_URL}/v1/learnlists/379' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Learnlists
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

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

## List a Learnlist's Contents

> List the contents of a single Learnlist:

```shell
curl --location --request GET 'https://{API_BASE_URL}/v1/learnlists/379/contents' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Learnlists
    include HTTParty
    base_uri "#{ENV['API_BASE_URL']}/v1"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def contents(id, filters = {})
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/learnlists/#{id}/contents?#{filters_query}", { headers: headers })
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

contents = Learnamp::Learnlists.new(token).contents(379)
```

List all of a Learnlist's contents, in Learnlist order.

A Learnlist can contain more than just Items — it may also hold Quizzes, Surveys and Events. This endpoint returns every entry, regardless of type. Each entry carries a `type` discriminator (`Item`, `Quiz`, `Survey` or `Event`) which you can pair with the entry `id` to fetch type-specific detail from the relevant endpoint (e.g. `GET /v1/items/:id`).

`displayType` is the product-facing label for the entry type. For Learnlist contents it always mirrors `type`; it differs only for content types whose internal name differs from the name shown in the product.

`GET https://{API_BASE_URL}/v1/learnlists/{learnlistId}/contents`

### Required Scope
This endpoint requires the `learnlists:read` scope.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
{
    "contents": [
        {
            "id": 1180,
            "name": "Intro to Onboarding",
            "shortDescription": "A short welcome video.",
            "type": "Item",
            "displayType": "Item",
            "url": "https://examplecompany.learnamp.com/en/items/intro-to-onboarding",
            "totalTimeEstimate": "< 5 mins",
            "addedBy": {
                "id": 1
            }
        },
        {
            "id": 477,
            "name": "Onboarding Knowledge Check",
            "shortDescription": "Assess what you've learned.",
            "type": "Quiz",
            "displayType": "Quiz",
            "url": "https://examplecompany.learnamp.com/en/quizzes/477",
            "totalTimeEstimate": "< 1 hr",
            "addedBy": {
                "id": 1
            }
        },
        {
            "id": 58,
            "name": "Team Welcome Session",
            "shortDescription": null,
            "type": "Event",
            "displayType": "Event",
            "url": "https://examplecompany.learnamp.com/en/events/58",
            "totalTimeEstimate": "1 hr",
            "addedBy": {
                "id": null
            }
        }
    ]
}

```

`addedBy` contains only the `id` of the user who added the entry. When the entry was added by an external contributor, `addedBy.id` is `null`. For content types that do not track who added them, `addedBy` is omitted entirely.

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```
