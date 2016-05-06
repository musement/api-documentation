## Authentication

Authentication is required for each request. With it we can grant you the right permissions to navigate and operate our data. We follow the oAuth2 standard [RFC-6749](https://tools.ietf.org/html/rfc6749) for authentication.

For a good explanation of OAUTH2 see this page on [DigitalOcean](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2)

At the moment we support _Client credentials_ and _Resource Owner Password Credentials_. 

### Client Credentials

As described in the previou article this grant type is useful if you don't have any customer context. Obtain an `Access Token` is very easy and you just need to send a request to the endpoint `/api/v3/login` specifing you `client_id` and `client_secret`
   
##### _Example_

```
GET /api/v3/login?client_id={client_id}&client_secret={client_secret}&grant_type=client_credentials
```

And you'll get an access token. *Please remember* that this access token is not connected to a customer.

### Resource Owner Password Credentials

Use this if you want to authenticate a specific customer and access his personal information ( previous orders, profile, preferences, etc)

The endpoint is again `/api/v3/login` but this time you need to specify also the `username` and the `password` for the user you want to authenticate.

##### _Example_

```
GET /api/v3/login?client_id={client_id}&client_secret={client_secret}&grant_type=password&username={username}&password={password}
```  

### Authentication response  

All the authentication requests, despite the grant type, have the same response. The information you need to save are `access_token` and `refresh_token`. The value of `access_token` is the value you have to pass in the `Authentication` header in all your request.

##### _Example_

A response for an authentication request

```
{
  "access_token": "MWFkZWE5YWUyZTZiNzM3NzFjMzkwZmI3ZDgyM2E2ZWQ0YWFmNDQ1NTM4ZDM4Mzc0MDkyNzMyZWMzNWNkNjQzOA",
  "expires_in": 3600,
  "token_type": "bearer",
  "scope": "user, password",
  "refresh_token": "NWU4ZjcyMzA0ZTVhM2JjY2MyMjU5NDQ4YWYxYzNkZWQwODA5YmVhOTc2YzY0NmFhMWNiYzcwYjIxNzM3NDVmOA"
} 
```

Request with the `Authorization` header.

```
GET /api/v3/cities HTTP/1.1
Host: api.musement.com
Authorization: Bearer MWFkZWE5YWUyZTZiNzM3NzFjMzkwZmI3ZDgyM2E2ZWQ0YWFmNDQ1NTM4ZDM4Mzc0MDkyNzMyZWMzNWNkNjQzOA
```

>  **Please remember that you must include this header in ALL requests**

### Token expirations

Token expires so you must check the status code for all responses. If the response has `401` as status code you must request a new `access_token` using the `refresh_token` that was in the authentication response.

To request a new `access_token` using the `refresh_token` just call the login endpoint passing `refresh_token` as `grant_type`

```
GET /api/v3/login?client_id={client_id}&client_secret={client_secret}&refresh_token={refresh_token}&grant_type=refresh_token
```

The response contains a new `access_token` *and a new* `refresh_token`. You should use these new values from now on. 

