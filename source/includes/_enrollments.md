# Enrollments

## Enrollment Description

In Learn Amp, an Enrollment is the association between a user and an event session. Users may be enrolled into various event sessions. Their enrollment may be marked as attended or not attended after the event session has ended. If an enrollment is set to "attended", an activity record will be created, in which the user is marked as having completed the associated event.

## View All Enrollments

> View all event session enrollments in your account:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/enrollments' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Enrollments
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(filters)
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/enrollments?#{filters_query}", { headers: headers })
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
  "filters[event_id]" => 4,
  "filters[event_session_id]" => 2,
  "filters[attendance]" => "attended"
}
tasks = Learnamp::Enrollments.new(token).all(filters)
```

View all events

`GET https://{API_BASE_URL}/v1/enrollments`

### Required Scope
This endpoint requires the `enrollments:read` scope.

Response will be paginated [see pagination](#pagination)


### Optional Filters in URL Params

The following URL params by be included, to filter the result set:

`GET https://{API_BASE_URL}/v1/enrollments?filters[created_at][from]=2021-01-01&filters[created_at][to]=2021-06-01`

URL Param | Example Value | Description
--------- | ------- | -----------
filters[event_id] | 4 | ID of specific event
filters[status] | approved | Status of enrollment. Must be one of "pending", "approved", "rejected", "unenrolled"
filters[created_at][from] | 2020-06-01 | Enrollment creation date range FROM in ISO 8601 date format
filters[created_at][to] | 2020-07-01 | Enrollment creation date range TO in ISO 8601 date format
filters[event_session_starts_at][from] | 2020-05-01 | Event session START date range FROM in ISO 8601 date format
filters[event_session_starts_at][to] | 2020-07-01 | Event session START date range TO in ISO 8601 date format
filters[event_session_ends_at][from] | 2020-05-01 | Event session END date range FROM in ISO 8601 date format
filters[event_session_ends_at][to] | 2020-07-01 | Event session END date range TO in ISO 8601 date format
filters[event_session_id]| 71 | ID of a specific event session
filters[enrolled_at][from] | 2020-05-01 | Date when enrollent status was set to "approved", range FROM value in ISO 8601 date format
filters[enrolled_at][to] | 2020-06-10 | Date when enrollent status was set to "approved", range TO value in ISO 8601 date format
filters[attendance] | attended | Attendance setting, one of "marked_not_yet", "attended", "did_not_attend"
filters[updated_at][from] | 2020-04-01 | Enrollment updated date range FROM in ISO 8601 date format
filters[updated_at][to] | 2020-04-01 | Enrollment updated date range TO in ISO 8601 date format

> 200 OK - successful response:

```json
{
    "enrollments": [
        {
            "id": 52,
            "createdAt": "2019-05-06T09:28:52.079Z",
            "updatedAt": "2019-05-07T07:01:42.016Z",
            "enrolledAt": null,
            "status": "pending",
            "attendance": "Attended",
            "isHost": false,
            "user": {
                "id": 7,
                "firstName": "John",
                "lastName": "Test",
                "jobTitle": "admin & ceo",
                "email": "admin@example.com",
                "timeZone": "London",
                "language": "en",
                "role": "super_admin",
                "hireDate": null,
                "profileUrl": "https://examplecompany.learnamp.com/en/users/7",
                "status": {
                    "status": "Not yet invited"
                }
            },
            "enrolledBy": {
                "id": 7,
                "firstName": "John",
                "lastName": "Test",
                "jobTitle": "admin & ceo",
                "email": "admin@example.com",
                "timeZone": "London",
                "language": "en",
                "role": "super_admin",
                "hireDate": null,
                "profileUrl": "https://examplecompany.learnamp.com/en/users/7",
                "status": {
                    "status": "Not yet invited"
                }
            },
            "eventSession": {
                "id": 5,
                "createdAt": "2018-12-18T13:15:26.401Z",
                "updatedAt": "2019-05-06T09:25:48.325Z",
                "startsAt": "2019-05-12T12:05:00.000Z",
                "endsAt": "2019-05-12T13:05:00.000Z",
                "locationType": "plain_text",
                "locationAddress": "france",
                "host": null,
                "enrollmentsCount": 1,
                "enrollmentType": "admin_approved",
                "enrollmentDeadline": null,
                "spaces": 30,
                "event": {
                    "id": 4,
                    "name": "event in france",
                    "shortDescription": null,
                    "createdAt": "2018-12-18T13:15:26.381Z",
                    "updatedAt": "2019-05-06T09:25:48.354Z",
                    "eventUrl": "https://examplecompany.learnamp.com/en/events/d3b96c0b-1665-4e6f-9b8d-5bdfadcd0807",
                    "eventSessionsCount": 1
                }
            }
        }
    ]
}
```

## Show an Event and it's Event Sessions

> Display details for a single event:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/events/1' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Events
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def find(id)
      response = self.class.get("/events/#{id}", { headers: headers })
      response.parsed_response if response.ok?
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

event = Learnamp::Events.new(token).find(1)
```

Display user details for one specific event.

`GET https://{API_BASE_URL}/v1/events/{eventId}`

### Required Scope
This endpoint requires the `events:read` scope.

> 200 OK - successful response:

```json
{
    "enrollments": [
        {
            "id": 52,
            "createdAt": "2019-05-06T09:28:52.079Z",
            "updatedAt": "2019-05-07T07:01:42.016Z",
            "enrolledAt": null,
            "status": "pending",
            "attendance": "Attended",
            "isHost": false,
            "user": {
                "id": 7,
                "firstName": "John",
                "lastName": "Test",
                "jobTitle": "admin & ceo",
                "email": "admin@example.com",
                "timeZone": "London",
                "language": "en",
                "role": "super_admin",
                "hireDate": null,
                "profileUrl": "https://examplecompany.learnamp.com/en/users/7",
                "status": {
                    "status": "Not yet invited"
                }
            },
            "enrolledBy": {
                "id": 7,
                "firstName": "John",
                "lastName": "Test",
                "jobTitle": "admin & ceo",
                "email": "admin@example.com",
                "timeZone": "London",
                "language": "en",
                "role": "super_admin",
                "hireDate": null,
                "profileUrl": "https://examplecompany.learnamp.com/en/users/7",
                "status": {
                    "status": "Not yet invited"
                }
            },
            "eventSession": {
                "id": 5,
                "createdAt": "2018-12-18T13:15:26.401Z",
                "updatedAt": "2019-05-06T09:25:48.325Z",
                "startsAt": "2019-05-12T12:05:00.000Z",
                "endsAt": "2019-05-12T13:05:00.000Z",
                "locationType": "plain_text",
                "locationAddress": "france",
                "host": null,
                "enrollmentsCount": 1,
                "enrollmentType": "admin_approved",
                "enrollmentDeadline": null,
                "spaces": 30,
                "event": {
                    "id": 4,
                    "name": "event in france",
                    "shortDescription": null,
                    "createdAt": "2018-12-18T13:15:26.381Z",
                    "updatedAt": "2019-05-06T09:25:48.354Z",
                    "eventUrl": "https://examplecompany.learnamp.com/en/events/d3b96c0b-1665-4e6f-9b8d-5bdfadcd0807",
                    "eventSessionsCount": 1
                }
            }
        }
    ]
}
```

## Show an Enrollment

> Show a specific enrollment:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/enrollments/52' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Enrollments
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def find(id)
      response = self.class.get("/enrollments/#{id}", { headers: headers })
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

enrollment = Learnamp::Enrollments.new(token).find(52)
```

Shows details of an enrollment.

`GET https://{API_BASE_URL}/v1/enrollments/{id}`

### Required Scope
This endpoint requires the `enrollments:read` scope.

> 200 OK - successful response:

```json
{
    "id": 52,
    "createdAt": "2019-05-06T09:28:52.079Z",
    "updatedAt": "2019-05-07T07:01:42.016Z",
    "enrolledAt": null,
    "status": "approved",
    "attendance": "Attended",
    "isHost": false,
    "user": {
        "id": 7,
        "firstName": "John",
        "lastName": "Test",
        "jobTitle": "admin & ceo",
        "email": "admin@example.com",
        "timeZone": "London",
        "language": "en",
        "role": "super_admin",
        "hireDate": null,
        "profileUrl": "https://examplcompany.learnamp.com/en/users/7",
        "status": {
            "status": "Not yet invited"
        }
    },
    "enrolledBy": {
        "id": 7,
        "firstName": "John",
        "lastName": "Test",
        "jobTitle": "admin & ceo",
        "email": "admin@example.com",
        "timeZone": "London",
        "language": "en",
        "role": "super_admin",
        "hireDate": null,
        "profileUrl": "https://examplcompany.learnamp.com/en/users/7",
        "status": {
            "status": "Not yet invited"
        }
    },
    "eventSession": {
        "id": 5,
        "createdAt": "2018-12-18T13:15:26.401Z",
        "updatedAt": "2019-05-06T09:25:48.325Z",
        "startsAt": "2019-05-12T12:05:00.000Z",
        "endsAt": "2019-05-12T13:05:00.000Z",
        "locationType": "plain_text",
        "locationAddress": "france",
        "host": null,
        "enrollmentsCount": 1,
        "enrollmentType": "admin_approved",
        "enrollmentDeadline": null,
        "spaces": 30,
        "event": {
            "id": 4,
            "name": "event in france",
            "shortDescription": null,
            "createdAt": "2018-12-18T13:15:26.381Z",
            "updatedAt": "2019-05-06T09:25:48.354Z",
            "eventUrl": "https://examplcompany.learnamp.com/en/events/d3b96c0b-1665-4e6f-9b8d-5bdfadcd0807",
            "eventSessionsCount": 1
        }
    }
}
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Create an Enrollment

> Create a new enrollment:

```shell
curl --location --request POST 'https://api.learnamp.com/v1/enrollments' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
  "user_id": 1,
  "event_name": "Leadership workshop",
  "event_session_id": 5
}'
```

```ruby
module Learnamp
  class Enrollments
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def create(params)
      response = self.class.post("/enrollments", { body: params, headers: headers })
      response.parsed_response
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}",
        'Content-Type' => 'application/json'
      }
    end
  end
end

params = {
  user_id: 1,
  event_name: "Leadership workshop",
  event_session_id: 5
}

enrollments = Learnamp::Enrollments.new(token).create(params)
```

Creates a new enrollment. This will enrol a given user into an event session. By default, the new enrollment will have a status of 'approved'.

`POST https://{API_BASE_URL}/v1/enrollments`

### Required Scope
This endpoint requires the `enrollments:create` scope.
