# Users

## User Description

In Learn Amp, a User, is a person who can access the platform. User's belong to your company account, and have a status like Invite Pending, Active or Deactivated.


## View All Users

> View all users in your account:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Users
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all
      response = self.class.get('/users', { headers: headers })
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

users = Learnamp::Users.new(token).all
```

View all users.

`GET https://api.learnamp.com/v1/users`

Response will be paginated [see pagination](#pagination)

### Optional Filters in URL Params

The following URL params may be included, to filter the result set:

`GET https://api.learnamp.com/v1/users?filters[email]=test@email.com`


URL Param | Type | Example Value | Description
--------- | ------- | ------- | -----------
expanded | boolean | true | Optional expanded json. Returns all available user details including custom fields and teams information.
filters[email] | email |email@test.com | Return users with matching email address
filters[first_name] | string | John | Return users with matching first name (using `ILIKE '%value%'`)
filters[last_name] | string | Smith | Return users with matching last name (using `ILIKE '%value%'`)
filters[role] | enum | viewer | Return users with matching role. <br />Possible values: "viewer", "curator", "reporter", "hr", "admin", "owner"
filters[no_team] | boolean | true | Return users that have no team assigned
filters[team_ids] | array or string | *[1,2,3]* or *1,2,3* | Return members of any of the given teams by team ID.<br /><br />Param can be array of team ids, or a string of comma seperated team ids


> 200 OK - successful response:

```json
{
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
      "hireDate": "2021-01-15",
      "profileUrl": "https://testaccount.learnamp.com/en/users/1",
      "status": {
          "status": "Confirmed",
          "time": "On 29 Nov 16"
      },
    },
  ]
}

```

> 200 OK - successfully response, when param expanded=true is included

```json
{
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
        "hireDate": "2021-01-15",
        "profileUrl": "https://testaccount.learnamp.com/en/users/1",
        "status": {
            "status": "Confirmed",
            "time": "On 29 Nov 16"
        },
        "avatar": "https://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,q_auto,w_100/v1505401603/iysnlkr6sr6ys0dybeh0.jpg",
        "manager": {
            "id": 17,
            "firstName": "Test",
            "lastName": "Manager",
            "jobTitle": "Ops Director",
            "email": "test2@email.com",
            "timeZone": "London",
            "language": "en",
            "role": "admin",
            "profileUrl": "https://testaccount.learnamp.com/en/users/17",
            "status": {
                "status": "Confirmed",
                "time": "On 20 Feb 17"
            }
        },
        "location": "London, UK",
        "department": "Marketing",
        "primaryTeam": {
            "id": 15,
            "name": "Operations",
            "teamUsersCount": 1,
            "apiTeamPath": "/v1/teams/15.json",
            "apiTeamUsersPath": "/v1/teams/15/users.json",
            "manager": {
              "id": 17,
              "firstName": "Test",
              "lastName": "Manager",
              "jobTitle": "Ops Director",
              "email": "test2@email.com",
              "timeZone": "London",
              "language": "en",
              "role": "admin",
              "profileUrl": "https://testaccount.learnamp.com/en/users/17",
              "status": {
                  "status": "Confirmed",
                  "time": "On 20 Feb 17"
              }
            }
          }
        },
        "secondaryTeams": []
    }
  ]
}
```

## Show a User

> Display details for a single user:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/users/1' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Users
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def find(id)
      response = self.class.get("/users/#{id}", { headers: headers })
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

user = Learnamp::Users.new(token).find(1920)
```

Display user details for one specific user.

`GET https://api.learnamp.com/v1/users/{userId}`


> 200 OK - successful response:

```json
{
    "id": 1,
    "firstName": "Test",
    "lastName": "User",
    "jobTitle": "Developer",
    "email": "test@email.com",
    "timeZone": "London",
    "language": "en",
    "role": "viewer",
    "hireDate": "2021-01-15",
    "profileUrl": "https://testaccount.learnamp.com/en/users/1",
    "status": {
        "status": "Confirmed",
        "time": "On 29 Nov 16"
    },
    "avatar": "https://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,q_auto,w_100/v1505401603/iysnlkr6sr6ys0dybeh0.jpg",
    "manager": {
        "id": 17,
        "firstName": "Test",
        "lastName": "Manager",
        "jobTitle": "Ops Director",
        "email": "test2@email.com",
        "timeZone": "London",
        "language": "en",
        "role": "admin",
        "profileUrl": "https://testaccount.learnamp.com/en/users/17",
        "status": {
            "status": "Confirmed",
            "time": "On 20 Feb 17"
        }
    },
    "location": "London, UK",
    "department": "Marketing",
    "primaryTeam": {
        "id": 15,
        "name": "Operations",
        "teamUsersCount": 1,
        "apiTeamPath": "/v1/teams/15.json",
        "apiTeamUsersPath": "/v1/teams/15/users.json",
        "manager": {
          "id": 17,
          "firstName": "Test",
          "lastName": "Manager",
          "jobTitle": "Ops Director",
          "email": "test2@email.com",
          "timeZone": "London",
          "language": "en",
          "role": "admin",
          "profileUrl": "https://testaccount.learnamp.com/en/users/17",
          "status": {
              "status": "Confirmed",
              "time": "On 20 Feb 17"
          }
        }
      }
    },
    "secondaryTeams": []
}
```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Create a User

> Create a new user in your account:

```shell
curl --location --request POST 'https://api.learnamp.com/v1/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--form 'email=testuser@test.com' \
--form 'firstName=Test' \
--form 'lastName=User' \
--form 'role=curator' \
--form 'language=fr' \
--form 'jobTitle=Developer' \
--form 'primaryTeamId=15' \
--form 'secondaryTeamIds[]=376' \
--form 'managerId=1' \
--form 'skipInvitation=true' \
--form 'secondaryTeamIds[]=377'
```

```ruby
module Learnamp
  class Users
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def create(params)
      response = self.class.post("/users", { body: params, headers: headers })
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
  email:  "testuser@test.com",
  firstName: "Test",
  lastName:  "User",
  language: "en",
  jobTitle:  "Developer",
  role: "viewer",
  primaryTeamId: 379,
  secondaryTeamIds:  [376,377],
  managerId: 1,
  skipInvitation:  true,
  hireDate:  "2021-02-28",
  location:  "London",
  department:  "Marketing",
  customFields: [{ name: "Employee ID", value: "123456" }]
}

user = Learnamp::Users.new(token).create(params)

```

Create a user and trigger an invite email.

`POST https://api.learnamp.com/v1/users`

### Data in Body

Parameter | Example value | Description (* required)
--------- | ------- | -----------
email | testuser@test.com | Email address of user *(\*)*
firstName | Test | First name of user *(\*)*
lastName | User | Last name of user *(\*)*
language | fr | Primary language short code. One of:  en, en-US, de, es-CO, fr, it, nl, pt-BR, pl, ru, zh-CN, zh-TW, ja, ar
jobTitle | Developer | Job title of user
role | viewer | user's role. One of: viewer, curator, admin, hr, reporter
primaryTeamId | 15 | Team ID of primary team [see Teams](#teams)
secondaryTeamIds | [376,377] | Array of Team IDs of seconary teams
managerId | 1 | User ID of this user's Manager (override manager, not primary team manager)
skipInvitation | true | Skip sending the user an invitation email immediately. If skipInvitation is not set, the user will be immediately sent an invitation email
hireDate | 2021-02-28 | Employment start date for user in ISO 8601 date format
location | London | Primary location of user
department | Marketing | Department of user
customFields | [{ name: "Employee ID", value: "12-34-56" }] | CustomFields param is an array, of name/value pairs for custom fields.

> 201 Created - successful response:

```json
{
    "id": 1,
    "firstName": "Test",
    "lastName": "User",
    "jobTitle": "Developer",
    "email": "test@email.com",
    "timeZone": "London",
    "language": "en",
    "role": "viewer",
    "hireDate": "2021-01-15",
    "profileUrl": "https://testaccount.learnamp.com/en/users/1",
    "status": {
        "status": "Confirmed",
        "time": "On 29 Nov 16"
    },
    "avatar": "https://res.cloudinary.com/dfiav5ctj/image/upload/c_crop,g_custom/a_exif,c_fill,dpr_1.0,f_auto,q_auto,w_100/v1505401603/iysnlkr6sr6ys0dybeh0.jpg",
    "manager": {
        "id": 17,
        "firstName": "Test",
        "lastName": "Manager",
        "jobTitle": "Ops Director",
        "email": "test2@email.com",
        "timeZone": "London",
        "language": "en",
        "role": "admin",
        "profileUrl": "https://testaccount.learnamp.com/en/users/17",
        "status": {
            "status": "Confirmed",
            "time": "On 20 Feb 17"
        }
    },
    "location": "London, UK",
    "department": "Marketing",
    "primaryTeam": {
        "id": 15,
        "name": "Operations",
        "teamUsersCount": 1,
        "apiTeamPath": "/v1/teams/15.json",
        "apiTeamUsersPath": "/v1/teams/15/users.json",
        "manager": {
          "id": 17,
          "firstName": "Test",
          "lastName": "Manager",
          "jobTitle": "Ops Director",
          "email": "test2@email.com",
          "timeZone": "London",
          "language": "en",
          "role": "admin",
          "profileUrl": "https://testaccount.learnamp.com/en/users/17",
          "status": {
              "status": "Confirmed",
              "time": "On 20 Feb 17"
          }
        }
      }
    },
    "secondaryTeams": []
}
```

> 400 Bad request - validation errors:

```json
{
    "error": "email is missing, email is empty, firstName is missing, lastName is missing",
    "fullErrors": {
        "email": [
            "is missing",
            "is empty"
        ],
        "firstName": [
            "is missing"
        ],
        "lastName": [
            "is missing"
        ]
    }
}
```

## Update a user

> Update an existing user:

```shell
curl --location --request PUT 'https://api.learnamp.com/v1/users/1382' \
--header 'Authorization: Bearer SQn9nZAxgwPJcz-neajmNahBdlc2DRoNbUHwx9A4Rvw' \
--form 'firstName=Jim' \
--form 'lastName=Robinson' \
--form 'jobTitle=Businessman' \
--form 'language=en' \
--form 'primaryTeamId=379' \
--form 'managerId=1' \
--form 'email=jimrobinson@test.com' \
--form 'secondaryTeamIds[]=380'
```

```ruby
module Learnamp
  class Users
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def update(id, params)
      response = self.class.put("/users/#{id}", { body: params, headers: headers })
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
  email:  "testuser@test.com",
  firstName: "Test2",
  lastName:  "User2",
  language: "en",
  jobTitle:  "Developer",
  role: "viewer",
  primaryTeamId: 379,
  secondaryTeamIds:  [376,377],
  managerId: 1,
  skipInvitation:  true,
  hireDate:  "2021-02-28",
  location:  "London",
  department:  "Marketing",
  customFields: [{ name: "Employee ID", value: "123456" }]
}

user = Learnamp::Users.new(token).update(1904, params)
```

Update a user's details'.

`PUT https://api.learnamp.com/v1/users/{userId}`

### Data in Body

Parameter | type | Example value | Description (* required)
--------- | ---- | ------- | -----------
email | email |testuser@test.com | Email address of user *(\*)*
firstName | string | Test | First name of user *(\*)*
lastName | string | User | Last name of user *(\*)*
language | enum | fr | Primary language short code. One of:  en, en-US, de, es-CO, fr, it, nl, pt-BR, pl, ru, zh-CN, zh-TW, ja, ar
jobTitle | string |Developer | Job title of user
primaryTeamId | integer | 15 | Team ID of primary team [see Teams](#teams)
secondaryTeamIds | Array(integer) | [376,377] | Array of Team IDs of seconary teams
managerId | integer | 1 | User ID of this user's Manager (override manager, not primary team manager)
hireDate | date | 2021-02-28 | Employment start date for user in ISO 8601 date format
location | string | London | Primary location of user
department | string | Marketing | Department of user
reactivate | boolean | true | Reactivates user if deactivated
customFields | object | [{ name: "Employee ID", value: "12-34-56" }] | CustomFields param is an array, of name/value pairs for custom fields.

> 200 OK - successful response:

```json
{
    "id": 1382,
    "firstName": "Jim",
    "lastName": "Robinson",
    "jobTitle": "Businessman",
    "email": "jimrobinson@test.com",
    "timeZone": "London",
    "language": "en",
    "role": "admin",
    "profileUrl": "https://testaccount.learnamp.com/en/users/1382",
    "status": {
        "status": "Invite pending",
        "time": "Sent 25 Sep 19"
    },
    "avatar": "AVATAR_URL",
    "manager": {
        "id": 1,
        "firstName": "Test",
        "lastName": "User",
        "jobTitle": "Director",
        "email": "test@email.com",
        "timeZone": "London",
        "language": "en",
        "role": "admin",
        "profileUrl": "https://testaccount.learnamp.com/en/users/1",
        "status": {
            "status": "Confirmed",
            "time": "On 29 Nov 16"
        }
    },
    "location": null,
    "primaryTeam": {
        "id": 379,
        "name": "Team 1",
        "teamUsersCount": 3,
        "apiTeamPath": "/v1/teams/379.json",
        "apiTeamUsersPath": "/v1/teams/379/users.json",
        "manager": {
        }
    },
    "secondaryTeams": [
        {
            "id": 380,
            "name": "Team 2 - operations",
            "teamUsersCount": 1,
            "apiTeamPath": "/v1/teams/380.json",
            "apiTeamUsersPath": "/v1/teams/380/users.json",
            "manager": {
            }
        }
    ]
}
```

> 400 Bad request - validation errors:

```json
{
    "error": "email is missing, email is empty, firstName is missing, lastName is missing",
    "fullErrors": {
        "email": [
            "is missing",
            "is empty"
        ],
        "firstName": [
            "is missing"
        ],
        "lastName": [
            "is missing"
        ]
    }
}
```

## Deactivate a User

> Deactivate a user:

```shell
curl --location --request PUT 'https://api.learnamp.com/v1/users/1/deactivate' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Users
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def deactivate(id)
      response = self.class.put("/users/#{id}/deactivate", { headers: headers })
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

Learnamp::Users.new(token).deactivate(1904)
```

Deactivate a user, so that they can no longer login. Their data is not removed.

`PUT https://api.learnamp.com/v1/users/{userId}/deactivate`


> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Reactivate a User

> Reactivate a deactivated user:

```shell
curl --location --request PUT 'https://api.learnamp.com/v1/users/1/reactivate' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Users
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def reactivate(id)
      response = self.class.put("/users/#{id}/reactivate", { headers: headers })
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

Learnamp::Users.new(token).reactivate(1904)
```

Reactivate a user, so that they login again.

`PUT https://api.learnamp.com/v1/users/{userId}/reactivate`


> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Delete a User

> Delete a user:

```shell
curl --location --request DELETE 'https://api.learnamp.com/v1/users/1' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Users
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def delete(id)
      response = self.class.delete("/users/#{id}", { headers: headers })
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

Learnamp::Users.new(token).delete(1904)
```

Delete a user, so that they can no longer login. Their data is removed.

`DELETE https://api.learnamp.com/v1/users/{userId}`


> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```

## Channels Progress

> View progress of all assigned channels by the specified user:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/users/567/channels_progress' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class Users
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def channels_progress(user_id)
      response = self.class.get("/channels/#{user_id}/users_progress", { headers: headers })
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

data = Learnamp::Users.new(token).Channels_progress(123)
```

View progress of all assigned channels by a specified user. This is analogous to Learn Amp's People Log feature, for a single user.

`GET https://api.learnamp.com/v1/users/{userId}/channels_progress`

This end-point will return a paginated array of channels that are assigned to the specified user. Each element will contain the completion percentage of the channel by that user, as well as the datetime (if any) when the channel was completed.

Please note: The completion percentage is a cached value, which is refreshed in the background automatically. The completion percentage shown may therefore take a few minutes to update.

The results are ordered alphabetically by created at date of the channel, most recent first.

The end-point will accept a `userId` of a deactivated user, and return the deactivated users's channel progress.

Response will be paginated [see pagination](#pagination)

> 200 OK - successful response:

```json
[
    {
        "user_id": 200321,
        "first_name": "John",
        "last_name": "Abc",
        "email": "jabc@test.com",
        "content_name": "Favourites",
        "content_type": "Channel",
        "content_id": 1,
        "completed": false,
        "completion_percent": 50,
        "completed_at": null
    },
    {
        "user_id": 200321,
        "first_name": "John",
        "last_name": "Abc",
        "email": "jabc@test.com",
        "content_name": "Compliance Training",
        "content_type": "Channel",
        "content_id": 2,
        "completed": true,
        "completion_percent": 100,
        "completed_at": "2023-04-04T15:44:04Z"
    },
    {
        "user_id": 200321,
        "first_name": "John",
        "last_name": "Abc",
        "email": "jabc@test.com",
        "content_name": "Business Essentials",
        "content_type": "Channel",
        "content_id": 3,
        "completed": false,
        "completion_percent": 0,
        "completed_at": null
    }
]

```

> 404 Not Found - unsuccessful response when user ID does not exist:

```json
{
    "error": "Not found"
}
```