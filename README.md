# Real Time Email Verification API 

## EmailVerifier.com - Email Verification API -  
You need an account on [EmailVerifier.com](https://emailverifier.com) and  an API key to use our Email Verification API
## Use our API to automate email verification anywhere.


## Table of Contents

- Single & Bulk Email Verification API.................................................................................................
- Request limits......................................................................................................................
- Verify individual email.......................................................................................................................
- Get available credit balance..................................................................................................................
- Verify a list of emails......................................................................................................................
- Get single list info........................................................................................................................
- Get all list info........................................................................................................................
- Get single list statistics..................................................................................................................
- Export all emails from the list grouped by primary/secondary results...............................................
- Export valid emails from the list........................................................................................................
- Export risky emails from the list........................................................................................................
- Export invalid emails from the list.....................................................................................................
- Export unknown emails from the list.................................................................................................
- General response codes......................................................................................................................


## Single & Bulk Email Verification API.................................................................................................

With our Real-Time Email Verification and Validation API you can integrate our
sophisticated algorithm directly into your ERP, marketing application, web application
forms and email marketing solutions. Get real time and accurate email validation results
with a simple request.

## Request limits.......................................................................................................................................

The single verification API is limited to **60 requests per minute**. If you need to
verify more frequently, use the bulk verification API endpoints.

The bulk (list) verification API is limited to **60 requests per minute**. You can send
up to **200,000 emails per request.**


## Verify individual email.........................................................................................................................

**/api/email**

**Request**

In order to issue a verification API call, you will need the following values:

- **apiKey (string)** - Your API key
- **email (string)** - Email address to verify

Request format:

**GET:** https://emailverifier.space/api/email?email={email}&apiKey={apiKey}

**Response (JSON)**

```
{
"email":"support@emailverifier.com",
"primary_result":"risky",
"secondary_result":"accept_all",
"process_result":{
"syntax_check":"Pass",
"mx_check":"Pass",
"smtp_check":"Pass",
"catch_all_check":"Pass",
"role_check":"Fail",
"disposable_check":"Pass"
}
}
```
- **email (string)** - Provided email address
- **primary_result (string)** - Primary verification result. Possible values are:
    - valid - Email is valid and safe for sending.
    - risky - Email is risky but probably safe for sending (check secondary result).
    - invalid - Email is invalid. Not safe for sending.
    - unknown - Email could not be verified for some reason (e.g. timeout).
- **secondary_result** - Secondary verification result. Possible values are:
    - valid - Email is valid and safe for sending.
    - accept_all - Email address is valid but accepting messages for all non existing
    email addresses for domain. (Risky).
    - role - Email is valid but a role. (Risky).
    - disposable - Email is valid but is an disposable address. (Risky).
    - invalid_address - Address is not an valid email format.
    - invalid_account - Email address does not exist on the server.


- invalid_domain - Provided domain is not an email domain.
- unknown - Email could not be verified for some reason (e.g. timeout).
- **process_result (array)** - Results of individual verification steps. Possible values:
Fail | Pass
- **syntax_check** - Email address syntax check result.
- **mx_check** - Server DNS check result.
- **smtp_check** - Server SMTP check result.
- **catch_all_check** - Email address catch all check result.
- **role_check** - Email address role check result.
- **disposable_check** - Email address disposable check result.


## Get available credit balance..................................................................................................................

**/api/usage/balance**

**Request**

In order to issue a credit balance API call, you will need the following values:

- **apiKey (string)** - Your API key

Request format:

**GET:** https://emailverifier.space/api/usage/balance?apiKey={apiKey}

**Response (JSON)**

```
{
"team":"My team",
"balance": 1000000 ,
"pending_balance": 0 ,
"payment_model":"PREPAY"
}
```
- **team (string)** - Team name
- **balance (int)** - Available credit balance
- **pending_balance (int)** - Pending credit balance
- **Payment model (string)** - Current payment model


## Verify a list of emails............................................................................................................................

**/api/lists/upload**

**Request**

In order to issue a list verification API call, you will need the following values:

- **apiKey (string)** - Your API key (as a **GET** parameter)
- **emails (array)** - JSON list of emails to be verified (as a **POST** parameter)

Request format:

**POST:** https://emailverifier.space/api/lists/upload?apiKey={apiKey}

You must send post request with **Content-Type** set to **application/json**.

An example of emails parameter:

```
{
"emails":
[
"raines@live.com",
"empathy@sbcglobal.net",
"loscar@sbcglobal.net",
"pajas@icloud.com",
"sburke@msn.com",
"barjam@verizon.net",
"msusa@att.net"
]
}
```
**Response (JSON)**

```
{
"list": {
"id": 123 ,
"status": "processing",
"count": 456789
}
}
```
- **id (int)** - List ID
- **status (string)** - Current status of the list
    extracting - List is being uploaded.
    uploaded - List is uploaded.
    processing - List is being verified.
    complete - List is verified.
- **count (int)** - Number of emails in the list


## Get single list info................................................................................................................................

**/api/lists/info/{listId}**

**Request**

In order to issue a single list info API call, you will need the following values:

- **apiKey (string)** - Your API key
- **listId (int)** - ID of the list

Request format:

**GET** : https://emailverifier.com/api/lists/info/{listId}?apiKey={apiKey}

**Response (JSON)**

```
{
"list": {
"id": 123,
"name": "list_1547991464",
"emails": 456789 ,
"status": "complete",
"uploaded": "2019-01-20 13:37:44"
}
}
```
- **id (int)** - List ID
- **name (string)** - List name
- **emails (int)** - Number of emails in the list
- **status (string)** - Current status of the list
    extracting - List is being uploaded.
    uploaded - List is uploaded.
    processing - List is being verified.
    complete - List is verified.


## Get all list info......................................................................................................................................

**/api/lists/info**

**Request**

In order to issue a all lists info API call, you will need the following values:

- **apiKey (string)** - Your API key
- **length (int)** - Number of lists in the result set.
    default - 0
    max - 100
- **offset (int)** - From which list should resulting set start.
    default - 0

Request format:

**GET** : https://emailverifier.com/api/lists/info?apiKey={apiKey}

**Response (JSON)**

```
{
"total": 3 ,
"length": 100 ,
"offset": 0 ,
"lists": [
{
"id": 123 ,
"name": "list_1547991464",
"emails": 456789 ,
"status": "complete",
"uploaded": "2019-01-20 13:37:44"
},
{
"id": 124 ,
"name": "list_1547994420",
"emails": 41 ,
"status": "complete",
"uploaded": "2019-01-20 14:27:00"
},
{
"id": 125 ,
"name": "list_1547998398",
"emails": 4 ,
"status": "complete",
"uploaded": "2019-01-20 15:33:18"
}
]
}
```

- **id (int)** - List ID
- **name (string)** - List name
- **emails (int)** - Number of emails in the list
- **status (string)** - Current status of the list
    extracting - List is being uploaded.
    uploaded - List is uploaded.
    processing - List is being verified.
    complete - List is verified.


## Get single list statistics.......................................................................................................................

**/api/lists/stats/{listId}**

**Request**
In order to issue a single list statistics API call, you will need the following values:

- **apiKey (string)** - Your API key
- **listId (int)** - List ID.
Request format:

**GET** : https://emailverifier.com/api/lists/stats/{listId}?apiKey={apiKey}

**Response (JSON)**
{
"id": 1 ,
"emails": 41 ,
"details": {
"valid": {
"count": 6 ,
"secondary": {
"valid": 6
}
},
"invalid": {
"count": 26 ,
"secondary": {
"invalid_address": 1 ,
"invalid_domain": 10 ,
"invalid_account": 15
}
},
"risky": {
"count": 9 ,
"secondary": {
"accept_all": 9 ,
"role": 0 ,
"disposable": 0
}
},
"unknown": {
"count": 0 ,
"secondary": {
"unknown": 0
}
}
}
}


- **id (int)** - List ID
- **emails (int)** - Number of emails in the list
- **details (array)** - List details

```
◦ valid/invalid/risky/unknown (array) - List details by primary result
▪ count (int) - Number of specific emails
▪ secondary (array) - Number of emails by secondary result
valid - Valid emails
invalid_address - Emails with invalid address
invalid_domain - Emails with invalid domain
invalid_account - Non existing emails on domain
accept_all - Accept all emails
role - Emails addresses with a role
disposable - Disposable emails
unknown - Emails that could not be verified
```

## Export all emails from the list grouped by primary/secondary results...............................................

**/api/lists/export/all/{listId}**

**Request**

In order to issue an export all emails API call, you will need the following values:

- **apiKey (string)** - Your API key
- **listId (int)** - List ID.
Request format:

**GET** : https://emailverifier.com/api/lists/export/all/{listId}?apiKey={apiKey}

**Response (JSON)**

```
{
"list": {
"id": 4,
"emails": {
"valid": {
"valid": [
"raines@live.com",
"pajas@icloud.com"
]
},
"risky": {
"accept_all": [
"empathy@sbcglobal.net",
"loscar@sbcglobal.net",
"barjam@verizon.net",
"msusa@att.net"
]
},
"invalid": {
"invalid_account": [
"sburke@msn.com"
]
}
}
}
}
```
- **Id (int)** - List ID
- **emails (array)** - Emails by primary/secondary results


## Export valid emails from the list........................................................................................................

**/api/lists/export/valid/{listId}**

**Request**

In order to issue an export valid emails API call, you will need the following values:

- **apiKey (string)** - Your API key
- **listId (int)** - List ID.

Request format:

**GET** : https://emailverifier.com/api/lists/export/valid/{listId}?apiKey={apiKey}

**Response (JSON)**

```
{
"list": {
"id": 4,
"emails": {
"valid": {
"valid": [
"raines@live.com",
"pajas@icloud.com"
]
}
}
}
}
```
- **Id (int)** - List ID
- **emails (array)** - Emails by primary/secondary results


## Export risky emails from the list........................................................................................................

**/api/lists/export/risky/{listId}**

**Request**

In order to issue an export risky emails API call, you will need the following values:

- **apiKey (string)** - Your API key
- **listId (int)** - List ID.

Request format:

**GET** : https://emailverifier.com/api/lists/export/risky/{listId}?apiKey={apiKey}

**Response (JSON)**

```
{
"list": {
"id": 4,
"emails": {
"risky": {
"accept_all": [
"raines@live.com",
"pajas@icloud.com"
]
}
}
}
}
```
- **Id (int)** - List ID
- **emails (array)** - Emails by primary/secondary results


## Export invalid emails from the list.....................................................................................................

**/api/lists/export/invalid/{listId}**

**Request**

In order to issue an export invalid emails API call, you will need the following values:

- **apiKey (string)** - Your API key
- **listId (int)** - List ID.

Request format:

**GET** : https://emailverifier.com/api/lists/export/invalid/{listId}?apiKey={apiKey}

**Response (JSON)**

```
{
"list": {
"id": 4,
"emails": {
"invalid": {
"invalid_account": [
"raines@live.com",
"pajas@icloud.com"
],
"invalid_domain": [
"raines@live.com",
"pajas@icloud.com"
]
}
}
}
}
```
- **Id (int)** - List ID
- **emails (array)** - Emails by primary/secondary results


## Export unknown emails from the list.................................................................................................

**/api/lists/export/unknown/{listId}**

**Request**

In order to issue an export unknown emails API call, you will need the following values:

- **apiKey (string)** - Your API key
- **listId (int)** - List ID.

Request format:

**GET** : https://emailverifier.com/api/lists/export/unknown/{listId}?apiKey={apiKey}

**Response (JSON)**

```
{
"list": {
"id": 4,
"emails": {
"unknown": {
"unknown": [
"raines@live.com",
"pajas@icloud.com"
]
}
}
}
}
```
- **Id (int)** - List ID
- **emails (array)** - Emails by primary/secondary results


## General response codes......................................................................................................................

- **200 Success**
- **400 Bad Request**
- **401 Unauthorized**
- **403 Forbidden**
- **404 Not Found**
- **429 Too Many Requests**
- **500 Internal Server Error**
- **502 Bad Gateway**
- **503 Service Unavailable**
- **504 Gateway Timeout**

**Error example (response codes 4xx – 5xx)**

```
{
"message":"API endpoint not implemented",
}
```

