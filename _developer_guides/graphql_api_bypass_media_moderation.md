---
layout: guide
title: "Working with Bypass Media Moderation"
description: "Technical documentation for implementing bypass mechanisms in GraphQL-based media moderation systems, including security considerations and implementation patterns."
author: "Muhamed Javid Hussain"
date: 2024-10-20
tags: ["GraphQL", "Media Moderation", "Security", "API Documentation"]
category: "developer_guides"
---
# Working with Bypass Media Moderation

## Table of Contents

1. [About Bypass Media Moderation](#about-bypass-media-moderation)
2. [Query to Fetch the Permission](#query-to-fetch-the-permission)
3. [Set the Bypass Media Moderation Permission](#set-the-bypass-media-moderation-permission)
   - [Setting Community-Level Permissions](#setting-community-level-permissions)
   - [Setting Role-Based Permissions](#setting-role-based-permissions)

---

## About Bypass Media Moderation

The bypass media moderation permission empowers members to share visual and multimedia content, including images, videos, and attachments, instantaneously. This feature circumvents the potential delays that would typically arise from a moderation review process, providing an efficient member experience where timely content sharing is essential.

You can set this permission at the following levels:

- Community
- Category
- Forum
- Blog
- KB
- Occasion (Event)
- Group
- Idea

> **Note:** Only Admins and Moderators can set this permission for your community.

## Query to Fetch the Permission

You can use the below query to view your community bypass media moderation permission.

```graphql
{
  permissionsForCoreNode(id: "community:DocsCommunity") {
    ... on CommunityPermissions {
      bypassMediaModeration {
        access
      }
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "permissionsForCoreNode": {
      "bypassMediaModeration": {
        "access": "DENIED"
      }
    }
  }
}
```

## Set the Bypass Media Moderation Permission

> **Note:** Only Admins and Moderators can set this permission for your community.

### Setting Community-Level Permissions

Use the `setCommunityPermissions` mutation to set the bypass media moderation permission for your community.

```graphql
mutation {
  setCommunityPermissions(updateInput: {
    bypassMediaModeration: {
      access: GRANTED
    }
  }) {
    result {
      bypassMediaModeration {
        access
      }
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "setCommunityPermissions": {
      "result": {
        "bypassMediaModeration": {
          "access": "GRANTED"
        }
      }
    }
  }
}
```

### Setting Role-Based Permissions

Use the `setCommunityPermissionsForRole` mutation to set bypass media moderation permission for a specific role at the community level.

```graphql
mutation {
  setCommunityPermissionsForRole(
    roleName: "BlogAuthor",
    updateInput: {
      bypassMediaModeration: {
        access: GRANTED
      }
    }
  ) {
    result {
      bypassMediaModeration {
        access
      }
    }
  }
}
```

**GraphQL Response:**

```json
{
  "data": {
    "setCommunityPermissionsForRole": {
      "result": {
        "bypassMediaModeration": {
          "access": "GRANTED"
        }
      }
    }
  }
}
```

