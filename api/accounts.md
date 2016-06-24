# Account
CLI Code: `acmt`

## Open Account
Code: `1` or `7`
HTTP route: `POST` to `/account/`

__Request__
- AccountHolderGivenName
- AccountHolderFamilyName
- AccountHolderDateOfBirth
- AccountHolderIdentificationNumber
- AccountHolderContactNumber1
- AccountHolderContactNumber2
- AccountHolderEmailAddress
- AccountHolderAddressLine1
- AccountHolderAddressLine2
- AccountHolderAddressLine3
- AccountHolderPostalCode

__Response__
- AccountNumber, Error

## Fetch Accounts
CLI Code: `1000`
HTTP route: `GET` on `/account/all`

__Request__
- Token

__Response__
- JSON of `[]AccountDetails`

## Fetch Single Account
CLI Code: `1001`
HTTP route: `GET` on `/account/`

__Request__
- `X-Auth-Token` header

__Response__
- JSON of current user's account `AccountDetails`

## Fetch Single Account By IdentificationNumber
CLI Code: `1002`
HTTP route: `GET` on `/account/{accountId}`

__Request__
- `accountHolderIdentificationNumber`

__Response__
- JSON of requested user's account `AccountDetails`

## Add push token to account
CLI Code: `1003`
HTTP route: `POST` on `/accountPushToken`

__Request__
- `X-Auth-Token` header
- PushToken
- Platform [one of ios, android, windows, blackberry, other]

__Response__
- Message, error

## Delete push token from account
CLI Code: `1004`
HTTP route: `DELETE` on `/accountPushToken`

__Request__
- `X-Auth-Token` header
- PushToken
- Platform [one of ios, android, windows, blackberry, other]

__Response__
- Message, error
