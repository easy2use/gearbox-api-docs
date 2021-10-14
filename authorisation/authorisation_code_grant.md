# Authorisation Code Grant

https://tools.ietf.org/html/rfc6749#section-4.1

You will need an active administrator account for Gearbox to accept the connecting application. If you do not already have one then please contact Gearbox Support.

## Examples 

### Ruby/Rails

1. Grab an (OAuth 2.0 library)[https://oauth.net/code/].

2. Configure your library with your `client_id`, `client_secret`, `redirect_uri` and `state`. Tell the client to use https://api.gearbox.com.au/oauth/authorize to request authorisation.

```Ruby
oauth_client = OAuth2::Client.new(
  'client_id',
  'client_secret',
  site: 'https://api.gearbox.com.au'
)

redirect_to oauth_client.auth_code.authorize_url(
  redirect_uri: 'https://www.example.com.au/oauth/auth_callback',
  state: '123456'
)
# => "https://api.gearbox.com.au/oauth/authorize?response_type=code&client_id=client_id&redirect_uri=https://www.example.com.au/oauth/callback&state=123456"
```

3. Once the user has confirmed access for the application in Gearbox, in the callback URL request an access token using the code recieved via https://api.gearbox.com.au/oauth/token. Please ensure the `state` value passed in step 2 has remained the same.
```Ruby
if params[:state] == '123456'
  access_token = oauth_client.auth_code.get_token(
    params[:code],
    redirect_uri: 'https://www.example.com.au/oauth/token_callback'
  )
end
```

4. The token can be refreshed after it has expired or revoked if you no longer plan to use it.