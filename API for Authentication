## 2. API for Authentication

Authentication is required for each request. With it we can grant you the right permissions to navigate and operate our data. We follow the oAuth2 standard [RFC-6749](https://tools.ietf.org/html/rfc6749) for authentication.

We have support for: 

* Client Credentials
* Resource Owner Password Credentials

### 2.1 Request

#### 2.1.1 Client Credentials

Use this to authenticate yourself with your integration credentials. You will have to use two values:

* client_id 
* client_secret
   
```
GET /api/v3/login?client_id={client_id}&client_secret={client_secret}&grant_type=client_credentials
```

#### 2.1.2 Resource Owner Password Credentials

  Use this if you want to authenticate a specific customer in a way to allow him to access to his information (like previous orders, profile, preferences, etc)

In this case you need these values:

* client_id 
* client_secret
* username
* password

```
GET /api/v3/login?client_id={client_id}&client_secret={client_secret}&grant_type=password&username={username}&password={password}
```  

### 2.2 Response  

In both cases (if you grant type credentials or password) the response will be:

```
{
  "access_token": "MWFkZWE5YWUyZTZiNzM3NzFjMzkwZmI3ZDgyM2E2ZWQ0YWFmNDQ1NTM4ZDM4Mzc0MDkyNzMyZWMzNWNkNjQzOA",
  "expires_in": 3600,
  "token_type": "bearer",
  "scope": "user, password",
  "refresh_token": "NWU4ZjcyMzA0ZTVhM2JjY2MyMjU5NDQ4YWYxYzNkZWQwODA5YmVhOTc2YzY0NmFhMWNiYzcwYjIxNzM3NDVmOA"
} 
```

You have to use the ```access_token``` for each request, for example to ```GET Cities```:

```
GET /api/v3/cities HTTP/1.1
Host: api.musement.com
Authorization: Bearer MWFkZWE5YWUyZTZiNzM3NzFjMzkwZmI3ZDgyM2E2ZWQ0YWFmNDQ1NTM4ZDM4Mzc0MDkyNzMyZWMzNWNkNjQzOA
```
