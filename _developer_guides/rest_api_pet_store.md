---
layout: guide
title: "Working with Pet Store API"
description: "Comprehensive guide for Pet Store API endpoints, including creating and managing pet purchase orders with practical examples and code samples."
author: "Muhamed Javid Hussain"
date: 2024-11-15
tags: ["REST API", "Pet Store", "Swagger", "API Documentation"]
category: "developer_guides"
---

# Working with Pet Store API

## Overview
This guide provides a list of API endpoints and how to create and delete an order.

## Table of Contents
- [Pet Store Release](#pet-store-release)
- [Pet Store API Endpoints](#pet-store-api-endpoints)
- [Sunset Endpoint](#sunset-endpoint)
- [Creating a Pet Purchase Order](#creating-a-pet-purchase-order)


## Pet Store Release
In this release, we have introduced a new _petstore_ website. The petstore is a store where
you can create and manage the pets, order a pet for purchase, and so on. See [petstore](https://petstore.swagger.io) for
more information.

## Pet Store API Endpoints
### Petstore

- **[POST /pet](https://petstore.swagger.io/#/pet/addPet)**  
  Introduced a new API endpoint that is used to create a pet such as cat, dog, or puppy in the petstore.
- **[POST /uploadImage](https://petstore.swagger.io/#/pet/uploadFile)**  
  Introduced a new API endpoint that allows you to upload an image for the existing pet in the petstore.
- **[GET /petId](https://petstore.swagger.io/#/pet/getPetById)**  
  Introduced a new API endpoint that allows you to retrieve the information about the existing pet in the petstore.
- **[GET /findByStatus](https://petstore.swagger.io/#/pet/findPetsByStatus)**  
  Introduced a new API endpoint that allows you to retrieve the information about the pet by using their status such as available, pending, and sold.
- **[POST /petId](https://petstore.swagger.io/#/pet/updatePetWithForm)**  
  Introduced a new API endpoint that enables you to update a pet information in the store in a form-data format.
- **[PUT /pet](https://petstore.swagger.io/#/pet/updatePet)**  
  Introduced a new API endpoint that allows you to update an existing pet in the petstore.
- **[DELETE /petId](https://petstore.swagger.io/#/pet/deletePet)**  
  Introduced a new API endpoint that allows you to delete a pet in the petstore.
### Petstore Order Management
- **[POST /order](https://petstore.swagger.io/#/store/placeOrder)**  
  Introduced a new API endpoint that allows you to place an order for purchasing a pet in the petstore.
- **[GET /orderId](https://petstore.swagger.io/#/store/getOrderById)**  
  Introduced a new API endpoint that allows you to retrieve the details of the pet purchase order in the petstore.
- **[GET /inventory](https://petstore.swagger.io/#/store/getInventory)**  
  Introduced a new API endpoint that enables you to return a map of status codes to quantities in the petstore.
- **[DELETE /orderId](https://petstore.swagger.io/#/store/deleteOrder)**  
  Introduced a new API endpoint that allows you to delete the purchase order in the petstore.

## Sunset Endpoint
The `GET /pet/findByTags` endpoint is deprecated and in the sunset phase.

This endpoint retrieves the pets based on the tags which affect the emotional state of the pet lovers. You can use the `GET /pet/findByStatus` to retrieve the pets based on the pet's availability which is an alternative to the `GET /pet/findByTags`. By using the `GET /pet/findByStatus`, the pet lovers emotional state is compromised.

**The sunset is scheduled on January 10, 2023.**

## Creating a Pet Purchase Order
_Learn how to create a pet purchase order._
In this topic, you will learn how to create a pet, upload an image for the pet, and create a purchase order for the pet.

### Endpoints

- **[POST /pet](https://petstore.swagger.io/#/pet/addPet)** - creates a pet in the petstore
- **[POST /uploadImage](https://petstore.swagger.io/#/pet/uploadFile)** - adds an image to the pet
- **[POST /order](https://petstore.swagger.io/#/store/placeOrder)** - creates a purchase order for the pet
- **[DELETE /orderId](https://petstore.swagger.io/#/store/deleteOrder)** - optionally, you can delete the purchase order

### Basic Steps

1. Create a pet in your petstore.
2. Add an image to the pet in your petstore.
3. Create a purchase order for the pet.
4. Delete a purchase order if you have placed an inappropriate order.

<img src="./Petstore_process.png" alt="Pet Purchase Order Process" style="width:6.49097in;height:7in" />

### Example

In this example, as a user, I would like to purchase a pet from the petstore.

#### Step 1: Create a pet in petstore

First, you must create a pet object with a category, tags, and status that needs to be added to the petstore.

Make a POST request with category, tags, and status.

This is the example of the POST request:

```bash
curl -X 'POST' \
'https://petstore.swagger.io/v2/pet' \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
-d '{
"id": 8764,
"category": {
"id": 34623,
"name": "Kitten"
},
"name": "Garfield Dom",
"photoUrls": [
"photo"
],
"tags": [
{
"id": 97487,
"name": "cat"
}
],
"status": "available"
}'
```

Upon sending the request, the pet is created in the petstore.

This is the response for the POST request:

```json
{
  "status": "success",
  "message": "",
  "data": {
    "id": 8764,
    "category": {
      "id": 34623,
      "name": "Kitten"
    },
    "name": "Garfield Dom",
    "photoUrls": [
      "photo"
    ],
    "tags": [
      {
        "id": 97487,
        "name": "cat"
      }
    ],
    "status": "available"
  }
}
```

#### Step 2: Add an image to the pet in your petstore

Secondly, add an image of the cat in the petstore. You can add the latest image of your cat.

Make a POST request to upload the image to the petstore.

This is the example POST request:

```bash
curl -X 'POST' \
'https://petstore.swagger.io/v2/pet/8764/uploadImage' \
-H 'accept: application/json' \
-H 'Content-Type: multipart/form-data' \
-F 'additionalMetadata=This is the pic of the garfield dom.' \
-F 'file=@garfield.jpg;type=image/jpeg'
```

This is the response for the POST request:

```json
{
  "code": 200,
  "type": "unknown",
  "message": "additionalMetadata: This is the pic of the garfield dom.\nFile uploaded to ./garfield.jpg, 3338467 bytes"
}
```

#### Step 3: Create a purchase order for the pet

Finally, create a purchase order for the pet (8764: Garfield Dom) in the petstore.

Make a POST request to create a purchase order for the pet.

This is the example POST request:

```bash
curl -X 'POST' \
'https://petstore.swagger.io/v2/store/order' \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
-d '{
"id": 26543,
"petId": 8764,
"quantity": 5,
"shipDate": "2022-07-10T16:04:26.381Z",
"status": "placed",
"complete": true
}'
```

This is the response for the POST request:

```json
{
  "id": 26543,
  "petId": 8764,
  "quantity": 5,
  "shipDate": "2022-07-10T16:04:26.381+0000",
  "status": "placed",
  "complete": true
}
```

#### Step 4: Delete a purchase order

This is an optional step. If you have placed an inappropriate purchase order, you can delete it and place a new purchase order.

Make a DELETE request with the order ID: 26543.

```bash
curl -X 'DELETE' \
'https://petstore.swagger.io/v2/store/order/26543' \
-H 'accept: application/json'
```

This is the response for the DELETE request:

```json
{
  "code": 200,
  "type": "unknown",
  "message": "26543"
}
```


  



