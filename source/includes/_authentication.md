# Authentication

> Generate an Access Token from the Auth Token URL:

```shell
curl --location --request POST 'https://api.learnamp.com/oauth/token' \
--form 'client_id=YOUR-CLIENT-ID' \
--form 'client_secret=YOUR-CLIENT-SECRET' \
--form 'grant_type=client_credentials'
```

```ruby
require "uri"
require "net/http"

url = URI("https://api.learnamp.com/oauth/token")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)

form_data = [
  ['client_id', 'YOUR-CLIENT-ID'],
  ['client_secret', 'YOUR-CLIENT-SECRET'],
  ['grant_type', 'client_credentials']
]

request.set_form form_data, 'multipart/form-data'

response = http.request(request)

puts response.read_body
```

```php
require_once 'HTTP/Request2.php';

$request = new HTTP_Request2();
$request->setUrl('https://api.learnamp.com/oauth/token');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));

$request->addPostParameter(array(
  'client_id' => 'YOUR-CLIENT-ID',
  'client_secret' => 'YOUR-CLIENT-SECRET',
  'grant_type' => 'client_credentials'
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

```python
import requests

url = "https://api.learnamp.com/oauth/token"

payload = {
  'client_id': 'YOUR-CLIENT-ID',
  'client_secret': 'YOUR-CLIENT-SECRET',
  'grant_type': 'client_credentials'
}

response = requests.request("POST", url, headers = [], data = payload, files = [])

print(response.text.encode('utf8'))
```

> 200 OK - successful response:

```json
{
  "access_token":"abcd1234efgh5678",
  "token_type":"Bearer",
  "expires_in":7200,
  "scope":"public",
  "created_at":1601480215
}
```

The Learn Amp API is secured using the OAuth2 Client Credentials flow.

Before calls can be made to the API, an **access token** must be retrieved from the Auth Token URL.

`POST https://api.learnamp.com/oauth/token`

### Data in Body

Parameter | Value | Description
--------- | ------- | -----------
client_id | YOUR-CLIENT-ID | API Credentials Client ID
client_secret | YOUR-CLIENT-SECRET | API Credentials Client Secret
grant_type | client_credentials | Required oath2 grant type

<aside class="notice">
You must replace <code>YOUR-CLIENT-ID</code> and <code>YOUR-CLIENT-SECRET</code> with your API credentials.
</aside>

Subsequent API calls then require this access token to be present in an **Authorization header**:

`Authorization: Bearer YOUR-ACCESS-TOKEN`

<aside class="success">
A successful response will return an <code>access_token</code>, please replace <code>YOUR-ACCESS-TOKEN</code> with this value in all authorization headers.
</aside>