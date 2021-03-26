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

The following URL params by be included, to filter the result set:

`GET https://api.learnamp.com/v1/users?filters[email]=test@email.com`

URL Param | Value | Description
--------- | ------- | -----------
filters[email] | email@test.com | Return user with matching email address

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
      "department": "Marketing",
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

Parameter | Example value | Description (* required)
--------- | ------- | -----------
email | testuser@test.com | Email address of user *(\*)*
firstName | Test | First name of user *(\*)*
lastName | User | Last name of user *(\*)*
language | fr | Primary language short code. One of:  en, en-US, de, es-CO, fr, it, nl, pt-BR, pl, ru, zh-CN, zh-TW, ja, ar
jobTitle | Developer | Job title of user
primaryTeamId | 15 | Team ID of primary team [see Teams](#teams)
secondaryTeamIds | [376,377] | Array of Team IDs of seconary teams
managerId | 1 | User ID of this user's Manager (override manager, not primary team manager)
hireDate | 2021-02-28 | Employment start date for user in ISO 8601 date format
location | London | Primary location of user
department | Marketing | Department of user
customFields | [{ name: "Employee ID", value: "12-34-56" }] | CustomFields param is an array, of name/value pairs for custom fields.

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