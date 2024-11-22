# Items

An item in Learn Amp is a learning object. It could be a SCORM, xAPI or AICC package. It could be a URL link to an external web page. It could be an embedded video, or downloadable file.

Items are typically organised into Channels and Learnlists.

## View all Items

> View all items in your account:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/items' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all
      response = self.class.get("/items", { headers: headers })
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

items = Learnamp::Items.new(token).all
```

View all items

`GET https://{API_BASE_URL}/v1/items`

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
{
    "items": [
        {
            "id": 3015,
            "title": "Activity 10",
            "shortDescription": null,
            "itemType": "Video",
            "itemCategory": "Audio/Visual",
            "itemUrl": "https://examplecompany.learnamp.com/en/items/activity-10"
        },
        {
            "id": 3014,
            "title": "rust-lang/rust",
            "shortDescription": null,
            "itemType": "Other",
            "itemCategory": "Other",
            "itemUrl": "https://examplecompany.learnamp.com/en/items/rust-lang-rust"
        }
    ]
}

```

## Show an Item

> Display details for a single Item:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/items/3015' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def find(id)
      response = self.class.get("/items/#{id}", { headers: headers })
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

item = Learnamp::Items.new(token).find(3015)
```

Display all details for one specific Item.

`GET https://{API_BASE_URL}/v1/items/{itemId}`


> 200 OK - successful response:

```json
{
    "id": 3015,
    "title": "Marketing 101",
    "shortDescription": null,
    "itemType": "Video",
    "itemCategory": "Audio/Visual",
    "itemUrl": "https://examplecompany.learnamp.com/en/items/marketing-101",
    "url": "https://www.youtube.com/watch?v=8Sj2tbh-ozE",
    "description": "<p>Testing Youtube video</p>",
    "slug": "marketing-101",
    "fileSize": null,
    "fileType": null,
    "expires": false,
    "ratingsCount": 0,
    "averageRating": 0.0,
    "goesLive": false,
    "sourceType": null,
    "sourceId": null,
    "createdAt": "2021-03-02T11:09:35Z",
    "updatedAt": "2021-03-02T12:24:02Z",
    "goesLiveAt": null,
    "image": "http://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,h_153,q_auto,w_260/v1614683346/c6q5ylwoencf2iaoc4zs.jpg",
    "supplier": {
        "id": 138,
        "name": "Youtube.com",
        "logo": "https://examplecompany.learnamp.com/assets/placeholders/organisation_avatar-cd6e9138f99068ded8cca1210674ac7abd1329738317547968943addc6056ae7.png"
    },
    "addedBy": {
        "id": 1,
        "firstName": "Test",
        "lastName": "User",
        "jobTitle": "Tester",
        "email": "test@example.com",
        "timeZone": "London",
        "language": "en",
        "role": "admin",
        "hireDate": "2017-02-01",
        "profileUrl": "https://examplecompany.learnamp.com/en/users/1",
        "status": {
            "status": "Confirmed",
            "time": "On 29 Nov 16"
        }
    },
    "displayAddedBy": false,
    "visibility": "Entire Company",
    "price": "Free",
    "totalTime": "< 10 mins",
    "tags": ["marketing", "video"]
}
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Create an Item

> Create a new Item:

```shell
curl --location --request POST 'https://api.learnamp.com/v1/items' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--form 'url=https://www.test.com' \
--form 'description=This is the description'
--form 'title=This is Item title'
--form 'itemType=video'
--form 'totalTime=less_than_one_hour'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def create(params)
      response = self.class.post("/items", { body: params, headers: headers })
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

params = {
  title: "Sales",
  url: "https://www.test.com"
  des1cription: "This is the description"
  tit1le: "This is Item title"
  ite1mType: "video"
  tot1alTime: "less_than_one_hour"
}

item = Learnamp::Items.new(token).create(params)
```

Create an Item

`POST https://{API_BASE_URL}/v1/items`

### Data in Body

Parameter (* required) | Example value | Description
--------- | ------- | -----------
title * | Sales and Marketing Guide | Title of item
url | https://www.test.com | URL to external web page for the item
description | Some description text | Description of the item
expires | true | If item must expire, set to true
expiresAt | 2022-12-31 | Expiry date when item should expire in ISO 8601 format
goesLive | true | If item should have a future go live date
goesLiveAt | 2022-12-31 | Future go live date in ISO 8601 format
imageUrl | https://sometileimageurl | URL of tile image for the item, must be publically accessible
imageUrl | https://sometileimageurl | URL of tile image for the item, must be publically accessible
visibility | entire_company | Item visibility. Must be one of "hidden", "selected", "entire_company"
sourceType | Udemy | Short string identifier of the source of the item, if is coming from an external LMS or other system
sourceId | e814koip | External identifier of item, if it if coming from an external LMS or other system
itemType | article | Item type, one of: "article", "document", "book", "q_and_a", "info_card", "video", "elearning", "infographic", "audio", "slides", "image", "project", "classroom", "coaching", "course", "event", "presentation", "meeting", "webinar", "action", "test", "qa", "announcement", "newsletter", "reminder", "website", "other"
totalTime | less_than_one_hour | Approximate time to complete. One of: "less_than_fifteen_minutes", "less_than_one_hour", "one_to_ten_hours", "ten_to_one_hundred_hours", "more_than_one_hundred_hours", "less_than_five_minutes", "five_to_ten_minutes", "ten_to_twenty_minutes", "twenty_to_thirty_minutes", "more_than_thirty_minutes", "less_than_ten_minutes", "ten_to_thirty_minutes", "thirty_minutes_to_one_hour", "one_to_two_hours", "more_than_two_hours", "two_to_four_hours", "four_to_six_hours", "more_than_six_hours", "less_than_two_hours", "four_to_eight_hours", "eight_to_twelve_hours", "more_than_twelve_hours", "more_than_one_hour", "custom_time"
itemCategory | audiovisual | Category. One of: "written", "audiovisual", "activity_category", "assessment", "update_category", "other_category"

> 201 Created - successful response:

```json
{
    "id": 3016,
    "title": "new item with tags",
    "shortDescription": null,
    "itemType": "Other",
    "itemCategory": "Other",
    "itemUrl": "https://examplecompany.learnamp.com/en/items/new-item-with-tags",
    "url": null,
    "description": "Short desc",
    "slug": "new-item-with-tags",
    "fileSize": null,
    "fileType": null,
    "expires": false,
    "ratingsCount": 0,
    "averageRating": 0.0,
    "goesLive": false,
    "sourceType": null,
    "sourceId": null,
    "createdAt": "2021-03-26T17:29:25Z",
    "updatedAt": "2021-03-26T17:29:25Z",
    "goesLiveAt": null,
    "image": "http://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,h_153,q_auto,w_260/v1588665654/e5pclrrfd8xyyffphich.jpg",
    "supplier": null,
    "addedBy": {
        "id": 1381,
        "firstName": "Rebecca",
        "lastName": "Bridger",
        "jobTitle": "Founder & Managing Director",
        "email": "rebecca@example.com",
        "timeZone": "London",
        "language": "en",
        "role": "owner",
        "hireDate": null,
        "profileUrl": "https://examplecompany.learnamp.com/en/users/1381",
        "status": {
            "status": "Not yet invited"
        }
    },
    "displayAddedBy": false,
    "visibility": "Entire Company",
    "price": "Free",
    "totalTime": "< 15 mins",
    "tags": []
}
```

> 400 Bad request - validation errors:

```json
{
    "error": "title is missing",
    "fullErrors": {
        "title": [
            "is missing"
        ],
    }
}
```

## Update an Item

> Update an Item:

```shell
curl --location --request PUT 'https://api.learnamp.com/v1/items/383' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--form 'title=New Item Title'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def update(id, params)
      response = self.class.put("/items/#{id}", { body: params, headers: headers })
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

item = Learnamp::Items.new(token).update(1234, params)
```

Update an Item

`POST https://{API_BASE_URL}/v1/items`

### Data in Body

Parameter (* required) | Example value | Description
--------- | ------- | -----------
title * | Sales and Marketing Guide | Title of item
url | https://www.test.com | URL to external web page for the item
description | Some description text | Description of the item
expires | true | If item must expire, set to true
expiresAt | 2022-12-31 | Expiry date when item should expire in ISO 8601 format
goesLive | true | If item should have a future go live date
goesLiveAt | 2022-12-31 | Future go live date in ISO 8601 format
imageUrl | https://sometileimageurl | URL of tile image for the item, must be publically accessible
imageUrl | https://sometileimageurl | URL of tile image for the item, must be publically accessible
visibility | entire_company | Item visibility. Must be one of "hidden", "selected", "entire_company"
sourceType | Udemy | Short string identifier of the source of the item, if is coming from an external LMS or other system
sourceId | e814koip | External identifier of item, if it if coming from an external LMS or other system
itemType | article | Item type, one of: "article", "document", "book", "q_and_a", "info_card", "video", "elearning", "infographic", "audio", "slides", "image", "project", "classroom", "coaching", "course", "event", "presentation", "meeting", "webinar", "action", "test", "qa", "announcement", "newsletter", "reminder", "website", "other"
totalTime | less_than_one_hour | Approximate time to complete. One of: "less_than_fifteen_minutes", "less_than_one_hour", "one_to_ten_hours", "ten_to_one_hundred_hours", "more_than_one_hundred_hours", "less_than_five_minutes", "five_to_ten_minutes", "ten_to_twenty_minutes", "twenty_to_thirty_minutes", "more_than_thirty_minutes", "less_than_ten_minutes", "ten_to_thirty_minutes", "thirty_minutes_to_one_hour", "one_to_two_hours", "more_than_two_hours", "two_to_four_hours", "four_to_six_hours", "more_than_six_hours", "less_than_two_hours", "four_to_eight_hours", "eight_to_twelve_hours", "more_than_twelve_hours", "more_than_one_hour", "custom_time"
itemCategory | audiovisual | Category. One of: "written", "audiovisual", "activity_category", "assessment", "update_category", "other_category"


> 200 OK - successful response:

```json
{
    "id": 3016,
    "title": "new item with tags",
    "shortDescription": null,
    "itemType": "Other",
    "itemCategory": "Other",
    "itemUrl": "https://examplecompany.learnamp.com/en/items/new-item-with-tags",
    "url": null,
    "description": "Short desc",
    "slug": "new-item-with-tags",
    "fileSize": null,
    "fileType": null,
    "expires": false,
    "ratingsCount": 0,
    "averageRating": 0.0,
    "goesLive": false,
    "sourceType": null,
    "sourceId": null,
    "createdAt": "2021-03-26T17:29:25Z",
    "updatedAt": "2021-03-26T17:29:25Z",
    "goesLiveAt": null,
    "image": "http://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,h_153,q_auto,w_260/v1588665654/e5pclrrfd8xyyffphich.jpg",
    "supplier": null,
    "addedBy": {
        "id": 1381,
        "firstName": "Rebecca",
        "lastName": "Bridger",
        "jobTitle": "Founder & Managing Director",
        "email": "rebecca@example.com",
        "timeZone": "London",
        "language": "en",
        "role": "owner",
        "hireDate": null,
        "profileUrl": "https://examplecompany.learnamp.com/en/users/1381",
        "status": {
            "status": "Not yet invited"
        }
    },
    "displayAddedBy": false,
    "visibility": "Entire Company",
    "price": "Free",
    "totalTime": "< 15 mins",
    "tags": []
}
```

> 400 Bad request - validation errors:

```json
{
    "error": "title is missing",
    "fullErrors": {
        "title": [
            "is missing"
        ]
    }
}
```

## Delete an Item

> Delete an item:

```shell
curl --location --request DELETE 'https://api.learnamp.com/v1/items/1' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Items
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def delete(id)
      response = self.class.delete("/items/#{id}", { headers: headers })
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

Learnamp::Items.new(token).delete(1)
```

Delete an Item

`DELETE https://{API_BASE_URL}/v1/items/{itemId}`


> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```
