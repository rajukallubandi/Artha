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
	- [KYC-Requirements](#KYC-Requirements)
    - [ApplyCard](#ApplyCard)
    - [Binding](#Binding)
	 - [EstimateCardTopUpFee](#EstimateCardTopUpFee)
    - [CardTopUp](#CardTopUp)
    - [CardSetPin](#CardSetPin)
    - [CardFreeze](#CardFreeze)
    - [CardUnFreeze](#CardUnFreeze)
    - [CancelCard](#CancelCard)
    - [CardDetails](#CardDetails)
    - [PinDetails](#PinDetails)
    - [CardBalance](#CardBalance)
    - [CardUnFreeze](#CardUnFreeze)
    - [Countries](#Countries)
    - [Towns](#Towns)
	
	
- [Callback notification](#Callback-notification)	
    - [kyc status result callback notification](#kycstatus-result-callback-notification)
    - [createcard result callback notification](#createcard-result-callback-notification)
    - [recharge callback notification](#recharge-callback-notification)
    - [operation callback notification](#operation-callback-notification)
    - [consume result callback notification](#consume-result-callback-notification)
    - [fee callback notification](#fee-callback-notification)
    - [refund callback notification](#refund-callback-notification)
    - [Bankcard 3DSverification](#Bankcard-3DSverification)
    


# Introduction
Welcome to the ArthaCard developer documentation. An overview of the merchant docking application development interface.



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

Test environment: https://tstcards.artha.work/


## API Base URL

Test environment:[ https://tstapi.artha.work/api/v1 _blank] 


**customerToken:** This token must be included in the request header when accessing the API, using the format **customerToken={customerToken}**.

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
| customerToken   | string | true    | Merchant identification, provided by Artha Cards                              |
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
| Parameters | Type   | required or not | Description    |
|:-----------|:-------|:---------|:-----------|
| taskId     | String | Y        | taskId |
| Remarks    | String | Y        | Remarks |
| Status	 | String | Y        | Whether it was a success or a failed	|
| opt_code   | String | Y        | It's Kind Of OTP |
| notifyType | String | Y        | notification type |
| amount	 | String | Y        | Amount   |

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
  "balances":
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
  "supportedOperationTypes": "Purchase, Transfer",
  "cardImage": "https://example.com/images/card.png",
  "supportedPlatforms": "iOS, Android, Web",
  "topUpTokens": [
    {
      "token": "USDT",
      "network": "TRC-20",
      "address": "000xxx"
    }
  ],
  "kycRequiredWhileApplyCard": true,
  "kycRequirements": "string",
  "kycType": "string",
  "needPhotoForActiveCard": true,
  "needPhotoForOperateCard": true
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
| programid | string  |        Y       |programid                          |

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
  "supportedOperationTypes": "Purchase, Transfer",
  "cardImage": "https://example.com/images/card.png",
  "supportedPlatforms": "iOS, Android, Web",
  "topUpTokens": [
    {
      "token": "USDT",
      "network": "TRC-20",
      "address": "000xxx"
    }
  ],
  "kycRequiredWhileApplyCard": true,
  "kycRequirements": "string",
  "kycType": "string",
  "needPhotoForActiveCard": true,
  "needPhotoForOperateCard": true
}
```

## KYC Requirements
- **Description:** Based on the program's KYC requirements, ensure that the required fields in the KYC section are provided while applying for a card or binding a card

|  KYC Requirements    | Required Fields                                                |
|:---------------------|:-------------------------------------------------------------- |
| PassportOnly         |  DocType, DocId, Frontdoc, Backdoc.                            |
| Passport             |  DocType, DocId, Frontdoc, Backdoc, DocExpireDate, DocNeverExpire|
| FullNameOnly         |  FirstName, LastName.                                           |
| FullName             |  FirstName, LastName, Gender, DOB.                              |
| Comms                |  Email, MobileCode, Mobile.                                     |
| EmergencyContact     |  emergencycontact.                                              |
| Address              |  Address.                                                       |
| FullAddress          |  Address, Town, City, State, ZipCode, CountryId, CountryIsoThree|
| HandedPassport       |  HandHoldIdPhoto.                                               |
| Face                 |  Photo.                                                         |
| Sign                 |  SignImage.                                                     |
| Biomatric            |  Biomatric.                                                     |


## ApplyCard

**HTTP request**

**POST /ApplyCard**

**Summary**

Apply for a card using the specified program ID.

**Request**

**Headers**

- **Content-Type:** application/json

**Request Body**

| Parameter                | Type    |Required or not | Description                                                    |
| :--------                | :-------|:-------------- | :--------------------------------                              |
| programId                | string  |       Y        | programId                                                      |
| kyc                      | object  |                |If the Kycrequirements are null, then KYC information is not needed. However, if the Kycrequirements have a value and kycRequiredWhileApplyCard is true, the KYC information must be provided. In this case, ensure that the required fields are passed based on the program's KYC requirements. Otherwise, KYC information is not required.   |

**kyc**
| Parameter                | Type     | Required or not|Description                                                     |
| :------------------------|:-------- |:---------------|:-------------------------------------------------------------- |
|       firstname	       |string	  |      N         |First name of the individual                                    |
|       lastname	       |string	  |      N         |Last name of the individual                                     |
|       gender	           |integer   |      N         |Gender (1: male, 2: female)                                     |
|       dob	               |string	  |      N         |Birthday (yyyy-MM-dd)                                           |
|       nationalityid      |string	  |      N         |Nationalityid.  Please call the interface /Countries             |
|       email	           |string	  |      N         |Email                                                           |
|       mobilecode	       |string	  |      N         |Mobile code (country code)                                      |
|       mobile	           |string	  |      N         |Mobile number                                                   |
|       address	           |string	  |      N         |Residential address                                             |
|       town	           |string	  |      N         |Town code. Please call the interface /Towns                     |
|       city	           |string	  |      N         |           city                                                 |
|       state	           |string	  |      N         |State or region                                                 |
|       zipcode	           |string	  |      N         |Postal code                                                     |
|       countryid	       |string	  |      N         |Country Id  Please call the interface /Countries                |
|       countryisothree    |string	  |      N         |countryisothree  Please call the interface /Countries           |
|       emergencycontact   |string    |      N         |Emergency contact number                                        |
|       doctype	           |integer   |      N         |Document type (0 for unspecified)                               |
|       docid	           |string	  |      N         |Document ID                                                     |
|       frontdoc	       |string	  |      N         |Front image of the document                                     |
|       backdoc	           |string	  |      N         |Back image of the document                                      |
|       docexpiredate      |string	  |      N         |Expiry date of the document                                     |
|       docneveexpire      |integer   |      N         |Indicates if the document has never expired (0 for no)          |
|       handholdidphoto    |string	  |      N         |Handheld ID photo                                               |
|       biomatric	       |string    |      N         |Biometric data                                                  |
|       photo	           |string	  |      N         |Personal photo                                                  |
|       signimage	       |string	  |      N         |Signature image                                                 |
        



```json
{
  "programId": "667c63ed-9187-4b1a-be64-8190c8d7ab2b",
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

| Parameter | Type    | Description           |
| :-------- | :-------|:----------------------| 
| cardId    | string  | cardId                |
| status    | string  | status(Success,Failed)|
| remarks   | string  | Status description    |

```json
{    
  "cardId": "c1b4d39b-7a60-48b7-9bf2-d3fe20d3fdef",
  "status": "Success",
  "remarks": "Description"
}

```


## Binding

**HTTP request**

**POST /Binding**

**Summary**

If kycRequiredWhileApplyCard is true, you must wait for the kycStatus callback. Once you receive a success status in the callback, you can proceed to bind the card.
If kycRequiredWhileApplyCard is false, you can directly call the Bind API after applying for the card.

**Request**

**Headers**

- **Content-Type:** application/json

**Request Body**

| Parameter                | Type     | Required or not|Description                                                    |
| :------------------------|:-------- |:---------------|:--------------------------------------------------------------|
| cardId                   | string   |       Y        | cardId                                                        |
| cardNumber               | string   |       Y        |**Required.** Card number                                      |
|envelopeNo                | string   |       N        |Check if the EnvelopeNoRequired field is required in the program interface.|
|handholdidphoto           |string    |       Y        | Photo of holding passport and bank card  Cannot exceed 2MB, supports .png, .jpeg, .jpg formats. When the user performs kyc, the card type represented by the parameter cardTypeId is only required when needPhotoForActiveCard=true. See the parameter needPhotoForActiveCard in the interface /MerchantInformation/Merchant.|
| kyc                      | object   |                |If the Kycrequirements are null, then KYC information is not needed. However, if the Kycrequirements have a value and kycRequiredWhileApplyCard is false, the KYC information must be provided. In this case, ensure that the required fields are passed based on the program's KYC requirements. Otherwise, KYC information is not required.                                |

**kyc**
| Parameter                | Type     | Required or not|Description                                                     |
| :------------------------|:-------- |:---------------|:-------------------------------------------------------------- |
|       firstname	       |string	  |      N         |First name of the individual                                    |
|       lastname	       |string	  |      N         |Last name of the individual                                     |
|       gender	           |integer   |      N         |Gender (1: male, 2: female)                                     |
|       dob	               |string	  |      N         |Birthday (yyyy-MM-dd)                                           |
|       nationalityid      |string	  |      N         |Nationalityid.  Please call the interface /Countries             |
|       email	           |string	  |      N         |Email                                                           |
|       mobilecode	       |string	  |      N         |Mobile code (country code)                                      |
|       mobile	           |string	  |      N         |Mobile number                                                   |
|       address	           |string	  |      N         |Residential address                                             |
|       town	           |string	  |      N         |Town Id  Please call the interface /Towns                       |
|       city	           |string	  |      N         |City                                                            |
|       state	           |string	  |      N         |State or region                                                 |
|       zipcode	           |string	  |      N         |Postal code                                                     |
|       countryid	       |string	  |      N         |Country Id  Please call the interface /Countries                |
|       countryisothree    |string	  |      N         |countryisothree  Please call the interface /Countries           |
|       emergencycontact   |string    |      N         |Emergency contact number                                        |
|       doctype	           |integer   |      N         |Document type (0 for unspecified)                               |
|       docid	           |string	  |      N         |Document ID                                                     |
|       frontdoc	       |string	  |      N         |Front image of the document                                     |
|       backdoc	           |string	  |      N         |Back image of the document                                      |
|       docexpiredate      |string	  |      N         |Expiry date of the document                                     |
|       docneveexpire      |integer   |      N         |Indicates if the document has never expired (0 for no)          |
|       handholdidphoto    |string	  |      N         |Handheld ID photo                                               |
|       biomatric	       |string    |      N         |Biometric data                                                  |
|       photo	           |string	  |      N         |Personal photo                                                  |
|       signimage	       |string	  |      N         |Signature image                                                 |

```json
{
  "taskId": "5eacfc9c-3c51-4e6c-b286-b4c632e44770",
  "cardNumber": "3566002020360505",
  "envelopeNo": "264",
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

| Parameter | Type    | Description           |
| :-------- | :-------|:----------------------| 
| cardId    | string  | cardId                |
| cardno    | string  | cardno                |
| status    | string  | status(Success,Failed)|
| remarks   | string  | Status description    |

```json
{    
   "cardId": "c1b4d39b-7a60-48b7-9bf2-d3fe20d3fdef",
   "cardno": "6200000000000005",
   "status": "Success",
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
|  cardId   | string  |       y         | CardId                            |
|  amount   | string  |       y         | TopUp Amount	                    |

```json
{    
  "cardId": "19d1f3e2-2ecf-45d3-a796-4f5a2d557f5d",
  "amount": "100"
}
```

**Response Example**

| Parameter    | Type    | Description           |
| :------------|:--------|:----------------------| 
| cardId       | string  | cardId                |
| amount       | decimal | amount                |
| fee          | decimal | fee                   |
| receiveAmount|  decimal| receiveAmount         |

```json
{  
  "cardId": "d54f5e69-107d-49d2-bafe-7f837eb85da8",
  "amount": "100",
  "fee": "10",
  "receiveAmount": "90"
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
| cardId    | string  |       Y        |  CardId                       |
| amount    | string  |       Y        | TopUp Amount                      |


```json
{    
  "cardId": "c4bf67ad-c353-45ce-9314-19922fdf0c55",
  "amount": "100"
}

```

**Response Example**

| Parameter | Type    | Description           |
| :-------- | :-------|:----------------------| 
| taskId    | string  | taskId                |
| status    | string  | status(Success,Failed)|
| remarks   | string  | Status description    |

```json
{  
  "taskId": "bb75ad44-ebee-42c5-8550-204901404e93",
  "status": "Success",
  "remarks": "CardInProgress"
}
```
## CardSetPin
**HTTP request**

**POST /CardSetPin**

**Summary**

card Set Pin

**Request**

**Headers**

- **Content-Type:** application/json

**RequestBody**

| Parameter | Type    |Required or not | Description                       |
| :-------- | :-------|:-------------- | :-------------------------------- |
| cardId    | string  |       Y        | CardId                        |
| signimage | string  |       N        | User signature photo It cannot be larger than 2M and supports the formats .png, .jpeg, and .jpg. It is only required when the card type represented by the bank card is needPhotoForOperateCard=true. See the parameter needPhotoForOperateCard in the interface /MerchantInformation/Merchant.|


```json
{    
  "cardId": "37e85fe5-8834-4b7c-9797-dd307e9418ef",
  "signimage": "signimage"
}

```

**Response Example**

| Parameter | Type    | Description           |
| :-------- | :-------|:----------------------| 
| taskId    | string  | taskId                |
| status    | string  | status(Success,Failed)|
| remarks   | string  | Status description    |

```json
{  
  "taskId": "0871d828-a18e-4841-8ace-b5bf264f8c51",
  "status": "Success",
  "remarks": "CardSetPin SuccesFully!"
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
| cardId    | string  |       Y        |  CardId                       |
| signimage | string  |       N        | User signature photo It cannot be larger than 2M and supports the formats .png, .jpeg, and .jpg. It is only required when the card type represented by the bank card is needPhotoForOperateCard=true. See the parameter needPhotoForOperateCard in the interface /MerchantInformation/Merchant.|

```json
{    
  "cardId": "81e38cea-9ea7-4f84-819f-16a40ba1f466"
}

```
**Response Example**

| Parameter | Type    | Description           |
| :-------- | :-------|:----------------------| 
| taskId    | string  | taskId                |
| status    | string  | status(Success,Failed)|
| remarks   | string  | Status description    |

```json
{  
 "taskId": "c9f691d9-178f-4f11-9040-9a81a513525a",
 "status": "Success",
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
| cardId    | string  |       Y        |  CardId                       |
| signimage | string  |       N        | User signature photo It cannot be larger than 2M and supports the formats .png, .jpeg, and .jpg. It is only required when the card type represented by the bank card is needPhotoForOperateCard=true. See the parameter needPhotoForOperateCard in the interface /MerchantInformation/Merchant.|

```json
{    
  "cardId": "74099243-5467-4e29-9f7f-6984f1db9e01"
}

```
**Response Example**

| Parameter | Type    | Description           |
| :-------- | :-------|:----------------------| 
| taskId    | string  | taskId                |
| status    | string  | status(Success,Failed)|
| remarks   | string  | Status description    |

```json
{  
 "taskId": "b3190d60-a06f-47ee-b9a9-bc63246b912d",
 "status": "Success",
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
| cardId    | string  |       Y        | cardId                            |

```json
{    
  "cardId": "d55709b7-a56f-4bcc-b08a-00131bfc8bf1"
}
```

**Response Example**

| Parameter | Type    | Description           |
| :-------- | :-------|:----------------------| 
| taskId    | string  | taskId                |
| status    | string  | status(Success,Failed)|
| remarks   | string  | Status description    |

```json
{  
 "taskId": "9404f5bc-b767-48c3-8651-befe0b2c3a3f",
 "status": "Success",
 "remarks": "CardInProgress"
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
| cardId    | string   |       Y       | CardId                        |
``` path parameter
{
"cardId":"76ddcaab-55c4-46e0-8d80-e7d097bfc1b3"
}
```

**Response Example**

| Parameter        | Type    | Description           |
| :----------------|:--------|:----------------------| 
| cardNumber       | string  | cardNumber            |
| cvv              | string  | cvv                   |
| expirationDate   | DateTime| expirationDate        |

```json
{  
 "cardNumber": "6200000000000047",
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
| cardId    | string   |       Y        | cardId                       |

``` path parameter
{
"cardId":"76ddcaab-55c4-46e0-8d80-e7d097bfc1b3"
}
```

**Response Example**

| Parameter        | Type    | Description           |
| :----------------|:--------|:----------------------| 
| cardNumber       | string  | cardNumber            |
| Pin              | string  | Pin                   |

```json
{  
 "cardNumber": "2222400010000008",
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
| cardId    | string   |       Y        | CardId                           |

``` path parameter
{
"cardId":"76ddcaab-55c4-46e0-8d80-e7d097bfc1b3"
}
```

**Response Example**

| Parameter               | Type     |Description                                    |
| :-----------------------| :------- |:----------------------------------------------|
|     availablebalance   | string    |         availablebalance                     |
|     cardcurrency        | string   |         cardcurrency                         |
|     cardnumber          | string   |         cardnumber                           |
|     cardtype            | string   |         cardtype                             |
|     currentbalance      | string   |         currentbalance                       |

```json 
{  
   "availablebalance": "100",
    "cardcurrency": "USD",
    "cardnumber": "12134523",
    "cardtype": "OT",
    "currentbalance": "50"
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
[
    {
        "id": "8ee37608-a700-434b-be5d-ba01fd74182c",
        "name": "India",
        "nationality": "Indian",
        "isoTwo": "IN",
        "isoThree": "IND",
        "isoNumber": "356"
    },
    {
        "id": "7aa23818-b812-47f4-af7d-99a20fa4b3de",
        "name": "United States",
        "nationality": "American",
        "isoTwo": "US",
        "isoThree": "USA",
        "isoNumber": "840"
    },
    {
        "id": "2f24d5b0-90a7-4f8d-8ed9-8337cf0e9c82",
        "name": "Canada",
        "nationality": "Canadian",
        "isoTwo": "CA",
        "isoThree": "CAN",
        "isoNumber": "124"
    }
]

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
[
    {
        "id": "1a2b3c4d-5678-9101-1121-314151617181",
        "name": "Mumbai",
        "code": "MU",
        "countryOrRegion": "India"
    },
    {
        "id": "2b3c4d5e-6789-1011-1213-141516171819",
        "name": "Delhi",
        "code": "DL",
        "countryOrRegion": "India"
    },
    {
        "id": "3c4d5e6f-7890-1112-1314-151617181920",
        "name": "Bangalore",
        "code": "BL",
        "countryOrRegion": "India"
    }
]

 ```
# Callback notification

This interface is used to set callback notification URL.

Please set the callback notification address in the merchant basic information in the merchant backend system. All callback notifications share this URL, and use notifyType to distinguish different notification contents

## Callback notification type

| notifyType  | business       | meaning |
|:------------|:---------------|:------  |
| kycstatus   | Bank card      | Card Kyc activation result callback notification 
This applies only to physical cards and only if kycRequiredWhileApplyCard is true. Once the KYC is approved, you can proceed to bind the card.|
| createcard  | Bank card      | Card activation result callback notification|
| recharge    | Bank card      | Card recharge result callback notification |
| operation   | Bank card      | Card freeze,unfreeze and cancel callback notification |
| consume     | Bank card      | Card Consumption callback notification |
| fee         | Bank card      | Card ConsumptionFee  callback notification |
| refund      | Bank card      | Card Refund callback notification |
| opt_code    | Bank card      | 3DS verification |


## kycstatus result callback notification
This notification notifyType = kycstatus

**Callback parameters:**

| Parameter      | Type       | Description               |
|:------         |:------     |:--------------------      |
| CardId         | String     |        CardId             |
| TaskId         | String     |        TaskId             |
| NotifyType     | String     |        NotifyType         |
| Status         | String     |        Status             |
| Remarks        | String     |        Remarks            |

**Callback example:**

```JSON
{
  "CardId": "1ecdc168-88e7-3e32-0c99-3a1640e53c67",
  "TaskId": "1ecdc168-88e7-3e32-0c99-3a1640e53c67",
  "NotifyType": "kycstatus",
  "Status": "Success",
  "Remarks": "Status Description"
}
```

## createcard result callback notification
This notification notifyType = createcard

**Callback parameters:**

| Parameter      | Type       | Description               |
|:------         |:------     |:--------------------      |
| CardId         | String     |        CardId             |
| TaskId         | String     |        TaskId             |
| NotifyType     | String     |        NotifyType         |
| Status         | String     |        Status             |
| Remarks        | String     |        Remarks            |

**Callback example:**

```JSON
{
  "TaskId": "4754c628-4a01-d836-d805-3a158ab6adf6",
  "NotifyType": "createcard",
  "CardId": "4754c628-4a01-d836-d805-3a158ab6adf6",
  "Status": "SUCCESS",
  "Remarks": "Status Description"
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
## recharge callback notification

This notification notifyType = recharge

**Callback parameters:**

| Parameter      | Type       | Description               |
|:------         |:------     |:--------------------      |
| CardId         | String     |        CardId             |
| TaskId         | String     |        TaskId             |
| NotifyType     | String     |        NotifyType         |
| Status         | String     |        Status             |
| Remarks        | String     |        Remarks            |

**Callback example:**

```JSON
{
  "CardId": "968d17d5-7e75-931c-ddbb-3a14c9775903",
  "TaskId": "b31fdd7b-f545-005a-aa28-3a15ed46b5d3",
  "NotifyType": "operation",
  "Status": "Success",
  "Remarks": "Status Description"
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
## operation callback notification
This notification notifyType = operation

**Callback parameters:**

| Parameter      | Type       | Description               |
|:------         |:------     |:--------------------      |
| CardId         | String     |        CardId             |
| TaskId         | String     |        TaskId             |
| NotifyType     | String     |        NotifyType         |
| Status         | String     |        Status             |
| Remarks        | String     |        Remarks            |

**Callback example:**
```JSON
{
  "CardId": "968d17d5-7e75-931c-ddbb-3a14c9775903",
  "TaskId": "b31fdd7b-f545-005a-aa28-3a15ed46b5d3",
  "NotifyType": "operation",
  "Status": "Success",
  "Remarks": "status Description"
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
## consume result callback notification
This notification notifyType = consume

**Callback parameters:**

| Parameter      | Type       | Description               |
|:------         |:------     |:--------------------      |
| CardId         | String     |        CardId             |
| TaskId         | String     |        TaskId             |
| NotifyType     | String     |        NotifyType         |
| Status         | String     |        Status             |
| Remarks        | String     |        Remarks            |                                                                                                                                                     
| Amount         | Decimal    |        Amount             |
| Currency       | String     |        Currency           |

**Callback example:**
```JSON
{
  "CardId": "b2df3227-9716-e841-3827-3a151c8c271e",
  "TaskId": "cf4a59d3-6248-2565-7b01-3a15b78b7903",
  "NotifyType": "consume",
  "Status": "Success",
  "amount": "207.5700",
  "currency": "USD",
  "Remarks": "Status Description"
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
## fee callback notification
This notification notifyType = fee

**Callback parameters:**

| Parameter      | Type       | Description               |
|:------         |:------     |:--------------------      |
| CardId         | String     |        CardId             |
| TaskId         | String     |        TaskId             |
| NotifyType     | String     |        NotifyType         |
| Status         | String     |        Status             |
| Remarks        | String     |        Remarks            |                                                                                                                                                     
| Amount         | Decimal    |        Amount             |
| Currency       | String     |        Currency           |

**Callback example:**

```JSON
{
  "CardId": "0217c3a8-42c9-c449-9a11-3a14f8f1169a",
  "TaskId": "d95ba737-e3d8-fbe6-4e03-3a15ca96bb8e",
  "NotifyType": "fee",
  "Status": "Success",
  "amount": "12.2900",
  "currency": "USD",
  "Remarks": "Status Description"
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
## refund callback notification
This notification notifyType = refund

**Callback parameters:**

| Parameter      | Type       | Description               |
|:------         |:------     |:--------------------      |
| CardId         | String     |        CardId             |
| TaskId         | String     |        TaskId             |
| NotifyType     | String     |        NotifyType         |
| Status         | String     |        Status             |
| Remarks        | String     |        Remarks            |                                                                                                                                                     
| Amount         | Decimal    |        Amount             |
| Currency       | String     |        Currency           |

**Callback example:**

```JSON
{
  "CardId": "135ae96b-c46b-3e18-600f-3a1596f820a6",
  "TaskId": "b2b8caf1-d457-09fd-276b-3a1597b81f6c",
  "NotifyType": "refund",
  "Status": "Success",
  "amount": "1.35",
  "currency": "USD",
  "Remarks": "Status Description"
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

## Bank card 3DS verification
This notification notifyType = OPT_CODE

**Callback parameters:**

| Parameter      | Type       | Description               |
|:------         |:------     |:--------------------      |
| CardId         | String     |        CardId             |
| TaskId         | String     |        TaskId             |
| NotifyType     | String     |        NotifyType         |
| OPT_CODE       | String     |        OPT_CODE           |

**Callback example:**

```JSON
{
  "CardId": "75568818-c4f3-715d-3fa9-3a157ee23b30",
  "TaskId": "75568818-c4f3-715d-3fa9-3a157ee23b30",
  "NotifyType": "OPT_CODE",
  "OPT_CODE": " 971615"
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