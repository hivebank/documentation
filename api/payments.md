# Payments

The accountDetails value for payments is in the following form:

`accountNumber@bankNumber`

The `bankNumber` parameter is used to determine which bank the sender and recipient belong to. Currently, 
only the current deployment is supported. 

If payments are made using an account on the current deployment, the `bankNumber` parameter should be left blank:

`accountNumber@`

## Credit Transfer
HTTP route: `POST /payment/credit`

This route initiates a payment from one party to another. Currently the system only supports payments 
between account holders on that system. There is, however, scope to include this to payments on another given 
deployment of BVNK, as well as account holders at other current banking institutions.

__Request__

```
curl --request POST \
  --url https://demo.bankai.co/transaction/credit \
  --header 'cache-control: no-cache' \
  --header 'content-type: multipart/form-data; boundary=---011000010111000001101001' \
  --header 'x-auth-token: bf48d106-1953-45a3-a9f7-489aa156068b' \
  --form SenderDetails=97890206-89d0-40f1-8162-af76bdf05089@ \
  --form RecipientDetails=89e2b879-c780-4481-a69a-d656a4d1f1ae@ \
  --form Amount=20 \
  --form Lat=11.0 \
  --form Lon=12.0 \
  --form 'Desc=Test desc'
```

__Response__

The ID of the transaction is returned

```
{
  "response": "1423"
}
```

## Deposit
HTTP route: `POST /payment/deposit`

This is used to deposit value into one of the user's own accounts. This route is locked down 
to administrators through HTTP Basic Auth.

__Request__

```
curl --request POST \
  --url https://demo.bankai.co/transaction/deposit \
  --header 'authorization: Basic {{ HTTP Basic Auth }}' \
  --form 'AccountDetails=97890206-89d0-40f1-8162-af76bdf05089@' \
  --form Amount=120 \
  --form Lat=10. \
  --form Lon=10. \
  --form 'Desc=🍊'
```

__Response__

The ID of the transaction is returned

```
{
  "response": "1424"
}
```

## Withdrawal
HTTP route: `POST /payment/withdrawal`

This is used to withdraw value from one of the user's own accounts. This route is locked down 
to administrators through HTTP Basic Auth.

__Request__

```
curl --request POST \
  --url https://demo.bankai.co/transaction/withdrawal \
  --header 'authorization: Basic {{ HTTP Basic Auth }}' \
  --form 'AccountDetails=97890206-89d0-40f1-8162-af76bdf05089@' \
  --form Amount=120 \
  --form Lat=10. \
  --form Lon=10. \
  --form 'Desc=🍊'
```

__Response__

The ID of the transaction is returned

```
{
  "response": "1425"
}
```

## Transaction List
HTTP route: `GET transactions/list/{perPage}/{pageNumber}`

This route returns a list of transactions according to the parameters sent in the request. In the `GET` request,
perPage and pageNumber are sent through to list transactions. Transactions are returned per account linked to a given 
user, and the accountNumber parameter specificies which user account to return transactions for.

The order of transactions returned is from most recent. If a user wants to retrieve all transactions, they would loop through 
transactions using this route increasing pageNumber until less than perPage is returned, or if an empty set is returned.

__Request__

```
curl --request GET \
  --url https://demo.bankai.co/transaction/list/10/0 \
  --header 'cache-control: no-cache' \
  --header 'content-type: multipart/form-data; boundary=---011000010111000001101001' \
  --header 'x-auth-accountnumber: 97890206-89d0-40f1-8162-af76bdf05089' \
  --header 'x-auth-token: 4e8afddd-e151-40ab-835c-2ced3a1ae075' \
```

__Response__

```
{
    "response": [
        {
            "ID": 81,
            "PainType": 1,
            "Sender": {
                "AccountNumber": "97890206-89d0-40f1-8162-af76bdf05089",
                "BankNumber": ""
            },
            "Receiver": {
                "AccountNumber": "89e2b879-c780-4481-a69a-d656a4d1f1ae",
                "BankNumber": ""
            },
            "Amount": "20",
            "Fee": "0.0020000000949949026",
            "Geo": [
                11,
                12
            ],
            "Desc": "Test desc",
            "Status": "approved",
            "Timestamp": 1472465399
        },
        {
            "ID": 80,
            "PainType": 1000,
            "Sender": {
                "AccountNumber": "0",
                "BankNumber": "0"
            },
            "Receiver": {
                "AccountNumber": "97890206-89d0-40f1-8162-af76bdf05089",
                "BankNumber": ""
            },
            "Amount": "120",
            "Fee": "0.012000000104308128",
            "Geo": [
                10,
                10
            ],
            "Desc": "🍊",
            "Status": "approved",
            "Timestamp": 1472379775
        },
	    /* the remaining records up to perPage */
    ]
}
```

## Transaction List After Timestamp
HTTP route: `GET transactions/list/{perPage}/{pageNumber}/{timestamp}`

This route is the same as above, but will only return those records after a given timestamp. The 
timestamp must be a UNIX timestamp.

__Request__

```
curl --request GET \
  --url https://demo.bankai.co/transaction/list/10/0/1472465399 \
  --header 'cache-control: no-cache' \
  --header 'content-type: multipart/form-data; boundary=---011000010111000001101001' \
  --header 'x-auth-accountnumber: 97890206-89d0-40f1-8162-af76bdf05089' \
  --header 'x-auth-token: 4e8afddd-e151-40ab-835c-2ced3a1ae075' \
```

__Response__

```
{
    "response": [
        {
            "ID": 81,
            "PainType": 1,
            "Sender": {
                "AccountNumber": "97890206-89d0-40f1-8162-af76bdf05089",
                "BankNumber": ""
            },
            "Receiver": {
                "AccountNumber": "89e2b879-c780-4481-a69a-d656a4d1f1ae",
                "BankNumber": ""
            },
            "Amount": "20",
            "Fee": "0.0020000000949949026",
            "Geo": [
                11,
                12
            ],
            "Desc": "Test desc",
            "Status": "approved",
            "Timestamp": 1472465399
        }
    ]
}
```
