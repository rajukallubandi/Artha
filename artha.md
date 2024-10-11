- [Introduction](#Introduction)
- [Basic Information](#Basic-Information)
    - [API Usage Instructions](#API-Usage-Instructions)
    - [Merchant platform URL](#Merchant-platform-URL)
    - [API Base URL](#API-Base-URL)
    - [Connection Method](#Connection-Method)
    - [API Specification](#API-Specification)
    - [SHA256WithRSA Signature](#SHA256WithRSA-Signature) 
	- [Callback Notification Callback Parameters and Template](#Callback-Notification-Callback-Parameters-and-Template)
    - [Callback Notification Response Parameters and Template](#Callback-Notification-Response-Parameters-and-Template)
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
- [Callback notification](#Callback-notification)
       - [Set callback notification url](#Set-callback-notification-url)
       - [Callback notification type](#Callback-notification-type)
	   - [Global Express Remittance Payment result callback notification](#Global-Express-Remittance-Payment-result-callback-notification)
	   - [Global Express Remittance Adjustment callback notification](#Global-Express-Remittance-Adjustment-callback-notification)
	   - [User KYC callback notification](#User-KYC-callback-notification)
	   - [Bank card callback notification of card recharge result](#Bank-card-callback-notification-of-card-recharge-result)
	   - [Bank card activation result callback notification](#Bank-card-activation-result-callback-notification)
       - [Bank card freeze thaw processing status callback notification](#Bank-card-freeze-thaw-processing-status-callback-notification)
	   - [Bank card 3DS verification](#Bank-card-3DS-verification)


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


## Callback Notification Callback Parameters and Template
| Parameters | Type | Whether it is required | Meaning |
|:-------------|:---------|:------------|:-----------|
| taskId | String | Y | response body |
| Remarks | String | Y | Request flow id. 20 random characters |
| Status	 | String | Y | Merchant ID. PayouCard assigned to merchants |
| opt_code | String | Y | signature |
| notifyType | String | Y | notification type |
| amount	 | String | Y | notification type |
| CreationTime	 | integer | Y | notification type |

## Callback Notification Response Parameters and Template
| Parameters | Type | Whether it is required | Meaning |
|:------------|:--------|:------------|:-----------|
| code | Integer | Y | response code |
| message | String | Y | message |
```json
{
    "code": 0,
    "message": ""
}
```

# API Reference

# Merchant Information

**HTTP request**

**Get /MerchantInformation/Merchant**

**Summary**

Merchant

**Headers**

- **Content-Type:** application/json

**Response Example**

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

**Get /MerchantInformation/programdetails/{programid}**

**Summary**

Programdetails

**Request**

**Headers**

- **Content-Type:** application/json



| Parameter | Type    |Required or not | Description                       |
| :-------- | :-------|:---------------| :-------------------------------- |
| programid | string  |        Y        |Must be between 1 and 36 bytes in UTF-8 encoding|

```path parameter
{    
  "programid": "76ddcaab-55c4-46e0-8d80-e7d097bfc1b3"
}
```


**Response Example**
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
|firstname	       |string	|        	    |First name of the individual              |
|lastname	       |string	|               |Last name of the individual               |
|gender	           |integer |       	    |Gender of the individual (0 for unspecified)|
|dob	           |string	|               |Date of birth                              |
|nationalityid     |string	|	            |Nationality ID                             |
|email	           |string	|	            |Email                                      |
|mobilecode	       |string	|	            |Mobile code (country code)                 |
|mobile	           |string	|               |Mobile number                              |
|address	       |string	|	            |Residential address                        |
|town	           |string	|               |Town or locality                           |
|city	           |string	|	            |City                                       |
|state	           |string	|	            |State or region                            |
|zipcode	       |string	|	            |Postal code                                |
|countryid	       |string	|	            |Country ID                                 |
|countryisothree   |string	|	            |ISO 3166-1 alpha-3 country code            |
|emergencycontact  |string  |	            |Emergency contact number                   |
|doctype	       |integer |	            |Document type (0 for unspecified)          |
|docid	           |string	|	            |Document ID                                |
|frontdoc	       |string	|	            |Front image of the document                |
|backdoc	       |string	|	            |Back image of the document                 |
|docexpiredate     |string	|	            |Expiry date of the document                |
|docneveexpire     |integer |	            |Indicates if the document has never expired (0 for no)|
|handholdidphoto   |string	|	            |Handheld ID photo                          |
|biomatric	       |string	|	            |Biometric data                             |
|photo	           |string	|	            |Personal photo                             |
|signimage	       |string	|               |Signature image                            |


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


**Response Example**

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
|firstname	       |string	|        	    |First name of the individual              |
|lastname	       |string	|               |Last name of the individual               |
|gender	           |integer |       	    |Gender of the individual (0 for unspecified)|
|dob	           |string	|               |Date of birth                              |
|nationalityid     |string	|	            |Nationality ID                             |
|email	           |string	|	            |Email                                      |
|mobilecode	       |string	|	            |Mobile code (country code)                 |
|mobile	           |string	|               |Mobile number                              |
|address	       |string	|	            |Residential address                        |
|town	           |string	|               |Town or locality                           |
|city	           |string	|	            |City                                       |
|state	           |string	|	            |State or region                            |
|zipcode	       |string	|	            |Postal code                                |
|countryid	       |string	|	            |Country ID                                 |
|countryisothree   |string	|	            |ISO 3166-1 alpha-3 country code            |
|emergencycontact  |string  |	            |Emergency contact number                   |
|doctype	       |integer |	            |Document type (0 for unspecified)          |
|docid	           |string	|	            |Document ID                                |
|frontdoc	       |string	|	            |Front image of the document                |
|backdoc	       |string	|	            |Back image of the document                 |
|docexpiredate     |string	|	            |Expiry date of the document                |
|docneveexpire     |integer |	            |Indicates if the document has never expired (0 for no)|
|handholdidphoto   |string	|	            |Handheld ID photo                          |
|biomatric	       |string	|	            |Biometric data                             |
|photo	           |string	|	            |Personal photo                             |
|signimage	       |string	|               |Signature image                            |

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

**Response Example**

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
  "cardId": "2345678",
  "amount": "100"
}

```

**Response Example**

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
| cardId    | string  |       Y        | Must be between 1 and 36 bytes in UTF-8 encoding|

```json
{    
  "cardId": "2qw234e"
}

```
**Response Example**

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
**Response Example**

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

**Response Example**

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

**Response Example**

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
``` path parameter
{
"cardId":"76ddcaab-55c4-46e0-8d80-e7d097bfc1b3"
}
```

**Response Example**

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
| cardId    | string   |       Y        |Must be between 1 and 36 bytes in UTF-8 encoding|

``` path parameter
{
"cardId":"76ddcaab-55c4-46e0-8d80-e7d097bfc1b3"
}
```

**Response Example**

```json
{  
 "cardNumber": "12134523",
  "pin": "234"
}
```
## CardBalance

**HTTP request**

**Get /CardBalance/{cardId}**

**Summary**

Card Balance

**Request**

**Headers**

- **Content-Type:** application/json


| Parameter | Type     |Required or not |Description                       |
| :-------- | :------- |:-------------- |:-------------------------------- |
| cardId    | string   |       Y         |Must be between 1 and 36 bytes in UTF-8 encoding|

``` path parameter
{
"cardId":"76ddcaab-55c4-46e0-8d80-e7d097bfc1b3"
}
```

**Response Example**
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
# Callback notification
## Set callback notification url
This interface is used to set callback notification URL.

Please set the callback notification address in the merchant basic information in the merchant backend system. All callback notifications share this URL, and use notifyType to distinguish different notification contents

## Callback notification type

| notifyType | business | meaning |
|:------------|:------|:------|
| 1 | Global Express | Payment success, payment failure, refund notification |
| 2 | Global Express | Order adjustment notification |
| 3 | Bank card | Callback notification after user registration review is completed |
| 4 | Bank card | Card recharge result callback notification |
| 5 | Bank card | Card activation result callback notification |
| 6 | Bank card | Card freeze, thaw processing status callback notification |
| 7 | Bank card | 3DS verification |

## Global Express Remittance Payment result callback notification
This notification notifyType = 1

**Callback parameters:**

| Parameter | Type | Must be transmitted | Meaning |
|:------|:------|:------|:------|
| orderNo | Long | Y | PayoCard order number |
| status | String | Y | Order status. B3: Successful payment; B4: Failed payment; B6: Refund; |
| paymentCurrency | String | Y | Payment currency |
| paymentAmount | BigDecimal | Y | Payment amount |
| paymentFee | BigDecimal | Y | Payment fee |
| receivedAccountNum | String | Y | Receiving account number |
| receivedAccountName | String | Y | Receiving account name |
| receivedCurrency | String | Y | Receiving currency |
| receivedAmount | BigDecimal | N | Receiving amount. Return when status = B3, B4, B6 |
| resultMsg | String | N | message |

**Callback example:**

```JSON
{
    "data":{
        "orderNo": 12343434232324,
        "status": "B3",
        "paymentCurrency": "EUR",
        "paymentAmount": 10,
        "payAccountNum": "1234",
        "paymentFee": 1.5,
        "receivedAccountNum": "es324353535",
        "receivedAccountName": "tom z",
        "receivedCurrency": "SGD",
        "receivedAmount": 30.5,
        "resultMsg": "success"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
    "notifyType": 1
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|:------|:------|:------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be initiated repeatedly |
| message | String | N | Information |

**Response example:**

```json
{
    "code": 0,
    "message": "success"
}
```
## Global Express Remittance Adjustment callback notification

This notification notifyType = 2

**Callback parameters:**

| Parameter | Type | Required or not | Meaning |
|:------|:------|:------|:------|
| orderNo | Long | Y | PayouCard order number |
| status | String | Y | Order status. B11: Transfer order_pending submission; B13: Transfer order_approved; B15: Transfer order_rejected |
| transferOrderInfo | List | N | Transfer order information. See dictionary_biz.pdf (2.2. Transfer order info type) |
| transferOrderFile | List | N | Transfer order file. See dictionary_biz.pdf (2.3. Transfer order file type) |
| transferOrderDesc | String | N | Transfer order information |

**Callback example:**

```json
{
  "data":
  {
    "orderNo": 12343434232324,
    "status": "B11",
    "transferOrderInfo":
    [
      "1",
      "3",
      "5"
    ],
    "transferOrderFile":
    [
      "21",
      "23",
      "24"
    ],
    "transferOrderDesc": ""
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
  "notifyType": 2
}
```
**Response parameters:**

| Parameter | Type | Required | Meaning |
|:------|:------|:------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be repeated |
| message | String | N | information |

**Response example:**
```json
{
    "code": 0,
    "message": "success"
}
```
## User KYC callback notification
This notification notifyType = 3

**Callback parameters:**

| Parameter | Type | Required or not | Meaning |
|:------|:------|:------|:------|
| uniqueId | String | Y | Unique ID of partner user |
| cardTypeId | Integer | Y | Card type |
| status | Integer | Y | Status |
| statusDesc | String | Y | Status description |

**Callback example:**
```json
{
    "data":{
        "uniqueId": "T6789O067890",
        "cardTypeId": 1,
        "status": 1,
        "statusDesc":"success"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
    "notifyType": 3
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|:------|:------|:------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be initiated repeatedly |
| message | String | N | Information |

**Response example:**
```json
{
    "code": 0,
    "message": "success"
}
```
## Bank card activation result callback notification
This notification notifyType = 5

**Callback parameters:**

| Parameter | Type | Is it required | Meaning |
|:------|:------|:------|:------|
| cardNo | String | Y | Card number |
| status | Integer | Y | Status. 5: Activation successful; 12: Activation review failed |
| msg | String | N | Error message |

**Callback example:**
```JSON
{
    "data":{
        "cardNo": "12456782323",
        "status": 5,
        "msg": null
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
    "notifyType": 5
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|:------|:------|:------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be initiated repeatedly |
| message | String | N | information |

**Response example:**
```json
{
    "code": 0,
    "message": "success"
}
```
## Bank card freeze thaw processing status callback notification
This notification notifyType = 6

**Callback parameters:**

| Parameter | Type | Is it required | Meaning |
|:------|:------|:-----|:------|
| uniqueId | String | Y | Unique ID of the partner user |
| cardNo | String | Y | Card number |
| status | Integer | Y | Status. 1: Success; 2: Failure |
| requestType | Integer | Y | Type. 1: Freeze; 2: Unfreeze |

**Callback example:**

```json
{
    "data":{
        "uniqueId": 502323,
        "cardNo": "12456782323",
        "status": 1,
        "requestType": 1
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
    "notifyType": 5
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|:------|:------|:------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be repeated |
| message | String | N | information |

**Response example:**
```json
{
    "code": 0,
    "message": "success"
}
```
## Bank card 3DS verification
This notification notifyType = 7

**Callback parameters:**

| Parameter | Type | Must be passed | Meaning |
|:------|:------|:------|:------|
| cardNo | String | Y | Card number |
| otp | String | Y | Verification code |
| merchantName | String | Y | Merchant name |
| transactionAmount | String | Y | Transaction amount |
| transactionCurrency | String | Y | Transaction currency |

**Callback example:**

```json
{
    "data":{
        "cardNo": "4611990424818446",
        "otp": "890789",
        "merchantName": "test",
        "transactionAmount": "100",
        "transactionCurrency": "EUR"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
    "notifyType": 5
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|:------|:------|:------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be initiated repeatedly |
| message | String | N | Information |

**Response example:**
```json
{
    "code": 0,
    "message": "success"
}
```