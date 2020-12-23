# Gearbox API

Gearbox is REST-style API that uses JSON for serialization and OAuth 2.0 for authentication. To get started you must first contact Gearbox Support to request your Client ID and Client Secret to successfully authenticate your application.

You must also have an active administrator login to Gearbox to connect successfully to the Gearbox API. If you do not already have one then please contact Gearbox Support.

## JSON only

We use JSON for all API data. The style is no root element and snake_case for object keys. This means that you have to send the Content-Type header `Content-Type: application/json` when you're POSTing data into Gearbox.

You'll receive a 415 Unsupported Media Type response code if you don't include the Content-Type header.

## Handling errors

API clients must expect and gracefully handle transient errors, such as rate limiting, data or server errors.

### 4xx Client Error

If a response returns a 4xx status code all changes will be rolled back and you must fix up and re-send the request.

### Rate limiting (503 Service unavailable)

You can perform up to 20 requests per 10-second period from the same IP address for the same account. If you exceed this limit, you'll get a 503 Service unavailable for subsequent requests.

### 5xx server errors

If Gearbox is having trouble, you will get a response with a 5xx status code indicating a server error. 500 (Internal Server Error), 502 (Bad Gateway) and 504 (Gateway Timeout) may be retried with exponential backoff.

## OAuth 2 Authentication (Ruby/Rails example)

Gearbox uses OAuth 2 Authorization Code Grant - https://tools.ietf.org/html/rfc6749#section-4.1

1. Grab an (OAuth 2 library)[https://oauth.net/code/].

2. Configure your library with your `client_id`, `client_secret`, `redirect_uri` and `state`. Tell the client to use https://api.gearbox.com.au/oauth/authorize to request authorization.

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

## Making a request

All URLs start with https://api.gearbox.com.au/public/. URLs are HTTPS only.

To make a request for all endpoints you must include the OAuth 2 Access token in your request header (curl example):
```shell
curl -H "Authorization: Bearer $ACCESS_TOKEN" -H 'Content-Type: application/json'
```

### Prestarts

URL: https://api.gearbox.com.au/public/v1/prestarts
Method: POST

```
prestarts: [{
  fleet_number: "", // string, must match existing fleet number in system, required
  completed_at: "", // datetime, required, format: yyyy-mm-dd hh:mm:ss
  employee: "", // string, required. If a match is not found then it is stored as a string
  odometer: "", // integer, not required
  hours: "", // integer, not required
  spare1: "", // string, maximum 256 characters, not required
  spare2: "", // string, maximum 256 characters, not required
  spare3: "", // string, maximum 256 characters, not required
  latitude: "", // float, not required
  longitude: "", // float, not required
  notes: "", // string, maximum 500 characters, not required
  items: [{ // required
    item_question: "", // string, maximum 256 characters, required
    passed: "", // boolean; ‘true’ or ‘false’, required
    fail_reason: "" // string, maximum 256 characters, not required
  }]
}]
```

Status Codes:
 - 200: OK
 - 400: Bad Request
 - 413: Request Entity Too Large

### Fault Reports

URL: https://api.gearbox.com.au/public/v1/fault_reports
Method: POST

```
fault_reports: [{
  fleet_number: “”, // string, must match existing fleet number in system, required
  created_at: “”, // datetime, required, format: yyyy-mm-dd hh:mm:ss
  employee: “”, // string, required. If a match is not found then it is stored as a string
  fail_reason: “”, // string, maximum 256 characters, not required
  notes: “” // string, maximum 500 characters, not required
}]
```

Status Codes:
 - 200: OK
 - 400: Bad Request
 - 413: Request Entity Too Large
