---
title: UK Internal Market Scheme (UKIMS) Authorisation Checker API Service Guide
weight: 1
description: For Fast Parcel Operators (FPOs), Software developers, Agents transporting goods GB/NI, B2B Agents for GB/NI, third party software houses, designers, product owners or business analysts. Processes involved in passing EORI numbers to check UKIM validity.
---
Version 1.0 issued May 2 2024

# UK Internal Market Scheme (UKIMS) Authorisation Checker API Service Guide
This document introduces Fast Parcel Operators (FPOs), software developers and other third party users to the UK Internal Market Scheme (UKIMS) Authorisation Checker API. Learn here about the processes involved in passing EORI numbers to check UKIM validity.

## API Overview 

This API allows Fast Parcel Operators (FPOs) software developers and third party users to check if the holders of provided EORI Numbers have a valid UKIM authorisation.
The aim is to avoid possible rejections of goods moving through from GB-NI, which could physically stop the B2B parcels.
    
The API is based on REST principles with a single POST method endpoint that returns data in JSON format. It uses standard HTTP error response codes. Use the API to request the UKIM Authorisation Status of between 1 to 3000 EORIs passed as an array. 
    
**Note:** The API endpoint relates only to Great Britain and Northern Ireland.

### What is an EORI?
The acronym EORI stands for Economic Operators Registration and Identification. It is a unique identification number used by customs authorities throughout the European Union (EU) 12. This system, instituted on July 1, 2009, replaced the older Trader’s Unique Reference Number (TURN). The EORI number plays a critical role in facilitating the import and export of goods both within the EU and with countries outside of it. Whether you’re a business or an individual, understanding the EORI system is crucial if you plan to engage in international trade. For those based in the UK, HM Revenue and Customs (HMRC) are responsible for allocating these numbers.

A breakdown of the EORI number format for UK VAT-registered businesses:

- GB: Indicates that the business is based in the UK.
- 205672212: Represents the business’s VAT Registration Number.
- 000: These three zeros are always added to the end of a UK EORI number.

In summary, having an EORI number is essential for anyone involved in international trade, as it allows customs authorities to monitor and track shipments effectively.

## API Status

This version of the UKIMS Checker API:

- supports **only** the UKIMS Checker API v1.0
- is currently **not** ready for testing
- will **not** be ready for use in production until the service goes live 

### Use the API to:

- Request the UKIM Authorisation Status of 1-3000 EORIs passed as an array.

The API endpoint relates only to Great Britain and Northern Ireland.  Eventually, you can also use the HMRC sandbox environment to run tests for Great Britain and Northern Ireland transit movements.

## Developer Setup

To develop using the UKIMS Checker API you must:

- be familiar with HTTP, RESTful services, XML and OAuth2
- be registered as a developer on the HMRC Developer Hub

You can view all the applications you have currently registered on the Developer Hub Applications page, where you can also administer API subscriptions and application credentials.

## Getting started

Making API requests
Before sending any requests to UKIMS Checker API, make sure that you are using the following points
in your software:
- the correct URL for the environment and API version number (see below)
- the correct header contents and payload information - see Fast Parcel Operators reference guide

The base URLs of the sandbox and production environments are as follows:

```code
Sandbox	https://test-api.service.hmrc.gov.uk/customs/uk-internal-market/authorisations

Production https://api.service.hmrc.gov.uk/customs/uk-internal-market/authorisations
```

### Validating a collection of EORI numbers

Link to POST method in  UKIMS Checker API v1.0 Reference Guide [here](ukim-auth-checker-api/resources/public/api/conf/1.0).

### Example of a POST request

```curl
curl --location POST 'https://test-api.service.hmrc.gov.uk/customs/uk-internal-market/authorisations
{
"date": "2024-02-31",
"eoris": [
"GB123123123123"
]
}
```

Example of a successful response:

```code
{
  "date": "2024-02-31",
  "eoris": [
    {
      "eori": "GB123123123123",
      "authorised": true
    }
  ]
}
```

An example of an unsuccessful response:

```code
{
  "code": "MISSING_CREDENTIALS",
  "message": "Authentication information is not provided"
}
```

## Error Responses

An introduction to the expected Error Responses:

```code          
400
description: 
Validation failure. The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid request message framing, or deceptive request routing).
```

```code
401
description:
Unauthorised. Authentication information is not provided.
```                

```code
403
description:
Forbidden. The client does not have access rights to the content; that is, it is unauthorized, so the server is refusing to give the requested resource. Unlike 401 Unauthorized, the client's identity is known to the server.
```

```code
405
description:
Method not allowed. The request method is known by the server but is not supported by the target resource. 
```

```code
406
description:
Not acceptable. This response is sent when the web server, after performing server-driven content negotiation, doesn't find any content that conforms to the criteria given by the user agent.
```

```code
429
description:
Request in excess of rate limit. The user has sent too many requests in a given amount of time ( See "API rate limiting" below).
```

```code
500
description: 
Internal Server Error. The server has encountered a situation it does not know how to handle.
```
 
## API rate limiting
Each software house should register a single application with HMRC. This application will be used to identify the software house during the OAuth 2.0 grant flow and will also be used in subsequent per user API calls. We limit the number of requests that each application can make. This protects our backend service against excessive load and encourages real-time API calls over batch processing.

We set limits based on anticipated loads and peaks. Our standard limit is 3 requests per second per application. If you believe that your application will sustain traffic load above this value, contact the SDS Team at email [SDSTeam@hmrc.gov.uk](mailto:SDSTeam@hmrc.gov.uk)k


