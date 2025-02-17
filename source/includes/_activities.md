# Activities

## Activity Description

In Learn Amp, an Activity describes an event that takes place on the platform. For example, User X completed Item Y, or User A started Learnlist B.
All activity records are associated to a User. They contain a [verb](#verbs), like "logged in", "watched" etc. They have a datetime for when the activity took place.

## View All Activity

> View all activity in your account:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/activities' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Tasks
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(filters)
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/activities?#{filters_query}", { headers: headers })
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
  "filters[date][from]" => "2021-12-31",
  "filters[date][to]" => "2022-02-28",
  "filters[user_id]" => 1,
  "filters[completed]" => true,
  "filters[activityable_type]" => "Item,Channel,Learnlist,Quiz",
  "filters[activityable_id]" => 99874
}
activities = Learnamp::Activities.new(token).all(filters)
```

View all activity

`GET https://{API_BASE_URL}/v1/activities`

Response will be paginated [see pagination](#pagination)


### Optional Filters in URL Params

The following URL params by be included, to filter the result set:

`GET https://{API_BASE_URL}/v1/activities?expanded=true&include_deactivated_users=true&filters[date][from]=2021-01-01&filters[date][to]=2021-06-01`

URL Param | Example Value | Description
--------- | ------- | -----------
expanded | true | Optional expanded json. Returns certificate and any related exercise submissions as nested data in the response. Recommend not using this option unless required.
include_deactivated_users | true | Optional - default is false. When true activity returned by the end-point will include activity by deactivated users.
filters[date][from] | "2021-12-31" | Date range FROM date in ISO 8601 format
filters[date][to] | "2022-02-28" | Date range TO date in ISO 8601 format
filters[timezone] | "UTC" | Specify Timezone by which to filter activites. See [timezones](#time-zones).
filters[user_id] | 1 | User ID of user who performed the activity
filters[team_id] | 78823 | Filter activities to users within a particular team, specified by team_id
filters[activityable_type] | "Item,Channel,Learnlist,Quiz" | Type of learning object. Can be single value, or comma seperated list of types: any of Item,Channel,Learnlist,Quiz
filters[activityable_id] | 99874 | ID of specific learning object
filters[completed] | true | Only return completion activity. Useful if you only need to see what learning objects have been completed
filters[verb] | "started" | Return activity for a specific verb. See [verbs](#verbs).

> 200 OK - successful response:

```json
{
    "activities": [
        {
            "id": 26338,
            "activityable": {
                "id": 461,
                "name": "User Added Content",
                "shortDescription": null,
                "type": "Item",
                "url": "https://examplecompany.learnamp.com/en/items/user-added-content",
                "addedBy": {
                    "id": 7,
                    "firstName": "Test",
                    "lastName": "User",
                    "jobTitle": "admin & ceo",
                    "email": "admin@example.com",
                    "timeZone": "London",
                    "language": "en",
                    "role": "viewer",
                    "hireDate": null,
                    "profileUrl": "https://examplecompany.learnamp.com/en/users/7",
                    "status": {
                        "status": "Not yet invited"
                    }
                },
                "displayAddedBy": null,
                "totalTimeEstimate": "< 1 hr"
            },
            "user": {
                "id": 7,
                "firstName": "Test",
                "lastName": "User",
                "jobTitle": "admin & ceo",
                "email": "admin@example.com",
                "timeZone": "London",
                "language": "en",
                "role": "admin",
                "hireDate": null,
                "profileUrl": "https://examplecompany.learnamp.com/en/users/7",
                "status": {
                    "status": "Not yet invited"
                }
            },
            "verb": "updated",
            "createdAt": "2017-03-21T15:39:03Z",
            "expiredAt": null,
            "result": "",
            "completed": false,
            "expired": false,
            "score": null,
            "totalTime": null
        }
    ]
}
```


> 200 OK - successful response:, with expanded=true param

```json
{
    "activities": [
        {
            "id": 29272,
            "activityable": {
                "id": 2958,
                "name": "Marketing 101",
                "shortDescription": null,
                "type": "Item",
                "url": "https://examplecompany.learnamp.com/en/items/dani",
                "addedBy": {
                    "id": 1,
                    "jobTitle": "TV Presenter & Personality",
                    "firstName": "Keith",
                    "lastName": "Chegwin",
                    "email": null,
                    "timeZone": null,
                    "language": null,
                    "role": null,
                    "profileUrl": null,
                    "status": {
                        "status": "External Contributor",
                        "time": null
                    }
                },
                "displayAddedBy": true,
                "totalTimeEstimate": "< 10 mins",
                "tags": []
            },
            "user": {
                "id": 1,
                "firstName": "Test",
                "lastName": "User",
                "jobTitle": "CTO",
                "email": "test@example.com",
                "timeZone": "London",
                "language": "en",
                "role": "viewer",
                "hireDate": "2017-02-01",
                "profileUrl": "https://examplecompany.learnamp.com/en/users/1",
                "status": {
                    "status": "Confirmed",
                    "time": "On 29 Nov 16"
                }
            },
            "verb": "updated",
            "createdAt": "2020-08-10T13:38:21Z",
            "expiredAt": null,
            "result": "",
            "completed": false,
            "expired": false,
            "score": null,
            "totalTime": null,
            "certificate": {
                "id": 58,
                "awardedAt": "2020-08-10T13:39:01.724Z",
                "expiresAt": null,
                "certificateUrl": "https://examplecompany.learnamp.com/en/certificates/58"
            }
        }
    ]
}
```

## Create an Activity

##### NOTE: :construction: This endpoint is currently in beta and requires special access. Please raise a ticket on our [Customer Portal](https://learnamp.atlassian.net/servicedesk/customer/portal/1/user/login?destination=portal%2F1) to request access
----

Create an activty with a related [Item](#items) record and [Verb](#verbs). Please check to get an appropriate ID assigned to a specific verb.

> Create a Activity record with an Item and Verb

```shell
curl --location 'hhttps://testaccount.learnamp.com/v1/activities' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer a3DWMdP8mmFRtvulvanOGgII7U1dVV8LZ9zr6jyCq4k' \
--header 'Cookie: _learnamp_session_=aadc48937fec7520fd7cf3f7e55681e4' \
--form 'user_id="1"' \
--form 'verb_id="54"' \
--form 'item_id="10"'
```

```ruby
module Learnamp
  class Activities
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def create(params)
      response = self.class.post("/activities", { body: params, headers: headers })
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

params_with_item_id = {
    user_id: '1',
    verb_id: '54'
    item_id: '10',
}

item_activity = Learnamp::Activities.new(token).create(params_with_item_id)

params_with_external_id_item = {
    user_id: '1',
    verb_id: '54'
    external_id: 'content_1234567890abcdef',
}

external_item_activity = Learnamp::Activities.new(token).create(params_with_external_id_item)

params_with_slug = {
    user_id: '1',
    verb_id: '54'
    slug: 'content-slug-for-activity',
}

activity = Learnamp::Activities.new(token).create(params_with_slug)
```

### Data in Body

**Note:** The following parameters are mutually exclusive. It means you can only use one of other parameters to be a valid request.
- For the User the Activity belongs to:
  - `user_id`
  - `email`
- For the Item the Activity belongs to:
  - `slug`
  - `item_id`
  - `external_id`

Parameter | Example value | Description (* required)
--------- | ------- | -----------
user_id | 1 | ID of the User. 
email | testuser@test.com | Email address of user.
verb_id | 54 | ID of the [Verb](#verbs) for this activity. *(\*)*
item_id | 1 | ID of the Item that is created within the Learn Amp platform.
slug | 'content-slug-for-activity' | Slug of an existing Item in the platform. 
external_id | 1 or 'content_1234567890abcdef' | `source_id` of the Item that is in the Learn Amp platform but was created through the [Items](#items) create endpoint.

> 201 Created - succesful response:

```json
{
    "id": 225,
    "activityable": {
        "id": 10,
        "name": "Content",
        "shortDescription": null,
        "type": "Item",
        "url": "hhttps://testaccount.learnamp.com/en/items/content-slug-for-activity",
        "addedBy": {
            "id": 1,
            "firstName": "First Name",
            "lastName": "Last Name",
            "jobTitle": "CTO",
            "email": "firstlast.lastname@learnamp.com",
            "timeZone": "London",
            "language": "en",
            "role": "viewer",
            "hireDate": null,
            "profileUrl": "hhttps://testaccount.learnamp.com/en/users/1",
            "status": {
                "status": "Confirmed",
                "time": "On 4 Apr 23"
            }
        },
        "displayAddedBy": false,
        "totalTimeEstimate": "< 5 mins"
    },
    "user": {
        "id": 1,
        "firstName": "First Name",
        "lastName": "Last Name",
        "jobTitle": "Last Name",
        "email": "firstlast.lastname@learnamp.com",
        "timeZone": "London",
        "language": "en",
        "role": "viewer",
        "hireDate": null,
        "profileUrl": "hhttps://testaccount.learnamp.com/en/users/1",
        "status": {
            "status": "Confirmed",
            "time": "On 4 Apr 23"
        }
    },
    "verb": "started",
    "createdAt": "2025-02-17T16:08:29Z",
    "expiredAt": null,
    "result": "",
    "completed": false,
    "expired": false,
    "score": null,
    "totalTime": null
}
```