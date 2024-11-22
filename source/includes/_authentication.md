# Authentication

> Generate an Access Token from the Auth Token URL:

```shell
curl --location --request POST 'https://api.learnamp.com/oauth/token' \
--form 'client_id=YOUR-CLIENT-ID' \
--form 'client_secret=YOUR-CLIENT-SECRET' \
--form 'grant_type=client_credentials'
```

```ruby
module Learnamp
  class Auth
    include HTTParty
    base_uri ENV['BASE_URL']

    def self.access_token
      post('/oauth/token',
           body: {
             grant_type: 'client_credentials',
             client_id: ENV['CLIENT_ID'],
             client_secret: ENV['CLIENT_SECRET']
           })
    end
  end
end

response = Learnamp::Auth.access_token
puts response["access_token"] if response.ok?
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

For accounts on EU1 Pod, the authentication URL is:
`POST https://api.learnamp.com/oauth/token`

For accounts on EU2 Pod, the authentication URL is:
`POST https://api-eu2.learnamp.com/oauth/token`

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