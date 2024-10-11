- [Introduction](#Introduction)
- [Basic Information](#Basic-Information)
    - [API Usage Instructions](#API-Usage-Instructions)
    - [Merchant platform URL](#Merchant-platform-URL)
    - [API Base URL](#API-Base-URL)
    - [Connection Method](#Connection-Method)
    - [API Specification](#API-Specification)
    - [SHA256WithRSA Signature](#SHA256WithRSA-Signature) 
- [API Reference](#API-Reference)	
    - [Merchant Information](#Merchant-Information)
    - [Programdetails](#Programdetails)	
    - [ApplyCard](#ApplyCard)
    - [Binding](#Binding)
    - [CardTopUp](#CardTopUp)
    - [CardFreeze](#CardFreeze)
    - [CardUnFreeze](#CardUnFreeze)
    - [CancelCard](#CancelCard)
    - [EstimateCardTopUpFee](#EstimateCardTopUpFee)
    - [CardDetails](#CardDetails)
    - [PinDetails](#PinDetails)
    - [CardBalance](#CardBalance)
    - [CardUnFreeze](#CardUnFreeze)
    - [Countries](#Countries)
    - [Towns](#Towns)
  


# Introduction
Welcome to the ArthaCard developer documentation. An overview of the merchant docking application development interface.

REST API includes two business categories: bank card and global remittance

# Basic Information

## API Usage Instructions

Before you can use the API, Contact ArthaCard API operator to register the merchant account

Request Client token from Artha Merchant platform, and configure the RSA key, callback address on the Merchant platform. IP must be whitelisted to use the services. 

**CustomerToken:** This token must be included in the request header when accessing the API, using the format **CustomerToken={CustomerToken}.**

Merchant can use the ArthaCard Merchant platform to perform wallet address checking, wallet deposit (USDT)

Recommended to using Test environment for debugging before proceed to the production

## How to generate RSA private and public keys

**1.** <a href="https://www.webdevsplanet.com/post/how-to-generate-rsa-private-and-public-keys?expand_article=1" target="_blank">In your PC (Recommended)</a>

**2.** <a href="https://it-tools.tech/rsa-key-pair-generator" target="_blank">On WEB</a>

## Merchant platform URL

Test environment: https://tstapi.artha.work/api/v1


## API Base URL

Test environment: https://tstapi.artha.work/api/v1


**CustomerToken:** This token must be included in the request header when accessing the API, using the format **CustomerToken={CustomerToken}**.

## API Specification

To ensure security and verify the sender's identity, all open API requests are authenticated using SHA256WithRSA.Both the requester and the receiver must whitelist each other's IP addresses to prevent unauthorized access.

**Lowercase Parameters and URLs:** All parameter names and URLs should be in lowercase.

**Time Zone:** All date and time used in the API must be in UTC. Requesters need to convert their date and time to UTC when using the API.

**Unix Timestamp:** All time and date-related data in the API use Unix timestamps (in seconds).

**JSON Format:** The request body must be in JSON format unless specified otherwise. Use Content-Type: application/json.

| Parameter Name | Type   | Required | Description                                                                   |
|----------------|--------|----------|-------------------------------------------------------------------------------|
| timestamp      | long   | true     | Unix timestamp (in second)                                                    |
| nonce          | string | true     | Random 10 characters string                                                   |
| Client token   | string | true     | Merchant identification, provided by Artha                                    |
| signature      | string | true     | Signature of request body + header request                                    |


## SHA256WithRSA Signature

This section explains how to generate a signature for a request using asymmetric encryption. Both ArthaCard
and the merchant must exchange public keys, which will be used for validating requests.

* **Public Key:** Exchanged between both parties for validation.

* **Private Key:** Retained securely for yourself.

### Steps to Create the Signature

**1.** **Pick and Sort**

* Gather all the request header information and the request body.

* Exclude the signature field and any fields with empty values.

* Sort all the fields by parameter name in ascending ASCII order.

**2.** **Combine**

* Form a new string using the sorted parameters with the format `<parameter name>=<parameter value>`, separating each parameter pair with an  `&` symbol.

* The combined string will be signed using the private key.

**3.** **Invoke Signature Function**

* Use the SHA256WithRSA signature function along with the private key to sign the combined string. The RSA key should be 1024 bits in length.

* Encode the resulting signature in base64 format.

* Insert the base64-encoded signature value into the `signature` field in the request header.

# API Reference

# Merchant Information

**HTTP request**

**Get /Merchant**

**Summary**

Merchant

**Headers**

- **Content-Type:** application/json

**Response**

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
 
 # Programdetails
 
 **HTTP request**

**Get /programdetails/{programid}**

**Summary**

Programdetails

**Request**

**Headers**

- **Content-Type:** application/json



| Parameter | Type    |Required or not | Description                       |
| :-------- | :-------|:---------------| :-------------------------------- |
| programid | string  |        Y        |Must be between 1 and 36 bytes in UTF-8 encoding|

```json
{    
  "programid": "2qw234e"
}
```


**Response**
```JSON
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
```

## ApplyCard

**HTTP request**

**POST /ApplyCard**

**Summary**

Apply for a card using the specified program ID.

**Request**

**Headers**

- **Content-Type:** application/json

**Request Body**

| Parameter | Type    |Required or not| Description                       |
| :-------- | :-------|:--------------| :-------------------------------- |
| programId | string  |       Y       | must be between 1 and 36 bytes in UTF-8 encoding |
| kyc       | object  |               |{}                                 |


| Parameter        | Type   |Required or not| Description                          |
|:-----------------|:------ |:--------------|:-----------------------------------
|firstname	       |string	|Required	    |First name of the individual              |
|lastname	       |string	|Required	    |Last name of the individual               |
|gender	           |integer |Required	    |Gender of the individual (0 for unspecified)|
|dob	           |string	|Required	    |Date of birth                              |
|nationalityid     |string	|Required	    |Nationality ID                             |
|email	           |string	|Required	    |Email                                      |
|mobilecode	       |string	|Required	    |Mobile code (country code)                 |
|mobile	           |string	|Required	    |Mobile number                              |
|address	       |string	|Required	    |Residential address                        |
|town	           |string	|Required	    |Town or locality                           |
|city	           |string	|Required	    |City                                       |
|state	           |string	|Required	    |State or region                            |
|zipcode	       |string	|Required	    |Postal code                                |
|countryid	       |string	|Required	    |Country ID                                 |
|countryisothree   |string	|Required	    |ISO 3166-1 alpha-3 country code            |
|emergencycontact  |string  |Required	    |Emergency contact number                   |
|doctype	       |integer |Required	    |Document type (0 for unspecified)          |
|docid	           |string	|Required	    |Document ID                                |
|frontdoc	       |string	|Required	    |Front image of the document                |
|backdoc	       |string	|Required	    |Back image of the document                 |
|docexpiredate     |string	|Required	    |Expiry date of the document                |
|docneveexpire     |integer |Required	    |Indicates if the document has never expired (0 for no)|
|handholdidphoto   |string	|Required	    |Handheld ID photo                           |
|biomatric	       |string	|Required	    |Biometric data                              |
|photo	           |string	|Required	    |Personal photo                              |
|signimage	       |string	|Required	    |Signature image                             |


```json
{
  "programId": "string",
  "kyc":  {
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


**Response**

```json
{  
  "taskId": "2qw234e",
  "referenceId": "23456",
  "status": "Submit",
  "remarks": "CardInProgress"
}
```

## Binding

**HTTP request**

**POST /Binding**

**Summary**

Binding KYC

**Request**

**Headers**

- **Content-Type:** application/json

**Request Body**

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| taskId    | string   |  must be between 1 and 36 bytes in UTF-8 encoding |
| cardNumber| string   |**Required.** Card number must be at least 1 byte and no more than 19 bytes in UTF-8 encoding|
|envelopeNo | string   |Envelope number can be null. If provided, it must be between 1 and 15 bytes in UTF-8 encoding|
| kyc       | object   |{}                                                                                           |


| Parameter        | Type   |Required or not| Description                          |
|:-----------------|:------ |:--------------|:-----------------------------------
|firstname	       |string	|Required	    |First name of the individual              |
|lastname	       |string	|Required	    |Last name of the individual               |
|gender	           |integer |Required	    |Gender of the individual (0 for unspecified)|
|dob	           |string	|Required	    |Date of birth                              |
|nationalityid     |string	|Required	    |Nationality ID                             |
|email	           |string	|Required	    |Email                                      |
|mobilecode	       |string	|Required	    |Mobile code (country code)                 |
|mobile	           |string	|Required	    |Mobile number                              |
|address	       |string	|Required	    |Residential address                        |
|town	           |string	|Required	    |Town or locality                           |
|city	           |string	|Required	    |City                                       |
|state	           |string	|Required	    |State or region                            |
|zipcode	       |string	|Required	    |Postal code                                |
|countryid	       |string	|Required	    |Country ID                                 |
|countryisothree   |string	|Required	    |ISO 3166-1 alpha-3 country code            |
|emergencycontact  |string  |Required	    |Emergency contact number                   |
|doctype	       |integer |Required	    |Document type (0 for unspecified)          |
|docid	           |string	|Required	    |Document ID                                |
|frontdoc	       |string	|Required	    |Front image of the document                |
|backdoc	       |string	|Required	    |Back image of the document                 |
|docexpiredate     |string	|Required	    |Expiry date of the document                |
|docneveexpire     |integer |Required	    |Indicates if the document has never expired (0 for no)|
|handholdidphoto   |string	|Required	    |Handheld ID photo                           |
|biomatric	       |string	|Required	    |Biometric data                              |
|photo	           |string	|Required	    |Personal photo                              |
|signimage	       |string	|Required	    |Signature image                             |

```json
{
  "taskId": "string",
  "cardNumber": "string",
  "envelopeNo": "string",
   "kyc":  {
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

**Response**

```json
{    
  "taskId": "2qw234e",
  "cardno": "2345678",
  "status": "Submit",
  "remarks": "CardInProgress"
}

```
## CardTopUp
**HTTP request**

**POST /CardTopUp**

**Summary**

Card Recharge

**Request**

**Headers**

- **Content-Type:** application/json

**RequestBody**

| Parameter | Type    |Required or not | Description                       |
| :-------- | :-------|:-------------- | :-------------------------------- |
| cardId    | string  |       Y         | Must be between 1 and 36 bytes in UTF-8 encoding|
| amount    | string  |       Y         | Must be between 1 and 10 bytes in UTF-8 encoding|


```json
{    
  "taskId": "2qw234e",
  "cardno": "2345678",
  "status": "Submit",
  "remarks": "CardInProgress"
}

```

**Response**

```json
{  
  "taskId": "2qw234e",
  "referenceId": "23456",
  "status": "Submit",
  "remarks": "CardInProgress"
}
```

## CardFreeze

**HTTP request**

**POST /CardFreeze**

**Summary**

Card Lock

**Request**

**Headers**

- **Content-Type:** application/json

**RequestBody**

| Parameter | Type    |Required or not | Description                       |
| :-------- | :-------|:-------------- | :-------------------------------- |
| cardId    | string  |       Y         | Must be between 1 and 36 bytes in UTF-8 encoding|

```json
{    
  "cardId": "2qw234e"
}

```
**Response**

```json
{  
 "taskId": "2qw234e",
  "referenceId": "23456",
  "status": "Submit",
  "remarks": "CardInProgress"
}
```
## CardUnFreeze

**HTTP request**

**POST /CardUnFreeze**

**Summary**

Card Unlock

**Request**

**Headers**

- **Content-Type:** application/json

**RequestBody**

| Parameter | Type    |Required or not | Description                       |
| :-------- | :-------|:-------------- | :-------------------------------- |
| cardId    | string  |       Y         | Must be between 1 and 36 bytes in UTF-8 encoding|

```json
{    
  "cardId": "2qw234e"
}

```
**Response**

```json
{  
 "taskId": "2qw234e",
  "referenceId": "23456",
  "status": "Submit",
  "remarks": "CardInProgress"
}
```
## CancelCard

**HTTP request**

**POST /CancelCard**

**Summary**

Card Cancellation

**Request**

**Headers**

- **Content-Type:** application/json

**RequestBody**

| Parameter | Type    |Required or not | Description                       |
| :-------- | :-------|:-------------- | :-------------------------------- |
| cardId    | string  |       Y         | Must be between 1 and 36 bytes in UTF-8 encoding|

```json
{    
  "cardId": "2qw234e"
}
```

**Response**

```json
{  
 "taskId": "2qw234e",
  "referenceId": "23456",
  "status": "Submit",
  "remarks": "CardInProgress"
}
```

## EstimateCardTopUpFee

**HTTP request**

**POST /EstimateCardTopUpFee**

**Summary**

Card Estimation TopUp Fee

**Request**

**Headers**

- **Content-Type:** application/json

**RequestBody**

| Parameter | Type    |Required or not  | Description                       |
| :-------- | :-------|:----------------| :-------------------------------- |
|  cardId   | string  |       N         |Must be between 1 and 36 bytes in UTF-8 encoding|
|  amount   | string  |       N         |Must be between 1 and 10 bytes in UTF-8 encoding	|

```json
{    
  "cardId": "2qw234e",
  "amount": "100"
}
```

**Response**

```json
{  
 "cardId": "2qw234e",
  "amount": "100",
  "fee": "10",
  "receiveAmount": "30"
}
```

## CardDetails

**HTTP request**

**Get /CardDetails/{cardId}**

**Summary**

Card Details

**Request**

**Headers**

- **Content-Type:** application/json

| Parameter | Type     |Required or not| Description                       |
| :-------- | :------- |:--------------| :-------------------------------- |
| cardId    | string   |       Y       |Must be between 1 and 36 bytes in UTF-8 encoding|


**Response**

```json
{  
 "cardNumber": "12134523",
  "cvv": "234",
  "expirationDate": "2025-10-01"
}
```

## PinDetails

**HTTP request**

**Get /PinDetails/{cardId}**

**Summary**

Pin Details

**Request**

**Headers**

- **Content-Type:** application/json

| Parameter | Type     |Required or not |Description                       |
| :-------- | :------- |:-------------- |:-------------------------------- |
| cardId    | string   |       Y         |Must be between 1 and 36 bytes in UTF-8 encoding|


**Response**

```json
{  
 "cardNumber": "12134523",
  "pin": "234"
}
```
## CardBalance

**HTTP request**

**Get /CardBalance**

**Summary**

Card Balance

**Request**

**Headers**

- **Content-Type:** application/json


| Parameter | Type     |Required or not |Description                       |
| :-------- | :------- |:-------------- |:-------------------------------- |
| cardId    | string   |       Y         |Must be between 1 and 36 bytes in UTF-8 encoding|

**Response**

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
 },

  "msg": "ok"
}
```

## Countries

**HTTP request**

**Get /Countries**

**Summary**

Countries

**Headers**

- **Content-Type:** application/json

**Response**

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

## Towns

**HTTP request**

**Get /Towns**

**Summary**

Towns

**Headers**

- **Content-Type:** application/json

**Response**

```json
{  
 "id": "8ee37608-a700-434b-be5d-ba01fd74182c",
 "name":"India",
  "code":"In",
   "countryOrRegion":"Y"
 }
 ```


