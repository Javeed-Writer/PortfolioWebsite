---
layout: guide
title: "Working with Content Filter"
description: "GraphQL API implementation guide for content filtering systems, including query examples, mutations, and content moderation workflows."
author: "Muhamed Javid Hussain"
date: 2024-12-01
tags: ["GraphQL", "Content Filter", "API Documentation", "Content Moderation"]
category: "developer_guides"
---

# Working with Content Filter
## Table of Contents

- [Working with Content Filter](#working-with-content-filter)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Create a Content Filter](#create-a-content-filter)
    - [Constraints](#constraints)
    - [Example](#example)
  - [Retrieve Content Filters](#retrieve-content-filters)
  - [Update a Content Filter](#update-a-content-filter)
  - [Delete a Content Filter](#delete-a-content-filter)
  - [Create or Update Filter Words](#create-or-update-filter-words)
  - [Remove Filter Words](#remove-filter-words)

## Overview

Aurora offers content filtering capabilities that serve as moderation tools to safeguard against the publication of objectionable materials in various areas, including posts, replies, tags, private messages, profile details, and member registrations.

When community members use inappropriate language, the content filters detect it and take appropriate action. This may include blocking the content from being published or substituting the words with predetermined alternatives. Additionally, there are instances where the filters may log the objectionable content without manual intervention.

## Create a Content Filter

Use the `createContentFilter` mutation to create a content filter for your community. When a user creates a message with inappropriate words, the content filter will moderate and take necessary action as per the community policies.

### Constraints

The constraints for the content filter are:

| Field | Description | Required |
|-------|-------------|----------|
| `name` | The content filter name. | Required |
| `action` | The action to be taken when the filter is applied.<br>Supported values: `REPLACE`, `DENY`, `DENYSTR`, `GRADE`, `OFF` | Required |
| `notify` | To notify the moderator when a content filter action takes place. (Boolean) | Required |
| `replacement` | The replacement word for the filter | Optional |
| `applyTo` | The application of the filter to the community entities.<br>Supported values: `MESSAGE`, `NOTES`, `TAG`, `USER`, `MEDIA`, `CAPTIONS`, `LOGIN` | Required |
| `allowDiacriticsFilter` | To allow the diacritics filter. (Boolean) | Optional |
| `wordList` | The list of filter words and the corresponding replacement string that must be used in your community. | Optional |

### Example

Use the GraphQL mutation below to create a Block Words filter in your community.

```graphql
mutation {
  createContentFilter(
    name: "Block words"
    action: OFF
    notify: false
    applyTo: [MESSAGE, MEDIA, TAG]
    replacement: "none"
    allowDiacriticsFilter: false
  ) {
    result {
      name
      action
      notify
      replacement
      applyTo
    }
    errors {
      __typename
      ... on Error {
        fields
        message
      }
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "createContentFilter": {
      "result": {
        "name": "Block words",
        "action": "OFF",
        "notify": false,
        "replacement": "none",
        "applyTo": [
          "MESSAGE",
          "MEDIA",
          "TAG"
        ]
      },
      "errors": null
    }
  }
}
```

## Retrieve Content Filters

Use the `contentFilters` query to retrieve all the content filters used in your community. The fields used to retrieve content filters are the same as the ones used to create content filters.

```graphql
query {
  contentFilters {
    name
    action
    filterWords {
      word
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "contentFilters": [
      {
        "name": "abhiram22",
        "action": "REPLACE",
        "filterWords": [
          {
            "word": "hul2"
          },
          {
            "word": "á¸¥Ãºl"
          }
        ]
      },
      {
        "name": "absurd",
        "action": "REPLACE",
        "filterWords": [
          {
            "word": "0"
          },
          {
            "word": "absrud"
          },
          {
            "word": "absurds"
          },
          {
            "word": "dd"
          }
        ]
      }
    ]
  }
}
```

## Update a Content Filter

Use the `editContentFilter` mutation to update the content filter name, its application, and notification.

The fields used to update content filters are the same as the ones used to create new content filters.

As an example, you can update the Block words filter with the application to the Message.

```graphql
mutation {
  editContentFilter(
    name: "Block words"
    notify: false
    allowDiacriticsFilter: false
    applyTo: [MESSAGE]
  ) {
    result {
      name
      action
      notify
      replacement
      applyTo
    }
    errors {
      __typename
      ... on Error {
        fields
        message
      }
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "editContentFilter": {
      "result": {
        "name": "Block words",
        "action": "OFF",
        "notify": false,
        "replacement": "none",
        "applyTo": [
          "MESSAGE"
        ]
      },
      "errors": null
    }
  }
}
```

## Delete a Content Filter

Use the `deleteContentFilter` mutation to delete a content filter.

```graphql
mutation {
  deleteContentFilter(name: "Block words") {
    result
    errors {
      __typename
      ... on Error {
        fields
        message
      }
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "deleteContentFilter": {
      "result": true,
      "errors": null
    }
  }
}
```

## Create or Update Filter Words

Use the `createOrUpdateFilterWords` mutation to add or update the filter words of your content filter.

```graphql
mutation {
  createOrUpdateFilterWords(
    filterName: "Block words"
    wordList: { word: "yip" }
  ) {
    result {
      name
      action
      notify
      replacement
      applyTo
      filterWords {
        word
      }
    }
    errors {
      __typename
      ... on Error {
        fields
        message
      }
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "createOrUpdateFilterWords": {
      "result": {
        "name": "Block words",
        "action": "OFF",
        "notify": false,
        "replacement": "none",
        "applyTo": [
          "MESSAGE",
          "MEDIA",
          "TAG"
        ],
        "filterWords": [
          {
            "word": "yip"
          }
        ]
      },
      "errors": []
    }
  }
}
```

## Remove Filter Words

Use the `removeFilterWords` mutation to remove filter words from your content filter.

```graphql
mutation {
  removeFilterWords(filterName: "Block words", wordList: "yip") {
    errors {
      __typename
      ... on Error {
        fields
        message
      }
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "removeFilterWords": {
      "errors": []
    }
  }
}
```
