# Teams

## Team Description

In Learn Amp, a Team is any grouping of users. Teams may map to your organisation (e.g "Sales Department"), or they may be learning teams (e.g. "Trainee Managers").

## View All Teams

> View all teams in your account:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/teams' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Teams
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(filters)
      filters_query = URI.encode_www_form(filters)
      response = self.class.get("/teams?#{filters_query}", { headers: headers })
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
  "filters[name]": "marketing"
}
teams = Learnamp::Teams.new(token).all(filters)
```

View all teams

`GET https://api.learnamp.com/v1/teams`

Response will be paginated [see pagination](#pagination)


### Optional Filters in URL Params

The following URL params by be included, to filter the result set:

`GET https://api.learnamp.com/v1/teams?filters[name]=founders`

URL Param | Value | Description
--------- | ------- | -----------
filters[name] | "Team name" | Return team with matching name
filters[tags] | "developers,product,compliance" | Return teams matching any of the given tags. Use comma seperated string.

> 200 OK - successful response:

```json
{
    "teams": [
        {
            "id": 379,
            "name": "Test Team",
            "teamUsersCount": 2,
            "apiTeamPath": "/v1/teams/379.json",
            "apiTeamUsersPath": "/v1/teams/379/users.json",
            "parentTeamId": 123,
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
            "secondaryManagers": [
                {
                    "id": 8,
                    "firstName": "Test2",
                    "lastName": "Secondary",
                    "jobTitle": "Vice director",
                    "email": "admin2@example.com",
                    "timeZone": "London",
                    "language": "en",
                    "role": "admin",
                    "profileUrl": "http://testaccount.learnamp.com/en/users/8",
                    "status": {
                      "status": "Invite pending",
                      "time": "Sent 25 Sep 19"
                    }
                }
            ]
        },
  ]
}

```

## Show a Team

> Display details for a single Team:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/teams/383' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Teams
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def find(id)
      response = self.class.get("/teams/#{id}", { headers: headers })
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

teams = Learnamp::Teams.new(token).find(245)
```

Display details for one specific Team.

`GET https://api.learnamp.com/v1/teams/{teamId}`


> 200 OK - successful response:

```json
{
    "id": 383,
    "name": "Test Team",
    "teamUsersCount": 2,
    "apiTeamPath": "/v1/teams/383.json",
    "apiTeamUsersPath": "/v1/teams/383/users.json",
    "manager": {
        "id": 7,
        "firstName": "Test",
        "lastName": "Manager",
        "jobTitle": "Ops Director",
        "email": "admin@example.com",
        "timeZone": "London",
        "language": "en",
        "role": "admin",
        "profileUrl": "https://testaccount.learnamp.com/en/users/7",
        "status": {
            "status": "Invite pending",
            "time": "Sent 25 Sep 19"
        }
    },
    "secondaryManagers": [
        {
            "id": 8,
            "firstName": "Test2",
            "lastName": "Secondary",
            "jobTitle": "Vice director",
            "email": "admin2@example.com",
            "timeZone": "London",
            "language": "en",
            "role": "admin",
            "profileUrl": "http://testaccount.learnamp.com/en/users/8",
            "status": {
              "status": "Invite pending",
              "time": "Sent 25 Sep 19"
            }
        }
    ],
    "users": [
        {
            "id": 1186,
            "firstName": "Test",
            "lastName": "User2",
            "jobTitle": "Customer Retention Manager",
            "email": "testuser2@example.com",
            "timeZone": "London",
            "language": "en",
            "role": "viewer",
            "profileUrl": "https://testaccount.learnamp.com/en/users/1186",
            "status": {
                "status": "Confirmed",
                "time": "On 4 Mar 20"
            },
            "removeFromTeamUrl": "/v1/teams/383/users/1186.json"
        },
        {
            "id": 1281,
            "firstName": "Test",
            "lastName": "User",
            "jobTitle": "Sales director",
            "email": "testuser@example.com",
            "timeZone": "London",
            "language": "en",
            "role": "admin",
            "profileUrl": "https://testaccount.learnamp.com/en/users/1281",
            "status": {
                "status": "Confirmed",
                "time": "On 3 Mar 20"
            },
            "removeFromTeamUrl": "/v1/teams/383/users/1281.json"
        }
    ],
    "subTeams": [
        {
            "id": 169,
            "name": "Middle team",
            "teamUsersCount": 2,
            "parentTeamId": 383,
            "apiTeamPath": "/v1/teams/169.json",
            "apiTeamUsersPath": "/v1/teams/169/users.json",
            "manager": null,
            "secondaryManagers": []
        }
    ],
    "parentTeam": {
        "id": 1,
        "name": "Super team",
        "teamUsersCount": 2,
        "parentTeamId": null,
        "apiTeamPath": "/v1/teams/1.json",
        "apiTeamUsersPath": "/v1/teams/1/users.json",
        "manager": null,
        "secondaryManagers": []
    },
    "tags": [
      "example team tag 1",
      "example team tag 2"
    ],
}
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Create a Team

> Create a new Team:

```shell
curl --location --request POST 'https://api.learnamp.com/v1/teams' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--form 'name=New Team Test' \
--form 'managerId=1'
```

```ruby
module Learnamp
  class Teams
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def create(params)
      response = self.class.post("/teams", { body: params, headers: headers })
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
  name: "Sales",
  managerId: 1,
  parentTeamId: 245,
  tags: "sales,account management,cold calling"
}

team = Learnamp::Teams.new(token).create(params)
```

Create a Team

`POST https://api.learnamp.com/v1/teams`

### Data in Body

Parameter (* required) | Example value | Description
--------- | ------- | -----------
name * | Sales | Name of team
managerId | 1 | User ID of Team Manager
parentTeamId | 10 | ID of the parent team
tags | "developers,product,compliance" | Tags - comma seperated string

> 201 Created - successful response:

```json
{
    "id": 449,
    "name": "New Team Test",
    "teamUsersCount": 0,
    "apiTeamPath": "/v1/teams/449.json",
    "apiTeamUsersPath": "/v1/teams/449/users.json",
    "manager": {
        "id": 1,
        "firstName": "Test",
        "lastName": "User",
        "jobTitle": "Sales Manager",
        "email": "test@example.com",
        "timeZone": "London",
        "language": "en",
        "role": "viewer",
        "profileUrl": "http://testaccount.learnamp.com/en/users/1",
        "status": {
            "status": "Confirmed",
            "time": "On 29 Nov 16"
        }
    },
    "secondaryManagers": [],
    "users": [],
    "parentTeam": {
        "id": 10,
        "name": "Super team",
        "teamUsersCount": 2,
        "parentTeamId": null,
        "apiTeamPath": "/v1/teams/10.json",
        "apiTeamUsersPath": "/v1/teams/10/users.json",
        "manager": null,
        "secondaryManagers": []
    },
    "subTeams": [],
    "tags": []
}
```

> 400 Bad request - validation errors:

```json
{
    "error": "name is missing, name is empty, parentTeamId must match existing teams IDs",
    "fullErrors": {
        "name": [
            "is missing",
            "is empty"
        ],
        "parentTeamId": [
            "must match existing teams IDs"
        ]
    }
}
```

## Update a Team

> Update a team:

```shell
curl --location --request PUT 'https://api.learnamp.com/v1/teams/383' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--form 'name=New Team Name' \
--form 'managerId=1'
```

```ruby
module Learnamp
  class Teams
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def update(id, params)
      response = self.class.put("/teams/#{id}", { body: params, headers: headers })
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

team = Learnamp::Teams.new(token).update(456, params)
```

Update a Team

`POST https://api.learnamp.com/v1/items`

### Data in Body

Parameter | Example value | Description
--------- | ------- | -----------
name | Marketing | Team Name
managerId | 1 | User ID of Team Manager
parentTeamId | 10 | ID of the parent team
tags | "developers,product,compliance" | Tags - comma seperated string

> 200 OK - successful response:

```json
{
    "id": 383,
    "name": "New Team Name",
    "teamUsersCount": 0,
    "apiTeamPath": "/v1/teams/383.json",
    "apiTeamUsersPath": "/v1/teams/383/users.json",
    "manager": {
        "id": 1,
        "firstName": "Test",
        "lastName": "User",
        "jobTitle": "Sales Manager",
        "email": "test@example.com",
        "timeZone": "London",
        "language": "en",
        "role": "viewer",
        "profileUrl": "http://testaccount.learnamp.com/en/users/1",
        "status": {
            "status": "Confirmed",
            "time": "On 29 Nov 16"
        }
    },
    "secondaryManagers": [],
    "users": [],
    "parentTeam": {
        "id": 10,
        "name": "Super team",
        "teamUsersCount": 2,
        "parentTeamId": null,
        "apiTeamPath": "/v1/teams/10.json",
        "apiTeamUsersPath": "/v1/teams/10/users.json",
        "manager": null,
        "secondaryManagers": []
    },
    "subTeams": [],
    "tags": []
}
```

> 400 Bad request - validation errors:

```json
{
    "error": "name is missing, name is empty",
    "fullErrors": {
        "name": [
            "is missing",
            "is empty"
        ]
    }
}
```

## Delete a Team

> Delete a team:

```shell
curl --location --request DELETE 'https://api.learnamp.com/v1/teams/1' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Teams
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def delete(id)
      response = self.class.delete("/teams/#{id}", { headers: headers })
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

Learnamp::Teams.new(token).delete(456)
```

Delete a team. (Users are kept, but their association with team is removed).

`DELETE https://api.learnamp.com/v1/teams/{teamId}`


> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```
