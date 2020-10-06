# Teams

## Team Description

In Learn Amp, a Team is any grouping of users. Teams may map to your organisation (e.g "Sales Department"), or they may be learning teams (e.g. "Trainee Managers").

## View All Teams

> View all teams in your account:

```shell
curl --location --request GET 'http://api.learnamp.com/v1/teams' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
require "uri"
require "net/http"

url = URI("http://api.learnamp.com/v1/teams")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Get.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "http://api.learnamp.com/v1/teams"

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
$request->setUrl('http://api.learnamp.com/v1/teams');
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

View all teams

`GET https://api.learnamp.com/teams`

Response will be paginated [see pagination](#pagination)

### Headers
Header | Value | Description
--------- | ------- | -----------
Authorization | "Bearer YOUR-ACCESS-TOKEN" | Auth token [see Authorization](#authentication)

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
            }
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
require "uri"
require "net/http"

url = URI("https://api.learnamp.com/v1/teams/383")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Get.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://api.learnamp.com/v1/teams/383"

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
$request->setUrl('https://api.learnamp.com/v1/teams/383');
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

Display details for one specific Team.

`GET https://api.learnamp.com/teams/{teamId}`


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
    ]
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
require "uri"
require "net/http"

url = URI("https://api.learnamp.com/v1/teams")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"
form_data = [
  ['name', 'New Team Test'],
  ['managerId', '1']
]
request.set_form form_data, 'multipart/form-data'
response = http.request(request)
puts response.read_body

```

```python
import requests

url = "https://api.learnamp.com/v1/teams"

payload = {
  'name': 'New Team Test',
  'managerId': '1'
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
$request->setUrl('https://api.learnamp.com/v1/teams');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Authorization' => 'Bearer YOUR-ACCESS-TOKEN'
));
$request->addPostParameter(array(
  'name' => 'New Team Test',
  'managerId' => '1'
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

Create a Team

`POST https://api.learnamp.com/teams`

### Data in Body

Parameter | Example value | Description (* required)
--------- | ------- | -----------
name | Sales | Name of team
managerId | 1 | User ID of Team Manager

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
    "users": []
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

## Update a Team

> Update a team:

```shell
curl --location --request PUT 'https://api.learnamp.com/v1/teams/383' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--form 'name=New Team Name' \
--form 'managerId=1'
```

```ruby
require "uri"
require "net/http"

url = URI("https://api.learnamp.com/v1/teams/383")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Put.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"
form_data = [['name', 'New Team Name'],['managerId', '1']]
request.set_form form_data, 'multipart/form-data'
response = http.request(request)
puts response.read_body

```

```python
import requests

url = "https://api.learnamp.com/v1/teams/383"

payload = {
  'name': 'New Team Name',
  'managerId': '1'
}

headers = {
  'Authorization': 'Bearer YOUR-ACCESS-TOKEN'
}

response = requests.request("PUT", url, headers=headers, data = payload, files = [])

print(response.text.encode('utf8'))

```

```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.learnamp.com/v1/teams/383');
$request->setMethod(HTTP_Request2::METHOD_PUT);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Authorization' => 'Bearer YOUR-ACCESS-TOKEN'
));
$request->addPostParameter(array(
  'name' => 'New Team Name',
  'managerId' => '1'
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

Update a Team

`POST https://api.learnamp.com/items`

### Data in Body

Parameter | Example value | Description (* required)
--------- | ------- | -----------
name | Marketing | Team Name
managerId | 1 | User ID of Team Manager

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
    "users": []
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
require "uri"
require "net/http"

url = URI("https://api.learnamp.com/v1/teams/1")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Delete.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "https://api.learnamp.com/v1/teams/1"

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
$request->setUrl('https://api.learnamp.com/v1/teams/1');
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

Delete a team. (Users are kept, but their association with team is removed).

`DELETE https://api.learnamp.com/teams/{teamId}`


> 204 No Content - successful response:

```json

```

> 404 Not Found - unsuccessful response:

```json
{
    "error": "Not found"
}
```