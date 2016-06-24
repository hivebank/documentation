# App auth
CLI Code: `appauth`

## Auth existing
_Extends token_
CLI Code: `1`
HTTP route: `POST` to `/auth`

__Request__
- `X-Auth-Token` header

__Response__
- Message, error

## Log in / Get token
CLI Code: `2`
HTTP route: `POST` to `/auth/login`

__Request__
- User
- Password

__Response__
- Token, error

## Create auth account
CLI Code: `3`
HTTP route: `POST` to `/auth/account`

__Request__
- User
- Password

__Response__
- Token, error

## Remove auth account
CLI Code: `4`
HTTP route: `DELETE` to `/auth/account`

__Request__
- User
- Password (hashed using `sha512`)

__Response__
- Message, error
