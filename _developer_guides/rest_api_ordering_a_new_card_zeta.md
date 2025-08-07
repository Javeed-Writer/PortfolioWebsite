---
layout: guide
title: "Ordering a New Card For an Account Holder"
description: "REST API guide for ordering new cards in the Zeta platform, including API endpoints, request/response examples, and implementation details."
author: "Muhamed Javid Hussain"
date: 2024-08-10
tags: ["REST API", "Zeta Platform", "Card Ordering", "Financial Services"]
category: "developer_guides"
---
# Ordering a New Card For an Account Holder

## Table of Contents

1. [Ordering a New Card for an Account Holder](#ordering-a-new-card-for-an-account-holder)
   - [Prerequisites](#prerequisites)
2. [Information about the card](#information-about-the-card)
   - [Front Side Details](#front-side-details)
   - [Back Side Details](#back-side-details)
3. [Issue Bundle Flow](#issue-bundle-flow)
4. [Create a card order](#create-a-card-order)
   - [Issue a Bundle](#issue-a-bundle)
   - [API Reference](#api-reference)
   - [Request Parameters](#request-parameters)
   - [Response Parameters](#response-parameters)
   - [Status Codes and Errors](#status-codes-and-errors)
5. [Generate Embossing File](#generate-embossing-file)
   - [Get the Card ID](#get-the-card-id)
   - [Generating the Embossing File](#generating-the-embossing-file)

---

## Ordering a New Card for an Account Holder

### Prerequisites

Before you order a new card in fusion make sure that you understand the following:
- [Information about the card](#information-about-the-card)
- [Issue Bundle Flow](#issue-bundle-flow)

## Information about the card

When you order a card, you need to know the details listed in the card.

### Front Side Details

On the front side of the card, you can see the following details:
- Zeta logo and the bank name
- EMV (Europay Mastercard Visa) Chip
- 12-digit card number
- Validity details
- Your name
- Your organization name

### Back Side Details

On the back side of the card, you can see the following details:
- Magnetic Stripe
- CVV2

## Issue Bundle Flow

Issue Bundle Flow is the primary workflow used to create card order for your Account Holders.

**Short Description:** You can order a new card in fusion which is simple and adapts to your business and offers a solution that is secure and modern.

The Issue Bundle Flow involves the following:
1. [Create card order](#create-a-card-order)
2. [Generate embossing file](#generate-embossing-file)

## Create a card order

Creating a card order involves issuing a Bundle to the Account Holders. The fusion service processes the order and generates card data such as Card number, CCV1, CCV2, expiry.

### Issue a Bundle

Issue a bundle to the Account Holder.

### API Reference

**Method:** `POST`  
**URL:** `https://fusion.prepod.zeta.in/api/v1/ifi/{ifiID}/bundles/{bundleID}/issueBundle/{issueBundleID}`

Where:
- `{bundleID}` is the ID of the bundle
- `{issueBundleID}` is the ID of the issue bundle

#### Headers

| Header name | Description | Required | Values |
|-------------|-------------|----------|--------|
| Content-Type | The format of the data to be posted | Optional | application/json |
| Postman-Token | The access token from the postman | Required | |
| X-Zeta-AuthToken | The authorization access token returned | Required | |

#### Sample Request

```bash
curl -X POST \
https://fusion.preprod.zeta.in/api/v1/ifi/140827/bundles/ee64c930-c06f-49cb-9443-84b132b9d4e2/issueBundle \
-H 'Content-Type: application/json' \
-H 'Postman-Token: c98d6562-c8ff-4bcc-ba4f-c3f103951d11' \
-H 'X-Zeta-AuthToken: {{AUTH_TOKEN}}' \
-d '{
  "accountHolderID": "b23ef097-8d7e-41e9-86da-c2072db6fa25",
  "name": "Bundle",
  "phoneNumber": "+919191919191"
}'
```

### Request Parameters

#### POST Body

| Element | Description | Type | Required | Notes |
|---------|-------------|------|----------|-------|
| accountHolderID | ID of the Account Holder to whom the Bundle is issued | string | Required | |
| name | Name of the issued bundle | string | Required | |
| phoneNumber | Phone number associated with the Account Holder | Integer | Required | |

### Response Parameters

| Element | Description | Type | Notes |
|---------|-------------|------|-------|
| accounts | Indicates the values of the account | array | It contains the list of account data |
| paymentInstruments | Indicates the values of payment instruments | array | It contains the list of payment instrument data |
| bundleID | ID of the issued bundle | string | |
| accountHolderID | ID of the Account Holder | string | |
| accountID | ID of the Account Product | string | |
| resourceID | ID of the Resource associated with the Payment Instrument | integer | |
| status | Status of the issued bundle | string | The values are active, inactive, and deleted |
| targetAccount | When the Bundle do not have any Account Products, targetAccount shall be used in the Payment Instrument field | string | |

#### Sample Response

```json
{
  "accounts": [
    {
      "bundleID": "ee64c930-c06f-49cb-9443-84b132b9d4e2",
      "accountHolderID": "8563929b-f7f0-4e2f-9891-75a412383029",
      "accountID": "4cdb45da-0ae5-46bf-8aae-c0943a5aae3f"
    }
  ],
  "paymentInstruments": [
    {
      "bundleID": "ee64c930-c06f-49cb-9443-84b132b9d4e2",
      "resourceID": "908b3067",
      "status": "ACTIVE",
      "targetAccount": "4cdb45da-0ae5-46bf-8aae-c0943a5aae3f"
    }
  ]
}
```

### Status Codes and Errors

The following table lists the returned HTTP status codes:

| Code | Description | Notes |
|------|-------------|-------|
| 201 | Order placed | Success |
| 400 | Bad Request | The request is invalid |
| 401 | Unauthorized | Forbidden Request |
| 409 | Already Exists | The order already exists in the system |

---

## Generate Embossing File

Embossing is the process of imprinting the card with various details such as card number, name, Account Holder name, Account Holder organization and the process is carried by an external vendor.

Once the card data is successfully generated, you must generate an embossing file with the generated data. All the card data and the order must be populated in the embossed file. The embossed file is shared with the external vendor and prints the card with the specification in the file.

**Key Points:**
- An Account Holder is represented as a Resource ID
- Form Factor ID, Card ID, CGUID represents the card
- When you issue a Bundle to an Account Holder, Fusion generates Resource ID and a Form Factor ID
- The card ID is required to dispatch the embossed file to the external vendor

The process involves:
1. [Get the Card ID](#get-the-card-id)
2. [Generating embossing file](#generating-the-embossing-file)

### Get the Card ID

Retrieve the card ID details.

**Method:** `GET`  
**URL:** `https://fusion.prepod.zeta.in/api/v1/ifi/{ifiID}/orders/{orderID}/cards/list`

Where:
- `{ifiID}` is the ID of the IFI (Issuing Financial Institution)
- `{orderID}` is the ID of the card order

#### Headers

| Header name | Description | Required | Values |
|-------------|-------------|----------|--------|
| Content-Type | The format of the data to be posted | Optional | application/json |
| Authorization | The access token | Required | |

#### Sample Request

```bash
curl -X GET 'https://fusion.preprod.zeta.in/api/v1/ifi/140827/orders/e8f6e34f-1deb-4e28-84ee-5c4d73dd7364/cards/list' \
-H 'Authorization: {{AUTH_TOKEN}}' \
-H 'Content-Type: application/json'
```

#### Response Parameters

| Element | Description | Type | Notes |
|---------|-------------|------|-------|
| resource | The values of the resource | list | |
| uri | Resource ID number for which data are returned | string | |
| cardID | The ID of the order card for which data are returned | string | |
| crn | The ID of the card reference number | integer | |
| cardType | The type of card | string | The values are physical and digital |
| maskedPan | The masked PAN number of the account holder | string | |
| cardStatus | The status of the card | string | The values are Enabled, Disabled, and Inactive |
| orderDetails | The values of the order details | list | |
| orderID | The order ID of the ordered card | string | |
| cardSku | The list providing the vendor details and card ID | list | |
| cardSkuID | The ID for the card type | string | Values are RUPAY, VISA, Mastercard |
| productID | The ID of the card type | integer | |
| ifi | The ID of the IFI | integer | |
| bin | | integer | |
| plasticCode | The code for coating the card | string | Default value is Whitec |
| vendorID | The ID number of the vendor | string | |
| range | | integer | |
| thirdLineEmbossing | The name of the account holder | string | |
| fourthLineEmbossing | The name of the account holder employer | string | |
| expiry | List of the expiry details | list | |
| month | The month for the expiry | integer | |
| year | The year for the expiry | integer | |
| deliveryAddress | List of the values for delivery | list | |
| country | The country code for delivery | string | |
| city | The city for the delivery | string | |
| postalCode | The postal code for the delivery address | integer | |
| contactNumber | The contact number of the person in the delivery address | integer | |
| addressLine1 | The 1st line of the delivery address | string | |
| addressLine2 | The 2nd line of the delivery address | string | |
| addressLine3 | The 3rd line of the delivery address | string | |
| state | The state of the delivery address | string | |
| addressLine4 | The 4th line of the delivery address | string | |
| orderStatus | The status of the ordered card | string | |

#### Sample Response

```json
[
  {
    "resource": {
      "uri": "resource://a8e27abb-3490-4b4b-9a5f-82de543f8b96",
      "attributes": {}
    },
    "cardID": "f0c1bb24-d800-4024-bd34-67a61aaab588",
    "crn": "167635292871",
    "cardType": "PHYSICAL",
    "maskedPan": "508645-xxxxxx-7335",
    "cardStatus": "ENABLED",
    "orderDetails": {
      "orderID": "e8f6e34f-1deb-4e28-84ee-5c4d73dd7364",
      "cardSkuID": "RUPAY_ABC_VBO_PHYSICAL",
      "cardSku": {
        "cardSkuId": "RUPAY_ABC_VBO_PHYSICAL",
        "productID": "201383917079881",
        "ifi": "140827",
        "bin": "508645",
        "plasticCode": "WHITEC",
        "vendorID": "SESHAASAI_VBO_FAMPAY",
        "tags": [],
        "range": "32"
      },
      "plasticCode": "WHITEC",
      "thirdLineEmbossing": "",
      "fourthLineEmbossing": "",
      "expiry": {
        "month": "2",
        "year": "30"
      },
      "deliveryAddress": {
        "country": "IN",
        "city": "dkwkr",
        "postalCode": "400050",
        "contactNumber": "9867962795",
        "addressLine1": "Test Line 1",
        "addressLine2": "Test2",
        "addressLine3": "Test Line 3",
        "state": "dfejhr",
        "addressLine4": "Test2"
      },
      "tenantAttributes": {},
      "orderStatus": "CARD_DATA_GENERATED"
    },
    "tenantAttributes": {},
    "binRange": {
      "bin": "508645",
      "range": "32"
    }
  }
]
```

#### Status Codes and Errors

| Code | Description | Notes |
|------|-------------|-------|
| 201 | Order placed | Success |
| 401 | Unauthorized | Forbidden Request |
| 404 | Order does not exist | Invalid order id |
| 409 | Already Exists | The order already exists in the system |

### Generating the Embossing File

Fusion Embossing service generates the embossing file for each product type or IFI. To generate the embossing file, use the cardID generated using the GET method.

**Method:** `POST`  
**URL:** `https://fusion.preprod.zeta.in/api/v1/ifi/{ifiID}/cards/{cardID}/dispatch`

Where:
- `{ifiID}` is the ID of the IFI
- `{cardID}` is the ID of the ordered card

#### Headers

| Header name | Description | Required | Values |
|-------------|-------------|----------|--------|
| Content-Type | The format of the data to be posted | Optional | application/json |
| Authorization | The access token for authorization | Required | |

#### Sample Request

```bash
curl -X POST 'https://fusion.preprod.zeta.in/api/v1/ifi/140827/cards/f0c1bb24-d800-4024-bd34-67a61aaab588/dispatch' \
-H 'Content-Type: application/json' \
-H 'Authorization: {{AUTH_TOKEN}}' \
--data-raw '{
  "plasticCode": "WHITEC",
  "thirdLineEmbossing": "Rohit",
  "fourthLineEmbossing": "Joseph",
  "deliveryAddress": {
    "name": "Farzan Shaikh",
    "addressLine1": "Directiplex",
    "addressLine2": "Near Andheri Subway",
    "addressLine3": "Old Nagardas Road",
    "addressLine4": "Andheri East",
    "city": "Mumbai",
    "state": "Maharashtra",
    "country": "India",
    "postalCode": "400069",
    "contactNumber": "+919090909090"
  },
  "vendorAttributes": {
    "shipping-partner.name": "BLUEDART",
    "shipping-partner.trackingNumber": "098137643242412312414",
    "welcome-kit.fullName": "Rohit Joseph",
    "welcome-kit.qrCode": "c48tn73793cfh93cg24c872t478r4ft3ubdcy8g3cr",
    "welcome-kit.templateID": "template-id"
  }
}'
```

#### POST Body Parameters

| Element | Description | Type | Required | Notes |
|---------|-------------|------|----------|-------|
| data-raw | Original data for embossing | list | Required | |
| plasticCode | Unique identifier of the design that the card will be printed | string | Optional | Default value is Whitec |
| thirdLineEmbossing | Account Holder Name displayed on the card | string | Optional | |
| fourthLineEmbossing | Additional Account Holder Details like company name displayed on the card | string | Optional | |
| deliveryAddress | Address where the cards should be delivered | list | Required | |
| name | Name of the tenant | string | Required | |
| addressLine1 | The 1st line of the delivery address | string | Required | |
| addressLine2 | The 2nd line of the delivery address | string | Required | |
| addressLine3 | The 3rd line of the delivery address | string | Required | |
| city | The city of the delivery address | string | Required | |
| state | The state of the delivery address | string | Required | |
| country | The country of the delivery address | string | Required | |
| postalCode | The postal code of the delivery address | integer | Required | |
| contactNumber | The contact number of the tenant | integer | Required | |
| vendorAttributes | The list of the external vendor details | list | Optional | |
| shipping-partner.name | The name of the external vendor | string | Optional | |
| shipping-partner.trackingNumber | The tracking number for the shipping partner | integer | Optional | |
| welcome-kit.fullName | The name of the welcome kit | string | Optional | |
| welcome-kit.qrCode | The QR code for the welcome kit | string | Optional | |
| welcome-kit.templateID | The Template ID of the welcome kit | string | Optional | |

#### Response Parameters

| Element | Description | Type | Notes |
|---------|-------------|------|-------|
| quantity | The values of the resource | list | |
| orderID | Resource ID number for which data are returned | string | |
| cardSkuID | The ID for the card type | string | Values are RUPAY, VISA, Mastercard |
| plasticCode | The code for coating the card | string | Default value is Whitec |
| expiry | List of the expiry details | list | |
| month | The month for the expiry | integer | |
| year | The year for the expiry | integer | |
| thirdLineEmbossing | The name of the account holder | string | |
| fourthLineEmbossing | The name of the account holder employer | string | |
| deliveryAddress | Address where the cards should be delivered | list | |
| name | Name of the tenant | string | |
| addressLine1 | The 1st line of the delivery address | string | |
| addressLine2 | The 2nd line of the delivery address | string | |
| addressLine3 | The 3rd line of the delivery address | string | |
| city | The city of the delivery address | string | |
| state | The state of the delivery address | string | |
| country | The country of the delivery address | string | |
| postalCode | The postal code of the delivery address | integer | |
| contactNumber | The contact number of the tenant | integer | |
| vendorAttributes | The list of the external vendor details | list | |
| welcome-kit.qrCode | The QR code for the welcome kit | string | |
| welcome-kit.fullName | The name of the welcome kit | string | |
| shipping-partner.name | The name of the external vendor | string | |
| welcome-kit.templateID | The Template ID of the welcome kit | string | |
| shipping-partner.trackingNumber | The tracking number for the shipping partner | integer | |
| orderStatus | The status of the ordered card | string | |
| orderedAt | The date and time for which the new card is ordered | string | |

#### Sample Response

```json
{
  "quantity": 1,
  "orderID": "e8f6e34f-1deb-4e28-84ee-5c4d73dd7364",
  "cardSkuID": "RUPAY_ABC_VBO_PHYSICAL",
  "plasticCode": "WHITEC",
  "expiry": {
    "month": 0,
    "year": 10
  },
  "thirdLineEmbossing": "Rohit",
  "fourthLineEmbossing": "Joseph",
  "deliveryAddress": {
    "name": "Farzan Shaikh",
    "addressLine1": "Directiplex",
    "addressLine2": "Near Andheri Subway",
    "addressLine3": "Old Nagardas Road",
    "addressLine4": "Andheri East",
    "city": "Mumbai",
    "state": "Maharashtra",
    "country": "India",
    "postalCode": "400069",
    "contactNumber": "+919090909090"
  },
  "tenantAttributes": {},
  "vendorAttributes": {
    "welcome-kit.qrCode": "c48tn73793cfh93cg24c872t478r4ft3ubdcy8g3cr",
    "welcome-kit.fullName": "Rohit Joseph",
    "shipping-partner.name": "BLUEDART",
    "welcome-kit.templateID": "template-id",
    "shipping-partner.trackingNumber": "098137643242412312414"
  },
  "additionalAttributes": {},
  "orderStatus": "EMBOSSING_FILE_PENDING",
  "orderedAt": "2020-03-19T16:15:04.131Z"
}
```

#### Status Codes and Errors

| Code | Description | Notes |
|------|-------------|-------|
| 201 | Order placed | Success |
| 400 | Bad Request | The request is invalid |
| 401 | Unauthorized | Forbidden Request |
| 409 | Already Exists | The order already exists in the system |
| 422 | Card cannot be dispatched | Invalid address |
| 500 | Error occurred. Please try again later | The system is not connected to internet |
