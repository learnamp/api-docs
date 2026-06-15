# Surveys

## Survey Description

In Learn Amp, a Survey is a set of questions sent to users to gather feedback. Each time a user is asked to complete a survey, a "response" (internally an attempt) is created. A response holds an answer for every question the user submitted.

These endpoints let you list surveys, fetch a single survey's metadata, and pull all submitted responses for a survey as JSON, for example to load into a data warehouse or BI tool.

`surveyType` can be one of `standard`, `about_other`, `learning` or `one_to_one_questions`.

When a survey is `anonymous`, responses are returned without the respondent (`user`) block, but the answers are still included.

## View All Surveys

> View all surveys in your account:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/surveys' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Surveys
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(filters = {})
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/surveys?#{filters_query}", { headers: headers })
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

surveys = Learnamp::Surveys.new(token).all
```

List all non-archived, non-template surveys in your company.

`GET https://{API_BASE_URL}/v1/surveys`

### Required Scope
This endpoint requires the `surveys:read` scope.

This endpoint requires survey reporting access (reporter, designer, admin or owner). A token whose owner does not have that access receives a `401 Unauthorized`.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
{
    "surveys": [
        {
            "id": 379,
            "name": "Employee Engagement Q1",
            "description": "Quarterly engagement survey",
            "shortDescription": "Q1 engagement",
            "surveyType": "standard",
            "anonymous": false,
            "questionsCount": 8,
            "createdAt": "2026-01-05T09:00:00Z",
            "updatedAt": "2026-01-10T11:30:00Z",
            "archivedAt": null
        }
    ]
}
```

## Show a Survey

> Display details for a single survey:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/surveys/379' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Surveys
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def find(id)
      response = self.class.get("/surveys/#{id}", { headers: headers })
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

survey = Learnamp::Surveys.new(token).find(379)
```

Display metadata for one specific survey.

`GET https://{API_BASE_URL}/v1/surveys/{surveyId}`

### Required Scope
This endpoint requires the `surveys:read` scope.

> 200 OK - successful response:

```json
{
    "id": 379,
    "name": "Employee Engagement Q1",
    "description": "Quarterly engagement survey",
    "shortDescription": "Q1 engagement",
    "surveyType": "standard",
    "anonymous": false,
    "questionsCount": 8,
    "createdAt": "2026-01-05T09:00:00Z",
    "updatedAt": "2026-01-10T11:30:00Z",
    "archivedAt": null
}
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## List a Survey's Responses

> List all submitted responses for a single survey:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/surveys/379/responses' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Surveys
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def responses(id, filters = {})
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/surveys/#{id}/responses?#{filters_query}", { headers: headers })
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

responses = Learnamp::Surveys.new(token).responses(379)
```

List all submitted responses for a survey, each with the respondent and all of their answers. Only responses that have been submitted are returned, in-progress attempts are excluded.

`GET https://{API_BASE_URL}/v1/surveys/{surveyId}/responses`

### Required Scope
This endpoint requires the `surveys:read` scope.

This endpoint requires permission to view all responses for the survey, and respects one-to-one survey privacy settings. A token whose owner is not allowed to view all responses receives a `401 Unauthorized`.

When the survey is `anonymous`, the `user` block is omitted from each response, but the `answers` are still returned.

Response will be paginated [see pagination](#pagination)

Each answer carries a `value` whose shape depends on the question's `questionType`:

questionType | value
--------- | -------
free_text | The submitted text, as a string.
opinion_scale | The selected score, as an integer. An optional free-text `comment` is included alongside (`null` when not given).
radio_button | The label of the chosen option, as a string.
drop_down | The label of the chosen option, as a string.
multiple_choice | An array of the chosen option labels.
poll | An array of `{ "choice": ..., "column": ... }` objects, one per selected cell.

> 200 OK - successful response:

```json
{
    "responses": [
        {
            "id": 12,
            "deadline": "2026-02-01",
            "submittedAt": "2026-01-15T14:22:00Z",
            "startedAt": "2026-01-15T14:10:00Z",
            "createdAt": "2026-01-10T09:00:00Z",
            "user": {
                "id": 1,
                "firstName": "Test",
                "lastName": "User",
                "jobTitle": "Tester",
                "email": "test@example.com",
                "timeZone": "London",
                "language": "en",
                "role": "viewer",
                "profileUrl": "https://examplecompany.learnamp.com/en/users/1",
                "status": {
                    "status": "Confirmed",
                    "time": "On 29 Nov 16"
                }
            },
            "answers": [
                {
                    "id": 88,
                    "questionId": 4,
                    "question": "What was the biggest challenge?",
                    "questionType": "free_text",
                    "value": "Keeping focus during a busy quarter"
                },
                {
                    "id": 89,
                    "questionId": 5,
                    "question": "How satisfied are you?",
                    "questionType": "opinion_scale",
                    "value": 8,
                    "comment": "Could be better"
                },
                {
                    "id": 90,
                    "questionId": 6,
                    "question": "Preferred language?",
                    "questionType": "radio_button",
                    "value": "Ruby"
                },
                {
                    "id": 91,
                    "questionId": 7,
                    "question": "Which tools do you use?",
                    "questionType": "multiple_choice",
                    "value": ["AWS", "Serverless", "Ruby on Rails"]
                },
                {
                    "id": 92,
                    "questionId": 8,
                    "question": "Rate your confidence per area",
                    "questionType": "poll",
                    "value": [
                        { "choice": "Ruby", "column": "average" },
                        { "choice": "JS", "column": "weak" }
                    ]
                }
            ]
        }
    ]
}
```

For an anonymous survey, the `user` block is omitted:

```json
{
    "responses": [
        {
            "id": 12,
            "deadline": "2026-02-01",
            "submittedAt": "2026-01-15T14:22:00Z",
            "startedAt": "2026-01-15T14:10:00Z",
            "createdAt": "2026-01-10T09:00:00Z",
            "answers": [
                {
                    "id": 88,
                    "questionId": 4,
                    "question": "What was the biggest challenge?",
                    "questionType": "free_text",
                    "value": "Keeping focus during a busy quarter"
                }
            ]
        }
    ]
}
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```
