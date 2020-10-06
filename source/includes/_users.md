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
require "uri"
require "net/http"

url = URI("https://api.learnamp.com/v1/users")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Get.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://api.learnamp.com/v1/users"

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
$request->setUrl('https://api.learnamp.com/v1/users');
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

```

## Show a User

> Display details for a single user:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/users/1' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
require "uri"
require "net/http"

url = URI("https://api.learnamp.com/v1/users/1")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Get.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"

response = http.request(request)
puts response.read_body

```

```python
import requests

url = "https://api.learnamp.com/v1/users/1"

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
$request->setUrl('https://api.learnamp.com/v1/users/1');
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

Display user details for one specific user.

`GET https://api.learnamp.com/users/{userId}`


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
require "uri"
require "net/http"

url = URI("https://api.learnamp.com/v1/users")

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
  ['skipInvitation', 'true']
]

request.set_form form_data, 'multipart/form-data'

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://api.learnamp.com/v1/users"

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
  'skipInvitation': 'true'
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
$request->setUrl('https://api.learnamp.com/v1/users');
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
  'skipInvitation' => 'true',
  'secondaryTeamIds[]' => 377
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

Create a user and trigger an invite email.

`POST https://api.learnamp.com/users`

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

## Update a user

> Update an existing user:

```shell
curl --location --request PUT 'http://api.learnamp.com/v1/users/1382' \
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
require "uri"
require "net/http"

url = URI("http://api.learnamp.com/v1/users/1382")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Put.new(url)
request["Authorization"] = "Bearer SQn9nZAxgwPJcz-neajmNahBdlc2DRoNbUHwx9A4Rvw"
form_data = [['firstName', 'Jim'],['lastName', 'Robinson'],['jobTitle', 'Businessman'],['language', 'en'],['primaryTeamId', '379'],['managerId', '1'],['email', 'jimrobinson@test.com'],['secondaryTeamIds[]', '380']]
request.set_form form_data, 'multipart/form-data'
response = http.request(request)
puts response.read_body

```

```python
import requests

url = "http://api.learnamp.com/v1/users/1382"

payload = {'firstName': 'Jim',
'lastName': 'Robinson',
'jobTitle': 'Businessman',
'language': 'en',
'primaryTeamId': '379',
'managerId': '1',
'email': 'jimrobinson@test.com',
'secondaryTeamIds[]': '380'}
files = [

]
headers = {
  'Authorization': 'Bearer SQn9nZAxgwPJcz-neajmNahBdlc2DRoNbUHwx9A4Rvw'
}

response = requests.request("PUT", url, headers=headers, data = payload, files = files)

print(response.text.encode('utf8'))

```

```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('http://api.learnamp.com/v1/users/1382');
$request->setMethod(HTTP_Request2::METHOD_PUT);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Authorization' => 'Bearer SQn9nZAxgwPJcz-neajmNahBdlc2DRoNbUHwx9A4Rvw'
));
$request->addPostParameter(array(
  'firstName' => 'Jim',
  'lastName' => 'Robinson',
  'jobTitle' => 'Businessman',
  'language' => 'en',
  'primaryTeamId' => '379',
  'managerId' => '1',
  'email' => 'jimrobinson@test.com',
  'secondaryTeamIds[]' => '380'
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

Update a user's details'.

`PUT https://api.learnamp.com/users/{userId}`

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
require "uri"
require "net/http"

url = URI("https://api.learnamp.com/v1/users/1/deactivate")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Put.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"

response = http.request(request)
puts response.read_body


```

```python
import requests

url = "https://api.learnamp.com/v1/users/1/deactivate"

payload = {}
headers = {
  'Authorization': 'Bearer YOUR-ACCESS-TOKEN'
}

response = requests.request("PUT", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```

```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.learnamp.com/v1/users/1/deactivate');
$request->setMethod(HTTP_Request2::METHOD_PUT);
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

Deactivate a user, so that they can no longer login. Their data is not removed.

`PUT https://api.learnamp.com/users/{userId}/deactivate`


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
require "uri"
require "net/http"

url = URI("https://api.learnamp.com/v1/users/1")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Delete.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"

response = http.request(request)
puts response.read_body


```

```python
import requests

url = "https://api.learnamp.com/v1/users/1"

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
$request->setUrl('https://api.learnamp.com/v1/users/1');
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

Delete a user, so that they can no longer login. Their data is removed.

`DELETE https://api.learnamp.com/users/{userId}`


> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```