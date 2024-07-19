# Tasks

## Task Description

In Learn Amp, a Task is a piece of learning content that a user is given a specific deadline to complete.

A task has an assigner - the person who set the task. It also has a setFor user - the person required to complete the task.

Tasks may be mandatory. Tasks always have a deadline, and are associated to a specific learning object, referred to as "taskable" in json reponses.

Tasks may be completed by the user, or they may still be pending/overdue.

Tasks may be expired, for example, if the same user is required to complete the same content every year. When the new task is assigned for the same user, their previous task to complete the same content is marked as expired.

## View All Tasks

> View all tasks in your account:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/tasks' \
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
      response = self.class.get("/tasks?#{filters_query}", { headers: headers })
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
  "filters[assigned_at][from]" => "2021-12-31",
  "filters[assigned_at][to]" => "2022-02-28",
  "filters[completed_at][from]" => "2021-12-31",
  "filters[completed_at][to]" => "2022-02-28",
  "filters[deadline][from]" => "2021-12-31",
  "filters[deadline][to]" => "2022-02-28",
  "filters[created_at][from]" => "2021-12-31",
  "filters[created_at][to]" => "2022-02-28",
  "filters[update_at][from]" => "2021-12-31",
  "filters[update_at][to]" => "2022-02-28",
  "filters[id]" => 2456,
  "filters[user_id]" => 1,
  "filters[taskable_type]" => "Item,Channel,Learnlist,Quiz",
  "filters[taskable_id]" => 99874,
  "filters[status]" => "completed",
  "filters[lifecycle]" => "active,deleted,deactivated",
}
tasks = Learnamp::Tasks.new(token).all(filters)
```

View all tasks

`GET https://api.learnamp.com/v1/tasks`

Response will be paginated [see pagination](#pagination)


### Optional Filters in URL Params

The following URL params by be included, to filter the result set:

`GET https://api.learnamp.com/v1/tasks?filters[deadline][from]=2021-01-01&filters[deadline][to]=2021-06-01`

URL Param | Example Value | Description
--------- | ------- | -----------
expanded | true | Optional expanded data. Will include related exercise submissions, and awarded certificates. Using this option is NOT recommended, unless required.
filters[assigned_at][from] | "2021-12-31" | Assigned date range FROM date in ISO 8601 format
filters[assigned_at][to] | "2022-02-28" | Assigned date range TO date in ISO 8601 format
filters[completed_at][from] | "2021-12-31" | Completion date range FROM date in ISO 8601 format
filters[completed_at][to] | "2022-02-28" | Completion date range TO date in ISO 8601 format
filters[deadline][from] | "2021-12-31" | Deadline date range FROM date in ISO 8601 format
filters[deadline][to] | "2022-02-28" | Deadline date range TO date in ISO 8601 format
filters[created_at][from] | "2021-12-31" | Created at date range FROM date in ISO 8601 format. Tasks may be created before they are actually assigned to the user.
filters[created_at][to] | "2022-02-28" | Created at date range TO date in ISO 8601 format
filters[update_at][from] | "2021-12-31" | Updated at date range FROM date in ISO 8601 format
filters[update_at][to] | "2022-02-28" | Updated at date range TO date in ISO 8601 format
filters[id] | 2456 | ID of specific task
filters[user_id] | 78823 | User ID of person assigned the task
filters[taskable_type] | "Item,Channel,Learnlist,Quiz" | Type of learning object. Can be single value, or comma seperated list of Task types: any of Item,Channel,Learnlist,Quiz
filters[taskable_id] | 99874 | ID of specific learning object
filters[status] | "completed" | Task status. One of: completed / overdue / incomplete
filters[lifecycle] | "active" | Lifecycle status of task. Can be single value, or comma seperated list of Task lifecycle values: active / deleted / deactivated. When tasks are deleted via the UI, they are given a lifecycle of 'deleted'. When a user is deactivated, all their tasks are given a lifecycle of 'deactivated'. By default the API will only return 'active' lifecycle tasks.

> 200 OK - successful response:

```json
{
    "tasks": [
        {
            "id": 4428,
            "taskableId": 634,
            "taskableType": "Item",
            "lifecycle": "active",
            "createdAt": "2021-02-24T15:52:59Z",
            "deadline": "2021-02-28",
            "expiredAt": null,
            "assignedAt": "2021-02-24T15:52:59Z",
            "completedAt": null,
            "name": "Landing Page Design & Web Design Fundamentals",
            "taskable": {
                "id": 634,
                "name": "Landing Page Design & Web Design Fundamentals",
                "shortDescription": null,
                "type": "Item",
                "url": "https://testaccount.learnamp.com/en/items/landing-page-design",
                "addedBy": {
                    "id": 1,
                    "firstName": "Test",
                    "lastName": "User",
                    "jobTitle": "Tester",
                    "email": "test@example.com",
                    "timeZone": "London",
                    "language": "en",
                    "role": "viewer",
                    "profileUrl": "https://testaccount.learnamp.com/en/users/1",
                    "status": {
                        "status": "Confirmed",
                        "time": "On 29 Nov 16"
                    }
                },
                "displayAddedBy": true,
                "totalTimeEstimate": "1-10 hrs"
            },
            "typeLabel": "Course",
            "overdue": true,
            "mandatory": false,
            "completed": false,
            "expired": false,
            "setFor": {
                "id": 123,
                "firstName": "Andy",
                "lastName": "Jones",
                "jobTitle": "Customer Success Executive",
                "email": "andy@example.com",
                "timeZone": "London",
                "language": "en",
                "role": "viewer",
                "profileUrl": "https://testaccount.learnamp.com/en/users/123",
                "status": {
                    "status": "Confirmed",
                    "time": "On 15 Oct 20"
                }
            },
            "url": "https://testaccount.learnamp.com/en/items/landing-page-design-web-design-fundamentals-2017",
            "assigner": {
                "id": 1,
                "firstName": "Admin",
                "lastName": "User",
                "jobTitle": "Boss",
                "email": "adminuser@example.com",
                "timeZone": "London",
                "language": "en",
                "role": "admin",
                "profileUrl": "https://testaccount.learnamp.com/en/users/1",
                "status": {
                    "status": "Confirmed",
                    "time": "On 29 Nov 16"
                }
            }
        }
    ]
}
```

> 200 OK - successful response, woth optional expanded=true param

```json
{
    "tasks": [
        {
            "id": 4169,
            "taskableId": 2958,
            "taskableType": "Item",
            "lifecycle": "active",
            "createdAt": "2020-08-10T13:38:50Z",
            "deadline": "2020-08-21",
            "expiredAt": null,
            "assignedAt": "2020-08-10T13:38:50Z",
            "completedAt": "2020-08-10T13:39:01Z",
            "name": "Marketing 101",
            "taskable": {
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
            "typeLabel": "Video",
            "overdue": true,
            "mandatory": true,
            "completed": true,
            "expired": false,
            "setFor": {
                "id": 1,
                "firstName": "Test",
                "lastName": "User",
                "jobTitle": "Tester",
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
            "url": "https://examplecompany.learnamp.com/en/items/dani",
            "assigner": {
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
            "certificate": {
                "id": 58,
                "awardedAt": "2020-08-10T13:39:01.724Z",
                "expiresAt": null,
                "certificateUrl": "https://examplecompany.learnamp.com/en/certificates/58"
            },
            "score": 100,
            "totalTime": 180
        }
    ]
}
```