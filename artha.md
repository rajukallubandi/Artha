- [card](#card)
  - [ApplyCard](#ApplyCard)

## API Reference

# Card Activation API

## POST /ApplyCardss

### Summary
Apply for a card using the specified program ID.

### Request

#### Headers
- `Content-Type`: `application/json`

#### Request Body
```json
{
  "programId": "string",
  "KYC":  {
    "firstname": "cameron",
    "lastname": "green",
    "gender": 1,
    "dob": "2000-10-01",
    "nationalityid": "+91",
    "email": "Green@gmail.com",
    "mobilecode": "+91",
    "mobile": "9398889038",
    "address": "hyderbad",
    "town": "Srnagar",
    "city": "city",
    "state": "Ts",
    "zipcode": "500038",
    "countryid": "+91",
    "countryisothree": "countryisothree",
    "emergencycontact": "98709870987",
    "doctype": 1,
    "docid": "1",
    "frontdoc": "frontimg",
    "backdoc": "backimg",
    "docexpiredate": "2025-10-01",
    "docneveexpire": 0,
    "handholdidphoto": "10-01-2025",
    "biomatric": "Finger",
    "photo": "img",
    "signimage": "sigimg"
  }
}
```
| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `programId`      | `string` | **Required**. must be between 1 and 36 bytes in UTF-8 encoding |
|`KYC`|`object`|{}|

### Response
```json
{  
  "taskId": "2qw234e",
  "referenceId": "23456",
  "status": "Submit",
  "remarks": "CardInProgress"
}
```

# Binding

## POST /Binding
### Summary
Binding KYC

### Request

#### Headers
- `Content-Type`: `application/json`

#### Request Body
```json
{
  "taskId": "string",
  "cardNumber": "string",
  "envelopeNo": "string",
   "KYC":  {
    "firstname": "cameron",
    "lastname": "green",
    "gender": 1,
    "dob": "2000-10-01",
    "nationalityid": "+91",
    "email": "Green@gmail.com",
    "mobilecode": "+91",
    "mobile": "9398889038",
    "address": "hyderbad",
    "town": "Srnagar",
    "city": "city",
    "state": "Ts",
    "zipcode": "500038",
    "countryid": "+91",
    "countryisothree": "countryisothree",
    "emergencycontact": "98709870987",
    "doctype": 1,
    "docid": "1",
    "frontdoc": "frontimg",
    "backdoc": "backimg",
    "docexpiredate": "2025-10-01",
    "docneveexpire": 0,
    "handholdidphoto": "10-01-2025",
    "biomatric": "Finger",
    "photo": "img",
    "signimage": "sigimg"
  }
}

```
| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `taskId`      | `string` |  must be between 1 and 36 bytes in UTF-8 encoding |
|`cardNumber`|`string`|**Required**.Card number must be at least 1 byte and no more than 19 bytes in UTF-8 encoding|
|`envelopeNo`|`string`|Envelope number can be null. If provided, it must be between 1 and 15 bytes in UTF-8 encoding|
|`KYC`|`object`|{}|

### Response

```json
{    
  "taskId": "2qw234e",
  "cardno": "2345678",
  "status": "Submit",
  "remarks": "CardInProgress"
}

```
# CardTopUp

## POST /CardTopUp
### Summary
Card Recharge

### Request

#### Headers
- `Content-Type`: `application/json`

### RequestBody

```json
{    
  "taskId": "2qw234e",
  "cardno": "2345678",
  "status": "Submit",
  "remarks": "CardInProgress"
}

```
| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
|`cardId`|`string`|**Required**.Must be between 1 and 36 bytes in UTF-8 encoding|
|`amount`|`string`|**Required**.Must be between 1 and 10 bytes in UTF-8 encoding|

### Response
```json
{  
  "taskId": "2qw234e",
  "referenceId": "23456",
  "status": "Submit",
  "remarks": "CardInProgress"
}
```

# CardFreeze

## POST /CardFreeze
### Summary
Card Lock

### Request

#### Headers
- `Content-Type`: `application/json`

### RequestBody

```json
{    
  "cardId": "2qw234e"
}

```
| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
|`cardId`|`string`|**Required**.Must be between 1 and 36 bytes in UTF-8 encoding|

### Response
```json
{  
 "taskId": "2qw234e",
  "referenceId": "23456",
  "status": "Submit",
  "remarks": "CardInProgress"
}
```
# CardUnFreeze

## POST /CardUnFreeze
### Summary
Card Unlock

### Request

#### Headers
- `Content-Type`: `application/json`

### RequestBody

```json
{    
  "cardId": "2qw234e"
}
```
| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
|`cardId`|`string`|**Required**.Must be between 1 and 36 bytes in UTF-8 encoding|

### Response
```json
{  
 "taskId": "2qw234e",
  "referenceId": "23456",
  "status": "Submit",
  "remarks": "CardInProgress"
}
```
# CancelCard

## POST /CancelCard
### Summary
Card Cancellation

### Request

#### Headers
- `Content-Type`: `application/json`

### RequestBody

```json
{    
  "cardId": "2qw234e"
}
```
| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
|`cardId`|`string`|**Required**.Must be between 1 and 36 bytes in UTF-8 encoding|

### Response
```json
{  
 "taskId": "2qw234e",
  "referenceId": "23456",
  "status": "Submit",
  "remarks": "CardInProgress"
}
```
# EstimateCardTopUpFee

## POST /EstimateCardTopUpFee
### Summary
Card Estimation TopUp Fee

### Request

#### Headers
- `Content-Type`: `application/json`

### RequestBody

```json
{    
  "cardId": "2qw234e",
  "amount": "100"
}
```
| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
|`cardId`|`string`|Must be between 1 and 36 bytes in UTF-8 encoding|
|`amount`|`string`|Must be between 1 and 10 bytes in UTF-8 encoding	|

### Response
```json
{  
 "cardId": "2qw234e",
  "amount": "100",
  "fee": "10",
  "receiveAmount": "30"
}
```
# CardDetails

## Get /CardDetails
### Summary
Card Details

### Request

#### Headers
- `Content-Type`: `application/json`

### RequestBody

```json
{    
  "cardId": "2qw234e"
}
```
| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
|`cardId`|`string`|**Required**.	Must be between 1 and 36 bytes in UTF-8 encoding|


### Response
```json
{  
 "cardNumber": "12134523",
  "cvv": "234",
  "expirationDate": "2025-10-01"
}
```

# PinDetails

## Get /PinDetails
### Summary
Pin Details

### Request

#### Headers
- `Content-Type`: `application/json`

### RequestBody

```json
{    
  "cardId": "2qw234e"
}
```
| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
|`cardId`|`string`|**Required**.	Must be between 1 and 36 bytes in UTF-8 encoding|


### Response
```json
{  
 "cardNumber": "12134523",
  "pin": "234"
}
```
# CardBalance

## Get /CardBalance
### Summary
Card Balance

### Request

#### Headers
- `Content-Type`: `application/json`

### RequestBody

```json
{    
  "cardId": "2qw234e"
}
```
| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
|`cardId`|`string`|**Required**.	Must be between 1 and 36 bytes in UTF-8 encoding|


### Response
```json
{  
 "code": "200",
 "data":
 {
   "available_balance": "100",
    "card_currency": "USD",
    "card_number": "12134523",
    "card_type": "OT",
    "current_balance": "50"
 }
  "msg": "ok"
}
```

# Countries

## Get /Countries
### Summary
Countries

### Request

#### Headers
- `Content-Type`: `application/json`

### Response
```json
{  
 "id": "8ee37608-a700-434b-be5d-ba01fd74182c",
 "name":"India",
  "nationality":"India",
   "isoTwo":"Y",
   "isoThree":"Y",
   "isoNumber":"Y"
 }
 ```
 # Towns

## Get /Towns
### Summary
Towns

### Request

#### Headers
- `Content-Type`: `application/json`

### Response
```json
{  
 "id": "8ee37608-a700-434b-be5d-ba01fd74182c",
 "name":"India",
  "code":"In",
   "countryOrRegion":"Y"
 }
 ```
 # Merchant

## Get /Merchant
### Summary
Merchant

### Request

#### Headers
- `Content-Type`: `application/json`

### Response
```json
{  
 "name": "Jhon",
 "email":"Jhon@gmail.com",
  "mobile":"9398909890",
  "Balance":
  {
  "currency":"USD",
    "currencyCode":"USD",
    "network":"TRC-20",
    "amount":"100"
  },
  "Programs":
  {
  "programId": "550e8400-e29b-41d4-a716-446655440000",
  "bin": 123456,
  "name": "Premium Card",
  "currency": "USD",
  "type": "Debit",
  "consumptionMethod": "Online",
  "isoCountryName": "United States",
  "cardFee": 15.99,
  "cardOpeningFee": 5.00,
  "firstRechargeAmount": 50.00,
  "cancellationFeeMin": 5.00,
  "cancellationFeeMax": 25.00,
  "freightFee": 2.50,
  "rechargeFeeMin": 1.00,
  "rechargeFeeMax": 10.00,
  "transactionFee": 0.50,
  "atmWithdrawalFee": 2.00,
  "maintenanceFee": 1.50,
  "atmBalanceInquiryFee": 0.50,
  "monthlyRechargeLimit": 1000.00,
  "dailyRechargeLimit": 300.00,
  "singleRechargeLimit": 200.00,
  "perPaymentLimit": 150.00,
  "atmDailyWithdrawalLimit": 500.00,
  "spendingLimit": 1000.00,
  "reviewTime": "2024-10-10T12:00:00Z",
  "cardState": "Active",
  "note": "First card issuance",
  "remarks": "For premium users only",
  "isKycRequired": "Yes",
  "supportedOperationTypes": "Purchase, Transfer",
  "cardImage": "https://example.com/images/card.png",
  "supportedPlatforms": "iOS, Android, Web",
  "TopUpTokens":
  {
  "token": "abcdef1234567890",
  "network": "Visa",
  "address": "123 Main St, Anytown, USA"
  }
}
 }
 ```
