# App auth

The application authentication process is as follows:

- Create authentication credentials for a given user
- Log in using said authentication credentials, token returned 
- Use token as `X-Auth-Token` header in authenticated requests
- On every successfull authenticated request, the token's TTL is extended.
- Optionally extend token 

## Auth Create
HTTP route: `POST auth/account/`

This will create an authentication account against a given user. The returned value will be used
as a unique identifier against a given user, with a one to one relationship to userIdentificationNumbers.

__Request__

```
curl --request POST \
  --url https://bvnk.co:8443/auth/account \
  --header 'cache-control: no-cache' \
  --header 'content-type: multipart/form-data; boundary=---011000010111000001101001' \
  --form UserIdentificationNumber=19000101-1234-124 \
  --form Password=TestPassword
```

__Response__

```
{
  "result": "2fe198f4-b953-4766-9d2a-bb6e9bc9be4f"
}
```

If an account already exists for the given user:

```
{
  "error": "appauth.CreateUserPassword: Account already exists: 2fe198f4-b953-4766-9d2a-bb6e9bc9be4f"
}
```

## Auth Login
HTTP route: `POST auth/login/`

This is used to retrieve a token to use in subsequent authenticated requests.

__Request__

```
curl --request POST \
  --url https://bvnk.co:8443/auth/login \
  --header 'cache-control: no-cache' \
  --header 'content-type: multipart/form-data; boundary=---011000010111000001101001' \
  --form User=2fe198f4-b953-4766-9d2a-bb6e9bc9be4f \
  --form Password=TestPassword
```

__Response__

```
{
  "response": "4e8afddd-e151-40ab-835c-2ced3a1ae075"
}
```

## Auth Extend
HTTP route: `POST auth/`

This is used to extend a token's TTL.

__Request__

```
curl --request POST \
  --url https://bvnk.co:8443/auth \
  --header 'cache-control: no-cache' \
  --header 'x-auth-token: f2e55643-f89d-4de3-a427-1be162c24c75'
```

__Response__

A successful response is an empty response with no error.

```
{
  "response": ""
}
```

An errored response looks as follows:

```
{
  "error": "httpApiHandlers: Token invalid"
}
```

## Auth Remove
HTTP route: `DELETE auth/account/`

This route removes associated authentication credentials from a given user. The hashed password is sent through.
The password is constructed using a unique user salt and the clear text password. 

_Note: currently this salt is not 
sent through during any requests, and as such this must be sent manually across a channel to the user by an administrator. 
We are investigating secure implementations of this._

__Request__

```
curl --request DELETE \
  --url https://bvnk.co:8443/auth/account \
  --header 'cache-control: no-cache' \
  --header 'content-type: multipart/form-data; boundary=---011000010111000001101001' \
  --header 'x-auth-token: 4e8afddd-e151-40ab-835c-2ced3a1ae075' \
  --form User=8e6176f6-58f1-4a5f-b120-2fb4799c2acd \
  --form Password=873343a194a15a840f9b0f4798ad51bd5784f0fb4c2690c7478aeee7e4159f02f435da9505499bef1eeb5036a160c0527d34b7cf3260ed613b0c82417b169659
```

__Response__

```
{
  "result": ""
}
```
