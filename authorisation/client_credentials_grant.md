# Client Credentials Grant

https://datatracker.ietf.org/doc/html/rfc6749#section-4.4

## Examples

### cURL

```
curl --location --request POST 'https://api.gearbox.com.au/oauth/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'client_id=$CLIENT_ID' \
--data-urlencode 'client_secret=$CLIENT_SECRET' \
--data-urlencode 'business_token=$BUSINESS_TOKEN'
```

### Ruby/Rails

1. Grab an (OAuth 2.0 library)[https://oauth.net/code/].

2. Configure your library with your `client_id` and `client_secret`. Tell the client to use https://api.gearbox.com.au/oauth/authorize to request authorisation (if required).

```Ruby
client = OAuth2::Client.new(
  'client_id',
  'client_secret',
  site: 'https://api.gearbox.com.au'
)
```

3. Request the access token also remembering to include the business token required, which any administration user access can provide by navigating to Settings->Integrations:

```Ruby
token = client.client_credentials.get_token(
  business_token: '$BUSINESS_TOKEN'
)
```

4. Repeat steps 2 to 3 to retrieve a new access token once it has expired.
