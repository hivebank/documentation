# Payments
CLI Code: `pain`

## Credit Transfer
CLI Code: `1`
HTTP route: `POST` to `/payment/credit`

__Request__
- `X-Auth-Token` header
- SenderAccount@SenderBankAccount
- ReceiverAccount@ReceiverBankAccount
- Amount

__Response__
- Message, error

## Deposit
CLI Code: `1000`
HTTP route: `POST` to `/payment/deposit`

__Request__
- `X-Auth-Token` header
- AccountNumber@BankNumber [Must be token user's account]
- Amount

__Response__
- Message, token
