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
                "profileUrl": "http://riseto.learnamp.com/en/users/7",
                "status": {
                    "status": "Invite pending",
                    "time": "Sent 25 Sep 19"
                }
            }
        },
  ]
}

```
