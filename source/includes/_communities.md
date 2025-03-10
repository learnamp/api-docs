# Communities

## View All Community Posts

> For a given community, view all posts

```shell
curl --location --request GET 'https://api.learnamp.com/v1/communities/123/posts' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class CommunityPosts
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(community_id, filters)
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/communities/#{community_id}/posts?#{filters_query}", { headers: headers })
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
  "filters[name][created_at][from]": DateTime.now.to_s
}
teams = Learnamp::CommunityPosts.new(token).all(filters)
```

View all community posts

`GET https://{API_BASE_URL}/v1/communities/:id/posts`

### Required Scope
This endpoint requires the `communities_posts:read` scope.

Response will be paginated [see pagination](#pagination)


### Optional Filters in URL Params

The following URL params by be included, to filter the result set:

`GET https://{API_BASE_URL}/v1/communities/:id/posts?filters[created_at][from]=2022-01-01`

URL Param | Value | Description
--------- | ------- | -----------
filters[created_at][from] | "2021-12-31" | Assigned date range FROM date/time in ISO 8601 format
filters[created_at][to] | "2022-01-01 15:00:00" | Assigned date range TO date/time in ISO 8601 format

> 200 OK - successful response:

```json
[
    {
        "id": 1,
        "content": "<p>This is a post with tags  <span class=\"atwho-inserted\" contenteditable=\"false\" data-atwho-at-query=\"@\"><a href=\"/en/users/198990\" data-user-id=\"198990\">@John Smith</a></span> <span class=\"atwho-inserted\" contenteditable=\"false\" data-atwho-at-query=\"@\"><a href=\"/en/users/3\" data-user-id=\"3\">@Sarah Johnson</a></span>&nbsp;</p>",
        "contentAsText": "This is a post with tags @John Smith @Sarah Johnson",
        "pinned": false,
        "createdAt": "2025-02-07T10:31:46.812Z",
        "user": {
            "id": 1,
            "firstName": "Henry",
            "lastName": "Dorsett Case",
            "jobTitle": null,
            "email": "henry.dorsettcase@learnamp.com",
            "timeZone": "International Date Line West",
            "language": "en",
            "role": "viewer",
            "hireDate": null,
            "profileUrl": "https://testaccount.learnamp.com/en/users/1",
            "status": {
                "status": "Confirmed",
                "time": "On 20 Jun 23"
            }
        },
        "likes": {
            "count": 0,
            "liked_by": []
        },
        "mentions": [
            {
                "mentioned": {
                    "id": 198990,
                    "firstName": "John",
                    "lastName": "Smith",
                    "jobTitle": "",
                    "email": "john.smith@company.com",
                    "timeZone": "International Date Line West",
                    "language": "en",
                    "role": "viewer",
                    "hireDate": null,
                    "profileUrl": "https://testaccount.learnamp.com/en/users/198990",
                    "status": {
                        "status": "Confirmed",
                        "time": "On 12 Aug 24"
                    }
                },
                "mentioned_by": {
                    "id": 1,
                    "firstName": "Henry",
                    "lastName": "Dorsett Case",
                    "jobTitle": null,
                    "email": "henry.dorsettcase@learnamp.com",
                    "timeZone": "International Date Line West",
                    "language": "en",
                    "role": "viewer",
                    "hireDate": null,
                    "profileUrl": "https://testaccount.learnamp.com/en/users/1",
                    "status": {
                        "status": "Confirmed",
                        "time": "On 20 Jun 23"
                    }
                }
            },
            {
                "mentioned": {
                    "id": 3,
                    "firstName": "Sarah",
                    "lastName": "Johnson",
                    "jobTitle": null,
                    "email": "sarah.johnson@company.com",
                    "timeZone": "International Date Line West",
                    "language": "en",
                    "role": "viewer",
                    "hireDate": null,
                    "profileUrl": "https://testaccount.learnamp.com/en/users/1",
                    "status": {
                        "status": "Confirmed",
                        "time": "On 20 Jun 23"
                    }
                },
                "mentioned_by": {
                    "id": 1,
                    "firstName": "Henry",
                    "lastName": "Dorsett Case",
                    "jobTitle": null,
                    "email": "henry.dorsettcase@learnamp.com",
                    "timeZone": "International Date Line West",
                    "language": "en",
                    "role": "viewer",
                    "hireDate": null,
                    "profileUrl": "https://testaccount.learnamp.com/en/users/3",
                    "status": {
                        "status": "Confirmed",
                        "time": "On 20 Jun 23"
                    }
                }
            }
        ],
        "comments": []
    }
]
```
