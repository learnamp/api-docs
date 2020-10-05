# Users

## User Description

In Learn Amp, a User, is a person who can access the platform. User's belong to your company account, and have a status like Invite Pending, Active or Deactivated.


## View All Users

> View all users in your account:

```shell
curl --location --request GET 'http://api.learnamp.com/v1/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
require "uri"
require "net/http"

url = URI("http://api.learnamp.com/v1/users")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Get.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "http://api.learnamp.com/v1/users"

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
$request->setUrl('http://api.learnamp.com/v1/users');
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

View all users.

`GET https://api.learnamp.com/users`

Response will be paginated [see pagination](#pagination)

### Headers
Header | Value | Description
--------- | ------- | -----------
Authorization | "Bearer YOUR-ACCESS-TOKEN" | Auth token [see Authorization](#authentication)

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
      "profileUrl": "http://testaccount.learnamp.com/en/users/1",
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
          "profileUrl": "http://testaccount.learnamp.com/en/users/17",
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
          "profileUrl": "http://testaccount.learnamp.com/en/users/17",
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


## Create a User

> Create a new user in your account:

```shell
curl --location --request POST 'http://api.learnamp.com/v1/users' \
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
--form 'skip_invitation=true' \
--form 'secondaryTeamIds[]=377'
```

```ruby
require "uri"
require "net/http"

url = URI("http://api.learnamp.com/v1/users")

http = Net::HTTP.new(url.host, url.port)
request = Net::HTTP::Post.new(url)

request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"

form_data = [
  ['email', 'testuser@test.com'],
  ['firstName', 'Test'],
  ['lastName', 'User'],
  ['role', 'curator'],
  ['language', 'fr'],
  ['jobTitle', 'Developer'],
  ['primaryTeamId', 15],
  ['secondaryTeamIds', [377,376]],
  ['managerId', 1],
  ['skip_invitation', 'true']
]

request.set_form form_data, 'multipart/form-data'

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "http://api.learnamp.com/v1/users"

payload = {
  'email': 'testuser@test.com',
  'firstName': 'Test',
  'lastName': 'User',
  'role': 'curator',
  'language': 'fr',
  'jobTitle': 'Developer',
  'primaryTeamId': 15,
  'secondaryTeamIds': [376,377],
  'managerId': 1,
  'skip_invitation': 'true'
}
files = []

headers = {
  'Authorization': 'Bearer YOUR-ACCESS-TOKEN'
}

response = requests.request("POST", url, headers = headers, data = payload, files = [])

print(response.text.encode('utf8'))
```

```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('http://api.learnamp.com/v1/users');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Authorization' => 'Bearer YOUR-ACCESS-TOKEN'
));
$request->addPostParameter(array(
  'email' => 'testuser@test.com',
  'firstName' => 'Test',
  'lastName' => 'User',
  'role' => 'curator',
  'language' => 'fr',
  'jobTitle' => 'Developer',
  'primaryTeamId' => 15,
  'secondaryTeamIds[]' => 376,
  'managerId' => '1',
  'skip_invitation' => 'true',
  'secondaryTeamIds[]' => 377
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

Create a user and trigger an invite email.

`POST https://api.learnamp.com/users`

### Headers
Header | Value | Description
--------- | ------- | -----------
Authorization | "Bearer YOUR-ACCESS-TOKEN" | Auth token [see Authorization](#authentication)

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
skip_invitation | true | Skip sending the user an invitation email immediately. If skip_invitation is not set, the user will be immediately sent an invitation email

> 201 Created - successful response:

```json
{
    "id": 1905,
    "firstName": "Test",
    "lastName": "User",
    "jobTitle": "Developer",
    "email": "testuser@test.com",
    "timeZone": "London",
    "language": "fr",
    "role": "curator",
    "profileUrl": "https://testaccount.learnamp.com/en/users/1905",
    "status": {
      "status": "Not yet invited"
    }
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

