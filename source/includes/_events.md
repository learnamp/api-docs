# Events

## Event Description

In Learn Amp, an Event is a specific type of learning object. Events have event sessions. For example the same event "Annual Christmas Party", could have multiple event sessions, one for each year.

Users are enroled to event sessions. Event sessions may have different enrolment rules - for example if space is limited, or if it is a closed event in which the user must be invited by an admin.

After an event session has been completed, the user's attendance is marked. If they attended the event session, then the event will be marked as completed for that user.

## View All Events

> View all events in your account:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/events' \
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

    def all(filters)
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/events?#{filters_query}", { headers: headers })
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
  "filters[title]" => "Christmas Party",
  "filters[created_at][from]" => "2021-12-31",
  "filters[created_at][to]" => "2022-02-28",
}
tasks = Learnamp::Events.new(token).all(filters)
```

View all events

`GET https://{API_BASE_URL}/v1/events`

Response will be paginated [see pagination](#pagination)


### Optional Filters in URL Params

The following URL params by be included, to filter the result set:

`GET https://{API_BASE_URL}/v1/events?filters[created_at][from]=2021-01-01&filters[created_at][to]=2021-06-01`

URL Param | Example Value | Description
--------- | ------- | -----------
filters[title] | "Christmas Party" | Search event title
filters[created_at][from] | "2021-12-31" | Created at date range FROM date in ISO 8601 format.
filters[created_at][to] | "2022-02-28" | Created at date range TO date in ISO 8601 format

> 200 OK - successful response:

```json
{
    "events": [
        {
            "id": 75,
            "name": "some incoming event",
            "shortDescription": null,
            "createdAt": "2019-09-20T11:00:26.464Z",
            "updatedAt": "2019-09-20T11:02:21.012Z",
            "eventUrl": "https://examplecompany.learnamp.com/en/events/some-incoming-event",
            "eventSessionsCount": 1
        },
        {
            "id": 74,
            "name": "not yet cancelled event",
            "shortDescription": null,
            "createdAt": "2019-09-09T10:24:23.755Z",
            "updatedAt": "2019-09-09T10:28:45.997Z",
            "eventUrl": "https://examplecompany.learnamp.com/en/events/not-yet-cancelled-event",
            "eventSessionsCount": 3
        },
        {
            "id": 73,
            "name": "event enrolled by manager",
            "shortDescription": null,
            "createdAt": "2019-09-06T13:36:17.667Z",
            "updatedAt": "2019-09-06T13:36:32.801Z",
            "eventUrl": "https://examplecompany.learnamp.com/en/events/event-enrolled-by-manager",
            "eventSessionsCount": 0
        },
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


> 200 OK - successful response:

```json
{
    "id": 75,
    "name": "some incoming event",
    "shortDescription": null,
    "createdAt": "2019-09-20T11:00:26.464Z",
    "updatedAt": "2019-09-20T11:02:21.012Z",
    "eventUrl": "https://examplecompany.learnamp.com/en/events/some-incoming-event",
    "createdBy": {
        "id": 718,
        "firstName": "Test",
        "lastName": "User",
        "jobTitle": "Sales Director",
        "email": "viewer@example.com",
        "timeZone": "London",
        "language": "en",
        "role": "viewer",
        "hireDate": null,
        "profileUrl": "https://examplecompany.learnamp.com/en/users/718",
        "status": {
            "status": "Confirmed",
            "time": "On 23 Mar 18"
        }
    },
    "visibility": "Entire Company",
    "ratingsCount": 0,
    "averageRating": 0.0,
    "expiresAt": null,
    "goesLiveAt": null,
    "eventSessions": [
        {
            "id": 56,
            "createdAt": "2019-09-20T11:02:20.778Z",
            "updatedAt": "2019-09-20T11:02:20.778Z",
            "startsAt": "2019-09-20T12:00:00.000Z",
            "endsAt": "2019-09-20T13:00:00.000Z",
            "locationType": "plain_text",
            "locationAddress": "Gliwice, HQ, akwarium",
            "host": null,
            "eventId": 75,
            "enrollmentsCount": 0,
            "enrollmentType": "user",
            "enrollmentDeadline": null
        }
    ],
    "tags": []
}
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```
