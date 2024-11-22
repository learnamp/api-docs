# Team Users

## View all users in a Team

> View all users in a specific team:

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

    def find(team_id)
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

users = Learnamp::TeamUsers.new(token).find(245)
```

View a specific team, and include all users in that team.

`GET https://{API_BASE_URL}/v1/teams/{teamId}/users`

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
        },
      ]
    }
  ]
}

```

## Add a User to a Team

> Add a user to a team:

```shell
curl --location --request POST 'https://api.learnamp.com/v1/teams/1/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--form 'userId=1'
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

    def create(team_id, params)
      response = self.class.post("/teams/#{team_id}/users", { body: params, headers: headers })
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
  userId: 1
}

Learnamp::TeamUsers.new(token).create(245, params)
```

Adds a user to a Team.

`POST https://{API_BASE_URL}/v1/teams/{teamId}/users`

Note that Team ID is passed in as a URL segment.

### Data in Body

Parameter | Example value | Description (* required)
--------- | ------- | -----------
userId | 1 | User ID of user to add to team

> 201 Created - successful response:

```json
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
        },
      ]
    }
  ]
}

```
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

## Add a User to a Team - second version

```shell
curl --location --request POST 'https://api.learnamp.com/v1/teams/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--form 'teamName=Marketing'
--form 'userEmail=new@member.com'
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
      response = self.class.post('/teams/users', { body: params, headers: headers })
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
  userEmail: 'user@email.com',
  teamName: 'Tech Team'
}

Learnamp::TeamUsers.new(token).create(params)
```

Adds a user to a Team.

In order to simplify integration with third party services, another
endpoint responsible for team member creation was implemented.

`POST https://{API_BASE_URL}/v1/teams/users`

### Data in Body

Parameter | Example value | Description (* required)
--------- | ------- | -----------
teamId | 1200 | ID of the team
teamName | "Marketing" | Name of the team
userId | 1 | User ID of user to add to team
userEmail | "user@email.com" | Email of the new team member

Parameters `teamId` and `teamName` are mutually exclusive - only one of them can be present in the request payload. Same rule applies for `userId` and `userEmail`.

> 201 Created - successful response:

```json
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
        },
      ]
    }
  ]
}

```
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

> Remove a user from team:

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

    def delete(team_id, user_id)
      response = self.class.delete("/teams/#{team_id}/users/#{user_id}", { headers: headers })
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

Learnamp::TeamUsers.new(token).delete(245, 1)
```

Delete a user from a team. (User and team are kept, but their association is removed).

`DELETE https://{API_BASE_URL}/v1/teams/{teamId}/users/{userId}`


> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```
