---
layout: guide
title: "Working with Content Archive"
description: "Comprehensive guide for implementing content archival systems using GraphQL, including data retention policies and content lifecycle management."
author: "Muhamed Javid Hussain"
date: 2024-09-15
tags: ["GraphQL", "Content Archive", "Data Management", "API Documentation"]
category: "developer_guides"
---
# Working with Content Archive

## Table of Contents

1. [About Content Archive](#about-content-archive)
2. [Permission for Content Archive](#permission-for-content-archive)
   - [Community Policy](#community-policy)
   - [Archival Settings](#archival-settings)
   - [Enable the Content Archive](#enable-the-content-archive)
3. [Archiving a Message](#archiving-a-message)
4. [Updating the Related URL of an Archived Message](#updating-the-related-url-of-an-archived-message)
5. [Retrieving the Archival Information of a Message](#retrieving-the-archival-information-of-a-message)
6. [Retrieving all the Archived Messages](#retrieving-all-the-archived-messages)
7. [Unarchiving a Message](#unarchiving-a-message)

---

## About Content Archive

Large enterprise communities tend to generate a substantial amount of content over time that can result in clutter filled with outdated, misleading, or irrelevant information, making it harder for users to locate what they need. Upholding good content hygiene is vital to maintaining a healthy and engaging environment.

Admins and moderators should routinely review the community content and archive any information that is no longer accurate, timely, or relevant.

Community Aurora provides a content archive feature to archive the following content:

- Discussion
- Blog
- KB content
- Idea
- Event

## Permission for Content Archive

> **Note:** For Aurora v24.11 and above, the content archive feature is enabled by default.

Use the archival settings to know whether the content archive feature is enabled for your community. Furthermore, use the community policy to know if the community member has the required permission to archive the content.

### Community Policy

Community policy is used to determine if a community member has required permission to archive a message.

If the GraphQL query returns no error, then it implies that the user has permission to archive the content.

```graphql
{
  coreNode(id: "community:Docscommunity") {
    id
    title
    ... on Community {
      communityPolicies {
        canArchiveMessage {
          failureReason {
            message
            key
            args
          }
        }
      }
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "coreNode": {
      "id": "community:Docscommunity",
      "title": "calvin2",
      "communityPolicies": {
        "canArchiveMessage": {
          "failureReason": null
        }
      }
    }
  }
}
```

### Archival Settings

Use the content archival settings query to check the community archival settings such as external URL and content archive.

```graphql
{
  community {
    contentArchiveSettings {
      isContentArchiveEnabled {
        key
        value
      }
      isContentArchiveExternalUrlsEnabled {
        key
        value
      }
    }
  }
}
```

In the above query, we have requested the key and value of the `contentArchiveSettings` input. The description of the inputs are as follows:

- `isContentArchiveEnabled` - If this value is set as `true`, the content archive feature is turned on for your community.
- `isContentArchiveExternalUrlsEnabled` - If this value is set as `true`, the external URLs are turned on for your community. When someone accesses the archived content, the external URL redirects the member for more information.

**GraphQL Response:**

```json
{
  "data": {
    "community": {
      "contentArchiveSettings": {
        "isContentArchiveEnabled": {
          "key": "content-archivals.enable_content_archival_settings",
          "value": true
        },
        "isContentArchiveExternalUrlsEnabled": {
          "key": "content-archivals.allow_external_url",
          "value": true
        }
      }
    }
  }
}
```

### Enable the Content Archive

> **Note:** Only Administrators and Moderators can enable the content archive feature.

Use the `setContentArchiveSettingsOnCommunity` mutation to enable the content feature and external URL.

When archiving, you can provide an external URL that is outside your community. When someone accesses this link, they will be redirected to the external URL for related content.

```graphql
mutation {
  setContentArchiveSettingsOnCommunity(settingsInput:{
    isContentArchiveEnabled: true,
    isContentArchiveExternalUrlsEnabled: true
  }){
    result
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "setContentArchiveSettingsOnCommunity": {
     "result": true
    }
  }
}
```

## Archiving a Message

Use the `archiveMessage` mutation to archive all the supported content types in the Aurora community. Ensure that the content archive feature is enabled in your community.

The fields of the mutation are given in the below table:

| Field | Description | Required |
|-------|-------------|----------|
| messageId | The message ID you want to archive. | Required |
| relatedUrl | This is the related or external URL for the archived content. When members access the archived content, they will be redirected to this URL. | Optional |

### Example

Use the example below to archive a message (ID: 3376).

```graphql
mutation {
  archiveMessage(
    messageId: "message:3376"
    relatedUrl: "https://[YOUR COMMUNITY DOMAIN].com/idea/ideas/externalvideo/328wsw1/comments/3691"
  ) {
    result
    errors {
      __typename
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "archiveMessage": {
      "result": true,
      "errors": null
    }
  }
}
```

## Updating the Related URL of an Archived Message

Use the `updateRelatedContentUrlForArchivedMessage` mutation to add or update the related URL of an archived message.

The related URL of an archived message is used to guide the community members to an external URL that has the related content of the archived message. With this content, the user can learn information about the archived message.

The fields of the mutation are given in the below table:

| Field | Description | Required |
|-------|-------------|----------|
| messageId | The archived message ID you want to add or update. | Required |
| relatedUrl | The related URL you want to add or update to the archived message. When community members access the archived content, they will be redirected to this URL for related content. | Optional |

### Example

Use the example below to update the related URL of the archived message (ID: 3374).

```graphql
mutation {
  updateRelatedContentUrlForArchivedMessage(
    messageId: "message:3374",
    relatedUrl:"https://docscommunity.com/"
  ) {
    result
    errors {
      __typename
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "updateRelatedContentUrlForArchivedMessage": {
      "result": true,
      "errors": null
    }
  }
}
```

## Retrieving the Archival Information of a Message

For admin users and members or anonymous users, use the message and message archival data query to retrieve archived message information, respectively.

### Admin Query

As an admin, use the query below to retrieve the archival information for a message (ID: 435).

```graphql
{
  message(id:"message:435"){
    ...on ForumTopicMessage {
      archivalData {
        archivedDate
        suggestedUrl
      }
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "message": {
      "archivalData": {
        "archivedDate": "2024-11-15T13:59:12.000+05:30",
        "suggestedUrl": "https://[YOUR COMMUNITY DOMAIN/EXTERNAL URL]/"
      }
    }
  }
}
```

### Community Member Query

As a community member, use the query below to retrieve the archival information for a message (ID: 435).

```graphql
{
  messageArchivalData(messageId:"message:435"){
    archivedDate
    suggestedUrl
    archived
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "messageArchivalData": {
      "archivedDate": "2024-11-15T13:59:12.000+05:30",
      "suggestedUrl": "https://[YOUR COMMUNITY DOMAIN/EXTERNAL URL]/",
      "archived": true
    }
  }
}
```

## Retrieving all the Archived Messages

Use the `archivedMessages` query to retrieve all the archived messages.

The constraints for the `archivedMessages` query are:

| Field | Description | Required |
|-------|-------------|----------|
| nodeId | The node ID where you want to retrieve all the archived messages information. | Optional |
| archivedBy | The community member ID who archived the messages. | Optional |
| postType | The content type. Supported values: `FORUM`, `BLOG`, `TKB`, `IDEA`, `CONTEST`, `GROUP`, `OCCASION` | Optional |
| archivedDate | The archived date range. Supported values: `PAST_24_HOURS`, `PAST_WEEK`, `PAST_MONTH`, `PAST_YEAR` | Optional |
| authorId | The author ID of a message | Optional |

### Example

Use the example query below to retrieve all the archived messages of a community.

```graphql
{
  archivedMessages(
    constraints: {
      nodeId: { eq: "community:Docscommunity" }
      archivedBy: { eq: "user:1" }
      postType: { eq: IDEA }
      archivedDate: { eq: PAST_YEAR }
      authorId: { eq: "user:1" }
    }
    sorts: { archivedDate: { direction: ASC } }
    first: 5
  ) {
    totalCount
    edges {
      node {
        message {
          subject
          uid
          conversation {
            style
          }
        }
        archiver {
          uid
        }
      }
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "archivedMessages": {
      "totalCount": 1,
      "edges": [
        {
          "node": {
            "message": {
              "subject": "***Ada**",
              "uid": 3513,
              "conversation": {
                "style": "IDEA"
              }
            },
            "archiver": {
              "uid": 1
            }
          }
        }
      ]
    }
  }
}
```

## Unarchiving a Message

Use the `unarchiveMessage` mutation to unarchive the archived message.

The fields of the mutation are given in the below table:

| Field | Description | Required |
|-------|-------------|----------|
| messageId | The message ID you want to unarchive. | Required |
| destinationBoardId | The destination board ID where you want the message to move after unarchiving. | Required |

### Example

Use the example below to unarchive a message.

```graphql
mutation {
  unarchiveMessage(
    messageId: "message:3376",
    destinationBoardId:"board:ideas"
  ) {
    result
    errors {
      __typename
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "unarchiveMessage": {
      "result": true,
      "errors": null
    }
  }
}
```

---
