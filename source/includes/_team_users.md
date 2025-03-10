# Team Users

## View All Team Users

> View all users for a given team:

```shell
curl --location --request GET 'https://api.learnamp.com/v1/teams/1/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class TeamUsers
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def all(team_id)
      response = self.class.get("/teams/#{team_id}/users", { headers: headers })
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

team_users = Learnamp::TeamUsers.new(token).all(1)
```

View all users for a given team.

### HTTP Request

`GET https://{API_BASE_URL}/v1/teams/{team_id}/users`

### Required Scope
This endpoint requires the `team_users:read` scope.

Response will be paginated [see pagination](#pagination)

## Add a User to a Team

> Add a user to a team:

```shell
curl --location --request POST 'https://api.learnamp.com/v1/teams/1/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
  "userId": 1,
  "isPrimary": true
}'
```

```ruby
module Learnamp
  class TeamUsers
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def add(team_id, user_details)
      response = self.class.post("/teams/#{team_id}/users",
        {
          headers: headers,
          body: user_details.to_json
        }
      )
      response.parsed_response
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}",
        'Content-Type' => 'application/json'
      }
    end
  end
end

user_details = {
  userId: 1,
  isPrimary: true
}

result = Learnamp::TeamUsers.new(token).add(1, user_details)
```

Add a user to a team.

### HTTP Request

`POST https://{API_BASE_URL}/v1/teams/{team_id}/users`

### Required Scope
This endpoint requires the `team_users:create` scope.

## Remove a User from a Team

> Remove a user from a team:

```shell
curl --location --request DELETE 'https://api.learnamp.com/v1/teams/1/users/1' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN'
```

```ruby
module Learnamp
  class TeamUsers
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def remove(team_id, user_id)
      response = self.class.delete("/teams/#{team_id}/users/#{user_id}", { headers: headers })
      response.ok?
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}"
      }
    end
  end
end

success = Learnamp::TeamUsers.new(token).remove(1, 1)
```

Remove a user from a team.

### HTTP Request

`DELETE https://{API_BASE_URL}/v1/teams/{team_id}/users/{user_id}`

### Required Scope
This endpoint requires the `team_users:delete` scope.

## Add Users to a Team

> Add multiple users to a team:

```shell
curl --location --request POST 'https://api.learnamp.com/v1/teams/1/bulk/users' \
--header 'Authorization: Bearer YOUR-ACCESS-TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
  "userIds": [1, 2, 3],
  "isPrimary": true
}'
```

```ruby
module Learnamp
  class TeamUsers
    include HTTParty
    base_uri "#{ENV['BASE_URL']}#{ENV['API_PATH']}"

    attr_accessor :token

    def initialize(token)
      @token = token
    end

    def bulk_add(team_id, users_details)
      response = self.class.post("/teams/#{team_id}/bulk/users",
        {
          headers: headers,
          body: users_details.to_json
        }
      )
      response.parsed_response
    end

    private

    def headers
      {
        'Authorization' => "Bearer #{token}",
        'Content-Type' => 'application/json'
      }
    end
  end
end

users_details = {
  userIds: [1, 2, 3],
  isPrimary: true
}

result = Learnamp::TeamUsers.new(token).bulk_add(1, users_details)
```

Add multiple users to a team in a single request.

### HTTP Request

`POST https://{API_BASE_URL}/v1/teams/{team_id}/bulk/users`

### Required Scope
This endpoint requires the `team_users:create` scope.
