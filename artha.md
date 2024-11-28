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
    - [Callback-notificationtype](#Merchant-Information)
    - [GlobalExpressRemittancePaymentresultcallbacknotification](#GlobalExpressRemittancePaymentresultcallbacknotification)	
    - [GlobalExpressRemittanceAdjustmentcallbacknotification](#GlobalExpressRemittanceAdjustmentcallbacknotification)
    - [UserKYCcallbacknotification](#UserKYCcallbacknotification)
    - [Bankcardcallbacknotificationofcardrechargeresult](#Bankcardcallbacknotificationofcardrechargeresult)
    - [Bankcardactivationresultcallbacknotification](#Bankcardactivationresultcallbacknotification)
    - [Bankcardfreezethawprocessingstatuscallbacknotification](#Bankcardfreezethawprocessingstatuscallbacknotification)
    - [Bankcard3DSverification](#Bankcard3DSverification)


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
| CustomerToken   | string | true     | Merchant identification, provided by Artha Cards                                  |
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
| Status	 | String | Y        | Whether it was successful. Success: Failed.|
| opt_code   | String | Y        | It's Kind Of OTP |
| notifyType | String | Y        | notification type |
| amount	 | String | Y        | Amount   |
|CreationTime| integer| Y        | CreationTime |

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
| Passport             |  DocType, DocId, Frontdoc, Backdoc, DocExpireDate, DocNeverExpire. |
| FullNameOnly         |  FirstName, LastName.                                           |
| FullName             |  FirstName, LastName, Gender, DOB.                              |
| Comms                | Email, EmailMobileCode, Mobile.                                 |
| EmergencyContact     |  EmergencyContactNumber.                                        |
| Address              |  Address.                                                       |
| FullAddress          | Address, Town, City, State, ZipCode, CountryId, CountryIsoThree.|
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
| programId                | string  |       Y        | must be between 1 and 36 bytes in UTF-8 encoding               |
| kyc                      | object  |                |If the Kycrequirements are null, then KYC information is not needed. However, if the Kycrequirements have a value and kycRequiredWhileApplyCard is true, the KYC information must be provided. In this case, ensure that the required fields are passed based on the program's KYC requirements. Otherwise, KYC information is not required.   |
     |       firstname	       |string	 |      N         |First name of the individual                                    |
     |       lastname	       |string	 |      N         |Last name of the individual                                     |
     |       gender	           |integer  |      N         |Gender (1: male, 2: female)                                     |
     |       dob	               |string	 |      N         |Birthday (yyyy-MM-dd)                                           |
     |       nationalityid      |string	 |      N         |Nationality ID                                                  |
     |       email	           |string	 |      N         |Email                                                           |
     |       mobilecode	       |string	 |      N         |Mobile code (country code)                                      |
     |       mobile	           |string	 |      N         |Mobile number                                                   |
     |       address	           |string	 |      N         |Residential address                                             |
     |       town	           |string	 |      N         |Town code. Please call the interface /Towns                     |
     |       city	           |string	 |      N         |Country code. 2-digit code.Please call the interface /Countries |
     |       state	           |string	 |      N         |State or region                                                 |
     |       zipcode	           |string	 |      N         |Postal code                                                     |
     |       countryid	       |string	 |      N         |Country ID                                                      |
     |       countryisothree    |string	 |      N         |ISO 3166-1 alpha-3 country code                                 |
     |       emergencycontact   |string   |      N         |Emergency contact number                                        |
     |       doctype	           |integer  |      N         |Document type (0 for unspecified)                               |
     |       docid	           |string	 |      N         |Document ID                                                     |
     |       frontdoc	       |string	 |      N         |Front image of the document                                     |
     |       backdoc	           |string	 |      N         |Back image of the document                                      |
     |       docexpiredate      |string	 |      N         |Expiry date of the document                                     |
     |       docneveexpire      |integer  |      N         |Indicates if the document has never expired (0 for no)          |
     |       handholdidphoto    |string	 |      N         |Handheld ID photo                                               |
     |       biomatric	       |string   |      N         |Biometric data                                                  |
     |       photo	           |string	 |      N         |Personal photo                                                  |
     |       signimage	       |string	 |      N         |Signature image                                                 |
        



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

Binding KYC

**Request**

**Headers**

- **Content-Type:** application/json

**Request Body**

| Parameter                | Type     | Required or not|Description                                                    |
| :------------------------|:-------- |:---------------|:--------------------------------------------------------------|
| taskId                   | string   |       Y        | must be between 1 and 36 bytes in UTF-8 encoding              |
| cardNumber               | string   |       Y        |**Required.** Card number must be at least 1 byte and no more than 19 bytes in UTF-8 encoding|
|envelopeNo                | string   |       N        |Envelope number can be null. If provided, it must be between 1 and 15 bytes in UTF-8 encoding|
|handholdidphoto           |string    |       Y        | Photo of holding passport and bank card (URL format). Cannot exceed 2M, supports .png, .jpeg, .jpg formats. When the user performs kyc, the card type represented by the parameter cardTypeId is only required when needPhotoForActiveCard=true. See the parameter needPhotoForActiveCard in the interface /MerchantInformation/Merchant.|
| kyc                      | object   |                |If the Kycrequirements are null, then KYC information is not needed. However, if the Kycrequirements have a value and kycRequiredWhileApplyCard is false, the KYC information must be provided. In this case, ensure that the required fields are passed based on the program's KYC requirements. Otherwise, KYC information is not required.                                |
|       firstname	       |string	  |      N         |First name of the individual                                    |
|       lastname	       |string	  |      N         |Last name of the individual                                     |
|       gender	           |integer   |      N         |Gender (1: male, 2: female)                                     |
|       dob	               |string	  |      N         |Birthday (yyyy-MM-dd)                                           |
|       nationalityid      |string	  |      N         |Nationality ID                                                  |
|       email	           |string	  |      N         |Email                                                           |
|       mobilecode	       |string	  |      N         |Mobile code (country code)                                      |
|       mobile	           |string	  |      N         |Mobile number                                                   |
|       address	           |string	  |      N         |Residential address                                             |
|       town	           |string	  |      N         |Town code. Please call the interface /Towns                     |
|       city	           |string	  |      N         |Country code. 2-digit code.Please call the interface /Countries |
|       state	           |string	  |      N         |State or region                                                 |
|       zipcode	           |string	  |      N         |Postal code                                                     |
|       countryid	       |string	  |      N         |Country ID                                                      |
|       countryisothree    |string	  |      N         |ISO 3166-1 alpha-3 country code                                 |
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
  "receiveAmount": "30"
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
| signimage | string  |       N        | User signature photo (URL format). It cannot be larger than 2M and supports the formats .png, .jpeg, and .jpg. It is only required when the card type represented by the bank card is needPhotoForOperateCard=true. See the parameter needPhotoForOperateCard in the interface /MerchantInformation/Merchant.|


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
| signimage | string  |       N        | User signature photo (URL format). It cannot be larger than 2M and supports the formats .png, .jpeg, and .jpg. It is only required when the card type represented by the bank card is needPhotoForOperateCard=true. See the parameter needPhotoForOperateCard in the interface /MerchantInformation/Merchant.|

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
| signimage | string  |       N        | User signature photo (URL format). It cannot be larger than 2M and supports the formats .png, .jpeg, and .jpg. It is only required when the card type represented by the bank card is needPhotoForOperateCard=true. See the parameter needPhotoForOperateCard in the interface /MerchantInformation/Merchant.|

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
|     available_balance   | string   |         available_balance                     |
|     cardcurrency       | string   |         card_currency                          |
|     cardnumber         | string   |         card_number                            |
|     cardtype           | string   |         card_type                              |
|     currentbalance     | string   |         current_balance                        |

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