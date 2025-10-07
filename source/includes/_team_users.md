# Team Users

## View All Team Users

> View all users for a given team:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/teams/1/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class TeamUsers
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(team_id)
      response = self.class.get("/teams/#{team_id}/users", { headers: headers })
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

team_users = Learnamp::TeamUsers.new(token).all(1)
```

View all users for a given team.

### HTTP Request

`GET https://{API_BASE_URL}/v1/teams/{team_id}/users`

### Required Scope
This endpoint requires the `team_users:read` scope.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
{
  "id": 379,
  "name": "Test Team",
  "teamUsersCount": 2,
  "apiTeamPath": "/v1/teams/379.json",
  "apiTeamUsersPath": "/v1/teams/379/users.json",
  "manager": {
      "id": 7,
      "firstName": "Test",
      "lastName": "Manager",
      "jobTitle": "Ops Director",
      "email": "admin@example.com",
      "timeZone": "London",
      "language": "en",
      "role": "admin",
      "profileUrl": "http://testaccount.learnamp.com/en/users/7",
      "status": {
          "status": "Invite pending",
          "time": "Sent 25 Sep 19"
      }
  },
  "users": [
    {
      "id": 1,
      "firstName": "Test",
      "lastName": "User",
      "jobTitle": "Developer",
      "email": "test@email.com",
      "timeZone": "London",
      "language": "en",
      "role": "viewer",
      "profileUrl": "https://testaccount.learnamp.com/en/users/1",
      "status": {
          "status": "Confirmed",
          "time": "On 29 Nov 16"
      },
      "avatar": "AVATAR_IMAGE_URL",
      "manager": {
          "id": 17,
          "firstName": "Manager",
          "lastName": "User",
          "jobTitle": "Head of Ops",
          "email": "testuser2@email.com",
          "timeZone": "New York",
          "language": "en-US",
          "role": "admin",
          "profileUrl": "https://testaccount.learnamp.com/en/users/17",
          "status": {
              "status": "Confirmed",
              "time": "On 20 Feb 17"
          }
      },
      "location": "London, UK",
      "primaryTeam": {
        "id": 15,
        "name": "Operations",
        "teamUsersCount": 10,
        "apiTeamPath": "/v1/teams/15.json",
        "apiTeamUsersPath": "/v1/teams/15/users.json",
        "manager": {
          "id": 17,
          "firstName": "Manager",
          "lastName": "User",
          "jobTitle": "Head of Ops",
          "email": "testuser2@email.com",
          "timeZone": "New York",
          "language": "en-US",
          "role": "admin",
          "profileUrl": "https://testaccount.learnamp.com/en/users/17",
          "status": {
            "status": "Confirmed",
            "time": "On 20 Feb 17"
          }
        }
      },
      "secondaryTeams": []
    }
  ]
}
```

## Add a User to a Team

> Add a user to a team:

```shell
curl --location --request POST 'https://api.learnamp.com/v1/teams/1/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
  "userId": 1,
  "isPrimary": true
}'
```

```ruby
module Learnamp
  class TeamUsers
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def add(team_id, user_details)
      response = self.class.post("/teams/#{team_id}/users",
        {
          headers: headers,
          body: user_details.to_json
        }
      )
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

user_details = {
  userId: 1,
}

result = Learnamp::TeamUsers.new(token).add(1, user_details)
```

Add a user to a team.

### HTTP Request

`POST https://{API_BASE_URL}/v1/teams/{team_id}/users`

### Required Scope
This endpoint requires the `team_users:create` scope.

### Data in Body

Parameter | Example value | Description
--------- | ------- | -----------
userId | 1 | User ID of user to add to team
response | Teams::Simple | Optional: (blank value is the default) or "Teams::Simple" or "Teams::Extended". Value of "Teams::Simple" will include basic team information in the response. Value of "Teams::Extended: will include full team information in the response, including full list of users, sub-teams and tags in the JSON response. Only use the "Teams::Extended" response if all this data is required.

> 201 Created - successful response:

```json
{
  "id": 1,
  "teamId": 3,
  "userId": 1
}
```

> 201 Created - successful response, using response = Teams::Extended param

```json
{
  "id": 1,
  "name": "Test Team",
  "teamUsersCount": 2,
  "apiTeamPath": "/v1/teams/1.json",
  "apiTeamUsersPath": "/v1/teams/1/users.json",
  "manager": {
      "id": 7,
      "firstName": "Test",
      "lastName": "Manager",
      "jobTitle": "Ops Director",
      "email": "admin@example.com",
      "timeZone": "London",
      "language": "en",
      "role": "admin",
      "profileUrl": "http://testaccount.learnamp.com/en/users/7",
      "status": {
          "status": "Invite pending",
          "time": "Sent 25 Sep 19"
      }
  },
  "users": [
    {
      "id": 1,
      "firstName": "Test",
      "lastName": "User",
      "jobTitle": "Developer",
      "email": "test@email.com",
      "timeZone": "London",
      "language": "en",
      "role": "viewer",
      "profileUrl": "https://testaccount.learnamp.com/en/users/1",
      "status": {
          "status": "Confirmed",
          "time": "On 29 Nov 16"
      },
      "avatar": "AVATAR_IMAGE_URL",
      "manager": {
          "id": 17,
          "firstName": "Manager",
          "lastName": "User",
          "jobTitle": "Head of Ops",
          "email": "testuser2@email.com",
          "timeZone": "New York",
          "language": "en-US",
          "role": "admin",
          "profileUrl": "https://testaccount.learnamp.com/en/users/17",
          "status": {
              "status": "Confirmed",
              "time": "On 20 Feb 17"
          }
      },
      "location": "London, UK",
      "primaryTeam": {
        "id": 15,
        "name": "Operations",
        "teamUsersCount": 10,
        "apiTeamPath": "/v1/teams/15.json",
        "apiTeamUsersPath": "/v1/teams/15/users.json",
        "manager": {
          "id": 17,
          "firstName": "Manager",
          "lastName": "User",
          "jobTitle": "Head of Ops",
          "email": "testuser2@email.com",
          "timeZone": "New York",
          "language": "en-US",
          "role": "admin",
          "profileUrl": "https://testaccount.learnamp.com/en/users/17",
          "status": {
            "status": "Confirmed",
            "time": "On 20 Feb 17"
          }
        }
      },
      "secondaryTeams": []
    }
  ]
}
```

> 400 Bad request - validation errors:

```json
{
    "error": "userId is missing, userId is empty",
    "fullErrors": {
        "userId": [
            "is missing",
            "is empty"
        ]
    }
}
```

## Add a User to a Team - Alternative Method

> Add a user to a team by email and team name:

```shell
curl --location --request POST 'https://api.learnamp.com/v1/teams/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
  "teamName": "Marketing",
  "userEmail": "new@member.com"
}'
```

```ruby
module Learnamp
  class TeamUsers
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def create(params)
      response = self.class.post('/teams/users', { 
        headers: headers,
        body: params.to_json 
      })
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
  userEmail: 'user@email.com',
  teamName: 'Tech Team'
}

Learnamp::TeamUsers.new(token).create(params)
```

Adds a user to a Team using email and team name instead of IDs.

In order to simplify integration with third party services, another
endpoint responsible for team member creation was implemented.

### HTTP Request

`POST https://{API_BASE_URL}/v1/teams/users`

### Required Scope
This endpoint requires the `team_users:create` scope.

### Data in Body

Parameter | Example value | Description
--------- | ------- | -----------
teamId | 1200 | ID of the team
teamName | "Marketing" | Name of the team
userId | 1 | User ID of user to add to team
userEmail | "user@email.com" | Email of the new team member
response | Teams::Simple | Optional: (blank value is the default) or "Teams::Simple" or "Teams::Extended". Value of "Teams::Simple" will include basic team information in the response. Value of "Teams::Extended: will include full team information in the response, including full list of users, sub-teams and tags in the JSON response. Only use the "Teams::Extended" response if all this data is required.

Parameters `teamId` and `teamName` are mutually exclusive - only one of them can be present in the request payload. Same rule applies for `userId` and `userEmail`.

> 201 Created - successful response:

```json
{
  "id": 1,
  "teamId": 3,
  "userId": 1
}
```

> 201 Created - successful response, using response = Teams::Extended param

```json
{
  "id": 1,
  "name": "Test Team",
  "teamUsersCount": 2,
  "apiTeamPath": "/v1/teams/1.json",
  "apiTeamUsersPath": "/v1/teams/1/users.json",
  "manager": {
      "id": 7,
      "firstName": "Test",
      "lastName": "Manager",
      "jobTitle": "Ops Director",
      "email": "admin@example.com",
      "timeZone": "London",
      "language": "en",
      "role": "admin",
      "profileUrl": "http://testaccount.learnamp.com/en/users/7",
      "status": {
          "status": "Invite pending",
          "time": "Sent 25 Sep 19"
      }
  },
  "users": [
    {
      "id": 1,
      "firstName": "Test",
      "lastName": "User",
      "jobTitle": "Developer",
      "email": "test@email.com",
      "timeZone": "London",
      "language": "en",
      "role": "viewer",
      "profileUrl": "https://testaccount.learnamp.com/en/users/1",
      "status": {
          "status": "Confirmed",
          "time": "On 29 Nov 16"
      },
      "avatar": "AVATAR_IMAGE_URL",
      "manager": {
          "id": 17,
          "firstName": "Manager",
          "lastName": "User",
          "jobTitle": "Head of Ops",
          "email": "testuser2@email.com",
          "timeZone": "New York",
          "language": "en-US",
          "role": "admin",
          "profileUrl": "https://testaccount.learnamp.com/en/users/17",
          "status": {
              "status": "Confirmed",
              "time": "On 20 Feb 17"
          }
      },
      "location": "London, UK",
      "primaryTeam": {
        "id": 15,
        "name": "Operations",
        "teamUsersCount": 10,
        "apiTeamPath": "/v1/teams/15.json",
        "apiTeamUsersPath": "/v1/teams/15/users.json",
        "manager": {
          "id": 17,
          "firstName": "Manager",
          "lastName": "User",
          "jobTitle": "Head of Ops",
          "email": "testuser2@email.com",
          "timeZone": "New York",
          "language": "en-US",
          "role": "admin",
          "profileUrl": "https://testaccount.learnamp.com/en/users/17",
          "status": {
            "status": "Confirmed",
            "time": "On 20 Feb 17"
          }
        }
      },
      "secondaryTeams": []
    }
  ]
}
```

> 400 Bad request - validation errors:

```json
{
    "error": "userId is missing, userId is empty",
    "fullErrors": {
        "userId": [
            "is missing",
            "is empty"
        ]
    }
}
```

## Remove a User from a Team

> Remove a user from a team:

```shell
curl --location --request DELETE 'https://api.learnamp.com/v1/teams/1/users/1' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class TeamUsers
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def remove(team_id, user_id)
      response = self.class.delete("/teams/#{team_id}/users/#{user_id}", { headers: headers })
      response.ok?
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

success = Learnamp::TeamUsers.new(token).remove(1, 1)
```

Remove a user from a team.

### HTTP Request

`DELETE https://{API_BASE_URL}/v1/teams/{team_id}/users/{user_id}`

### Required Scope
This endpoint requires the `team_users:delete` scope.

> 204 No Content - successful response:

```json
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Add Users to a Team

> Add multiple users to a team:

```shell
curl --location --request POST 'https://api.learnamp.com/v1/teams/1/bulk/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
  "userIds": [1, 2, 3],
  "isPrimary": true
}'
```

```ruby
module Learnamp
  class TeamUsers
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def bulk_add(team_id, users_details)
      response = self.class.post("/teams/#{team_id}/bulk/users",
        {
          headers: headers,
          body: users_details.to_json
        }
      )
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

users_details = {
  userIds: [1, 2, 3],
  isPrimary: true
}

result = Learnamp::TeamUsers.new(token).bulk_add(1, users_details)
```

Add multiple users to a team in a single request.

### HTTP Request

`POST https://{API_BASE_URL}/v1/teams/{team_id}/bulk/users`

### Required Scope
This endpoint requires the `team_users:create` scope.
