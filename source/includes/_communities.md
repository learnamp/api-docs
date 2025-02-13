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
        "content": "<p> Hi guys <span class=\"atwho-inserted\" contenteditable=\"false\" data-atwho-at-query=\"@\"><a href=\"/en/users/198986\" data-user-id=\"198986\">@Mr Inactive McGoo</a></span> <span class=\"atwho-inserted\" contenteditable=\"false\" data-atwho-at-query=\"@\"><a href=\"/en/users/198990\" data-user-id=\"198990\">@new guy</a></span> <span class=\"atwho-inserted\" contenteditable=\"false\" data-atwho-at-query=\"@\"><a href=\"/en/users/1\" data-user-id=\"1\">@taylor williams</a></span>&nbsp;</p>",
        "pinned": false,
        "created_at_words": "Feb 07, 2025",
        "user": {
            "name": "Thomas Millward Wright"
        },
        "likes": {
            "count": 0,
            "liked_by": []
        },
        "mentions": [
            {
                "mentioned": {
                    "id": 1,
                    "name": "taylor williams",
                    "email": "taylor.williams@learnamp.com"
                },
                "mentioned_by": {
                    "id": 1,
                    "name": "Thomas Millward Wright",
                    "email": "thomas.millwardwright@learnamp.com"
                }
            }
        ],
        "comments": []
    }
]
```
