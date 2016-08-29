# Account

## Open Account
HTTP route: `POST account/`

This route is used to open an account against a given user. If the user does not exist on the system, an `accounts_user`
is created as well as an `accounts` entry. If a user does already exist on the system, only a new account is created against the user.
Users are linked to accounts through their userIdentificationNumber with a one to many relationship.

__Request__

```
curl --request POST \
  --url https://bvnk.co:8443/account \
  --header 'cache-control: no-cache' \
  --header 'content-type: multipart/form-data; boundary=---011000010111000001101001' \
  --form AccountHolderGivenName=Test \
  --form AccountHolderFamilyName=User1 \
  --form AccountHolderDateOfBirth=1900-01-01 \
  --form AccountHolderIdentificationNumber=19000101-1234-124 \
  --form 'AccountHolderContactNumber1=(555) 555-1234' \
  --form 'AccountHolderContactNumber2=(555) 555-4321' \
  --form AccountHolderEmailAddress=test@user.com \
  --form 'AccountHolderAddressLine1=Address 1' \
  --form 'AccountHolderAddressLine2=Address 2' \
  --form 'AccountHolderAddressLine3=Address 3' \
  --form AccountHolderPostalCode=1234 \
  --form AccountType=savings
```

__Response__

```
{
  "response": "74d95847-69e7-483e-8f37-115f99d0261b"
}
```

## Get Account
HTTP route: `GET account/`

This call will respond with all details for all accounts linked to a given user. This will return sensitive
information, and is only accessible by the user who owns those accounts.

__Request__

```
curl --request GET \
  --url https://bvnk.co:8443/account \
  --header 'cache-control: no-cache' \
  --header 'x-auth-token: b0baa538-6967-436e-ba03-f1904d1b0dd5'
```

__Response__
JSON list of accounts linked to user making request. The user is derived from the authToken in the header.

```
{
    "response": [
        {
            "AccountNumber": "97890206-89d0-40f1-8162-af76bdf05089",
            "BankNumber": "a0299975-b8e2-4358-8f1a-911ee12dbaac",
            "AccountHolderName": "User1,Test",
            "AccountBalance": "259.9440002441406",
            "Overdraft": "0",
            "AvailableBalance": "259.9440002441406",
            "Type": "",
            "Timestamp": 0
        },
        {
            "AccountNumber": "de416ece-16da-4de2-ae3d-45c704db49c6",
            "BankNumber": "a0299975-b8e2-4358-8f1a-911ee12dbaac",
            "AccountHolderName": "User1,Test",
            "AccountBalance": "100",
            "Overdraft": "0",
            "AvailableBalance": "100",
            "Type": "",
            "Timestamp": 0
        },
    ]
}
```

## Account Get by ID
HTTP route: `GET account/{userIdentificationNumber}`

This call will repsond with all accountNumbers linked to a given userIdentificationNumber. There may be 
security implications involved around this and these will be investigated as the project matures.

__Request__

```
curl --request GET \
  --url https://bvnk.co:8443/account/19000101-1234-124 \
  --header 'cache-control: no-cache' \
  --header 'x-auth-token: bf48d106-1953-45a3-a9f7-489aa156068b'
```


__Response__
JSON list of accountNumbers linked to user.

```
{
    "response": [
        "97890206-89d0-40f1-8162-af76bdf05089",
        "de416ece-16da-4de2-ae3d-45c704db49c6",
        "e7ddaafa-cf53-4e6f-9bb7-e2b71be58557",
        "e7ddaafa-cf53-4e6f-9bb7-e2b71be58557",
        "bb2c84bb-6c05-403a-80de-bf24abd5e6f8",
        "bb2c84bb-6c05-403a-80de-bf24abd5e6f8",
        "7e01e7ea-606d-44f8-a5f0-efdb11162100",
        "7e01e7ea-606d-44f8-a5f0-efdb11162100",
        "4c1743fe-c5b6-457b-a572-2cb06051f27a",
        "4c1743fe-c5b6-457b-a572-2cb06051f27a",
        "74d95847-69e7-483e-8f37-115f99d0261b",
        "74d95847-69e7-483e-8f37-115f99d0261b"
    ]
}
```

## Account Search
HTTP route: `POST account/search/`

Search for an account using one of: userGivenName, userFamilyName, userEmailAddress. The search is case-sensitive.

__Request__

```
curl --request POST \
  --url https://bvnk.co:8443/account/search \
  --header 'cache-control: no-cache' \
  --header 'content-type: multipart/form-data; boundary=---011000010111000001101001' \
  --form Search=Test
```

__Response__
JSON list of accountDetails matching the search term.

```
{
    "response": [
        {
            "GivenName": "Kyle",
            "FamilyName": "Test",
            "DateOfBirth": "",
            "IdentificationNumber": "",
            "ContactNumber1": "",
            "ContactNumber2": "",
            "EmailAddress": "kyle@ksred.me",
            "AddressLine1": "",
            "AddressLine2": "",
            "AddressLine3": "",
            "PostalCode": ""
        },
        {
            "GivenName": "Test",
            "FamilyName": "User",
            "DateOfBirth": "",
            "IdentificationNumber": "",
            "ContactNumber1": "",
            "ContactNumber2": "",
            "EmailAddress": "test@user.com",
            "AddressLine1": "",
            "AddressLine2": "",
            "AddressLine3": "",
            "PostalCode": ""
        },
    ]
}
```

## Account Retrieve
HTTP route: `GET accountRetrieve`

The accountRetrieve route is used for retrieving account numbers linked to a given user using their userIdentificationNumber,
firstName, familyName and emailAddress. These values are sent through as header values in order for the data to not be logged in 
access logs.

_Note: possible duplicate of `accountGetByID`_

__Request__

```
curl --request GET \
  --url https://bvnk.co:8443/accountRetrieve \
  --header 'cache-control: no-cache' \
  --header 'postman-token: 4b4a0378-2411-27a8-6e76-a18365cdc27a' \
  --header 'x-emailaddress: new@ios.com' \
  --header 'x-familyname: IOS' \
  --header 'x-givenname: New' \
  --header 'x-idnumber: 567839924223432'
```

__Response__

```
{
    "response": [
        "fe80db5f-ae70-464e-8b08-f4e252703981"
    ]
}
```

## Account Delete
There is currently no route for accountDelete. This is to ensure accounts are deleted only by administrators.

Account deletion requests through this route going into pending state will be implemented in the future.

## Add Account Push Token
HTTP route: `POST accountPushToken/`

This route adds a push token to an account with an associated device. This is used when notifying users of activity on their account.
Possible `Platform` values: ios, android, windows, blackberry, other.

__Request__

```
curl --request POST \
  --url https://bvnk.co:8443/accountPushToken \
  --header 'cache-control: no-cache' \
  --header 'content-type: multipart/form-data; boundary=---011000010111000001101001' \
  --header 'x-auth-token: 0da2796a-3ff7-424c-aa47-148124a060a1' \
  --form PushToken=test-push-token \
  --form Platform=ios
```

__Response__

```
{
      "response": null
}
```

## Remove Account Push Token
HTTP route: `DELETE accountPushToken/`

This route removes an associated push token from an account and device.

__Request__

```
curl --request DELETE \
  --url https://bvnk.co:8443/accountPushToken \
  --header 'cache-control: no-cache' \
  --header 'content-type: multipart/form-data; boundary=---011000010111000001101001' \
  --header 'x-auth-token: b9b41dc2-9a78-4872-b653-fdd9b1bedf8d' \
  --form PushToken=test-push-token \
  --form Platform=ios
```

__Response__

```
{
  "response": null
}
```
