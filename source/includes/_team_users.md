# Team Users

## View all users in a Team

> View all users in a specific team:

```shell
curl --location --request GET 'http://api.learnamp.com/v1/teams/1/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
require "uri"
require "net/http"

url = URI("http://api.learnamp.com/v1/teams/1/users")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Get.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "http://api.learnamp.com/v1/teams/1/users"

payload = {}
headers = {
  'Authorization': 'Bearer YOUR-ACCESS-TOKEN'
}

response = requests.request("GET", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```

```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('http://api.learnamp.com/v1/teams/1/users');
$request->setMethod(HTTP_Request2::METHOD_GET);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Authorization' => 'Bearer YOUR-ACCESS-TOKEN'
));
try {
  $response = $request->send();
  if ($response->getStatus() == 200) {
    echo $response->getBody();
  }
  else {
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  }
}
catch(HTTP_Request2_Exception $e) {
  echo 'Error: ' . $e->getMessage();
}
```

View a specific team, and include all users in that team.

`GET https://api.learnamp.com/teams/{teamId}/users`

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
require "uri"
require "net/http"

url = URI("https://api.learnamp.com/v1/teams/1/users")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"
form_data = [
  ['userId', '1']
]
request.set_form form_data, 'multipart/form-data'
response = http.request(request)
puts response.read_body

```

```python
import requests

url = "https://api.learnamp.com/v1/teams/1/users"

payload = {
  'userId': '1'
}
headers = {
  'Authorization': 'Bearer YOUR-ACCESS-TOKEN'
}

response = requests.request("POST", url, headers=headers, data = payload, files = [])

print(response.text.encode('utf8'))

```

```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.learnamp.com/v1/teams/1/users');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Authorization' => 'Bearer YOUR-ACCESS-TOKEN'
));
$request->addPostParameter(array(
  'userId' => '1'
));
try {
  $response = $request->send();
  if ($response->getStatus() == 201) {
    echo $response->getBody();
  }
  else {
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  }
}
catch(HTTP_Request2_Exception $e) {
  echo 'Error: ' . $e->getMessage();
}
```

Adds a user to a Team.

`POST https://api.learnamp.com/teams/{teamId}/users`

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

## Remove a User from a Team

> Remove a user from team:

```shell
curl --location --request DELETE 'https://api.learnamp.com/v1/teams/1/users/1' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
require "uri"
require "net/http"

url = URI("https://api.learnamp.com/v1/teams/1/users/1")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Delete.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://api.learnamp.com/v1/teams/1/users/1"

payload = {}
headers = {
  'Authorization': 'Bearer YOUR-ACCESS-TOKEN'
}

response = requests.request("DELETE", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```

```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.learnamp.com/v1/teams/1/users/1');
$request->setMethod(HTTP_Request2::METHOD_DELETE);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Authorization' => 'Bearer YOUR-ACCESS-TOKEN'
));
try {
  $response = $request->send();
  if ($response->getStatus() == 204) {
    echo $response->getBody();
  }
  else {
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  }
}
catch(HTTP_Request2_Exception $e) {
  echo 'Error: ' . $e->getMessage();
}
```

Delete a user from a team. (User and team are kept, but their association is removed).

`DELETE https://api.learnamp.com/teams/{teamId}/users/{userId}`


> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```