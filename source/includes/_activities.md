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

`GET https://api.learnamp.com/v1/activities`

Response will be paginated [see pagination](#pagination)


### Optional Filters in URL Params

The following URL params by be included, to filter the result set:

`GET https://api.learnamp.com/v1/activities?expanded=true&include_deactivated_users=true&filters[date][from]=2021-01-01&filters[date][to]=2021-06-01`

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
