# Tasks

## Task Description

In Learn Amp, a Task is a piece of learning content that a user is given a specific deadline to complete.

## View All Tasks

> View all tasks in your account:

```shell
curl --location --request GET 'http://api.learnamp.com/v1/tasks' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
require "uri"
require "net/http"

url = URI("http://api.learnamp.com/v1/tasks")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Get.new(url)
request["Authorization"] = "Bearer YOUR-ACCESS-TOKEN"

response = http.request(request)
puts response.read_body
```

```python
import requests

url = "http://api.learnamp.com/v1/tasks"

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
$request->setUrl('http://api.learnamp.com/v1/tasks');
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

View all tasks

`GET https://api.learnamp.com/tasks`

Response will be paginated [see pagination](#pagination)


### Optional Filters in URL Params

The following URL params by be included, to filter the result set:

`GET https://api.learnamp.com/tasks?filters[deadline][from]=2021-01-01&filters[deadline][to]=2021-06-01`

URL Param | Example Value | Description
--------- | ------- | -----------
filters[assigned_at][from] | "2021-12-31" | Assigned date range FROM date in ISO 8601 format
filters[assigned_at][to] | "2022-02-28" | Assigned date range TO date in ISO 8601 format
filters[completed_at][from] | "2021-12-31" | Completion date range FROM date in ISO 8601 format
filters[completed_at][to] | "2022-02-28" | Completion date range TO date in ISO 8601 format
filters[deadline][from] | "2021-12-31" | Deadline date range FROM date in ISO 8601 format
filters[deadline][to] | "2022-02-28" | Deadline date range TO date in ISO 8601 format
filters[created_at][from] | "2021-12-31" | Created at date range FROM date in ISO 8601 format. Tasks may be created before they are actually assigned to the user.
filters[created_at][to] | "2022-02-28" | Created at date range TO date in ISO 8601 format
filters[update_at][from] | "2021-12-31" | Updated at date range FROM date in ISO 8601 format
filters[update_at][to] | "2022-02-28" | Updated at date range TO date in ISO 8601 format
filters[id] | 2456 | ID of specific task
filters[user_id] | 78823 | User ID of person assigned the task
filters[taskable_type] | "Item,Channel,Learnlist,Quiz" | Type of learning object. Can be single value, or comma seperated list of Task types: any of Item,Channel,Learnlist,Quiz
filters[taskable_id] | 99874 | ID of specific learning object
filters[status] | "completed" | Task status. One of: completed / overdue / incomplete

> 200 OK - successful response:

```json
{
    "tasks": [
        {
            "id": 4428,
            "taskableId": 634,
            "taskableType": "Item",
            "createdAt": "2021-02-24T15:52:59Z",
            "deadline": "2021-02-28",
            "expiredAt": null,
            "assignedAt": "2021-02-24T15:52:59Z",
            "completedAt": null,
            "name": "Landing Page Design & Web Design Fundamentals",
            "taskable": {
                "id": 634,
                "name": "Landing Page Design & Web Design Fundamentals",
                "shortDescription": null,
                "type": "Item",
                "url": "https://testaccount.learnamp.com/en/items/landing-page-design-web-design-fundamentals-2017",
                "addedBy": {
                    "id": 1,
                    "firstName": "Richard",
                    "lastName": "Larcombe",
                    "jobTitle": "CTO",
                    "email": "rjlarcombe@gmail.com",
                    "timeZone": "London",
                    "language": "en",
                    "role": "super_admin",
                    "profileUrl": "https://testaccount.learnamp.com/en/users/1",
                    "status": {
                        "status": "Confirmed",
                        "time": "On 29 Nov 16"
                    }
                },
                "displayAddedBy": true,
                "totalTimeEstimate": "1-10 hrs"
            },
            "typeLabel": "Course",
            "overdue": true,
            "mandatory": false,
            "completed": false,
            "expired": false,
            "setFor": {
                "id": 123,
                "firstName": "Andy",
                "lastName": "Jones",
                "jobTitle": "Customer Success Executive",
                "email": "andy@example.com",
                "timeZone": "London",
                "language": "en",
                "role": "viewer",
                "profileUrl": "https://testaccount.learnamp.com/en/users/123",
                "status": {
                    "status": "Confirmed",
                    "time": "On 15 Oct 20"
                }
            },
            "url": "https://testaccount.learnamp.com/en/items/landing-page-design-web-design-fundamentals-2017",
            "assigner": {
                "id": 1,
                "firstName": "Admin",
                "lastName": "User",
                "jobTitle": "Boss",
                "email": "adminuser@example.com",
                "timeZone": "London",
                "language": "en",
                "role": "admin",
                "profileUrl": "https://testaccount.learnamp.com/en/users/1",
                "status": {
                    "status": "Confirmed",
                    "time": "On 29 Nov 16"
                }
            }
        }
    ]
}
```