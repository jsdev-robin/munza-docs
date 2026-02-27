# Taxonomy API Documentation

## Base URL
```
https://product-taxonomy.devmun.xyz/api/v2/en
```

## Important Note for First-Time Users
⚠️ **This API is hosted on Vercel's free tier. The server may enter a dormant state when not in use. On the first request, please allow 5-10 seconds for the server to wake up. Subsequent requests will be faster.**

## Table of Contents
- [Redux Toolkit Integration](#redux-toolkit-integration)
- [Categories](#categories)
- [Attributes](#attributes)
- [Values](#values)
- [Return Reasons](#return-reasons)
- [Error Handling](#error-handling)
- [Rate Limiting](#rate-limiting)

---

## Redux Toolkit Integration

### API Slice Setup with Retry Logic
```javascript
import { createApi, fetchBaseQuery, retry } from '@reduxjs/toolkit/query/react';

export const taxonomyApi = createApi({
  reducerPath: 'taxonomyApi',
  baseQuery: retry(
    fetchBaseQuery({
      baseUrl: 'https://product-taxonomy.devmun.xyz/api/v2/en',
    }),
    { maxRetries: 5 }, // Automatically retry failed requests 5 times
  ),
  endpoints: (builder) => ({
    getCategories: builder.query({
      query: (ids) => `/category?in=${JSON.stringify(ids)}`,
    }),
    getAttributes: builder.query({
      query: (ids) => `/attribute?in=${JSON.stringify(ids)}`,
    }),
    getValues: builder.query({
      query: (ids) => `/value?in=${JSON.stringify(ids)}`,
    }),
    getReturnReasons: builder.query({
      query: (ids) => `/reasons?in=${JSON.stringify(ids)}`,
    }),
  }),
});

export const {
  useGetCategoriesQuery,
  useGetAttributesQuery,
  useGetValuesQuery,
  useGetReturnReasonsQuery,
} = taxonomyApi;
```

### Usage in React Component
```jsx
import React from 'react';
import { useGetCategoriesQuery } from './services/taxonomyApi';

function CategoryList() {
  // Exact query format: ?in=["aa","ae","ap","bi","bt","bu","co","el","fb","fr","gc","ha","hb","hg","lb","ma","me","na","os","pa","rc","se","sg","so","tg","vp"]
  const categoryIds = ["aa","ae","ap","bi","bt","bu","co","el","fb","fr","gc","ha","hb","hg","lb","ma","me","na","os","pa","rc","se","sg","so","tg","vp"];
  
  const { data, error, isLoading, isError } = useGetCategoriesQuery(categoryIds);

  if (isLoading) return <div>Loading... (first request may take 5-10s)</div>;
  if (isError) return <div>Error: {error.message}</div>;

  return (
    <div>
      <h2>Categories ({data?.total})</h2>
      <ul>
        {data?.payload.categories.map((category) => (
          <li key={category.id}>{category.name} (Level: {category.level})</li>
        ))}
      </ul>
    </div>
  );
}
```

---

## Categories

### Get Categories by IDs
Retrieves categories with their hierarchy levels.

**Endpoint:** `GET /category`

**Query Format:** `?in=["id1","id2","id3"]`

**Query Parameters:**
| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `in` | string | Yes | JSON array of category IDs | `?in=["aa","ae","ap","bi","bt","bu","co","el","fb","fr","gc","ha","hb","hg","lb","ma","me","na","os","pa","rc","se","sg","so","tg","vp"]` |

**Full Request URL:**
```
https://product-taxonomy.devmun.xyz/api/v2/en/category?in=["aa","ae","ap","bi","bt","bu","co","el","fb","fr","gc","ha","hb","hg","lb","ma","me","na","os","pa","rc","se","sg","so","tg","vp"]
```

**Request Example (curl):**
```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/category?in=[\"aa\",\"ae\",\"ap\",\"bi\",\"bt\",\"bu\",\"co\",\"el\",\"fb\",\"fr\",\"gc\",\"ha\",\"hb\",\"hg\",\"lb\",\"ma\",\"me\",\"na\",\"os\",\"pa\",\"rc\",\"se\",\"sg\",\"so\",\"tg\",\"vp\"]"
```

**RTK Query Example:**
```javascript
const { data, isLoading } = useGetCategoriesQuery([
  "aa","ae","ap","bi","bt","bu","co","el","fb","fr",
  "gc","ha","hb","hg","lb","ma","me","na","os","pa",
  "rc","se","sg","so","tg","vp"
]);

// This makes request to: /category?in=["aa","ae","ap","bi","bt","bu","co","el","fb","fr","gc","ha","hb","hg","lb","ma","me","na","os","pa","rc","se","sg","so","tg","vp"]
```

**Success Response (200 OK):**
```json
{
  "status": "success",
  "message": "Categories fetched successfully",
  "total": 26,
  "payload": {
    "categories": [
      {
        "_id": "67b5b8b8b8b8b8b8b8b8b8b8",
        "id": "aa",
        "name": "Electronics",
        "children": ["aa-1", "aa-2"],
        "attributes": ["color", "size"],
        "return_reasons": ["damaged", "wrong-item"],
        "level": 1,
        "isRoot": true,
        "isLeaf": false,
        "full_name": "Electronics",
        "createdAt": "2024-01-01T00:00:00.000Z",
        "updatedAt": "2024-01-01T00:00:00.000Z"
      }
    ]
  }
}
```

---

## Attributes

### Get Attributes by IDs
Retrieves attributes by their friendly IDs.

**Endpoint:** `GET /attribute`

**Query Format:** `?in=["id1","id2","id3"]`

**Query Parameters:**
| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `in` | string | Yes | JSON array of attribute friendly_ids | `?in=["color","size","material"]` |

**Full Request URL:**
```
https://product-taxonomy.devmun.xyz/api/v2/en/attribute?in=["color","size","material"]
```

**Request Example (curl):**
```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/attribute?in=[\"color\",\"size\",\"material\"]"
```

**RTK Query Example:**
```javascript
const { data, isLoading } = useGetAttributesQuery(["color", "size", "material"]);
// Makes request to: /attribute?in=["color","size","material"]
```

**Success Response (200 OK):**
```json
{
  "status": "success",
  "message": "Attributes fetched successfully",
  "total": 3,
  "payload": {
    "attributes": [
      {
        "_id": "67b5b8b8b8b8b8b8b8b8b8c0",
        "id": 1,
        "name": "Color",
        "description": "Product color options",
        "friendly_id": "color",
        "handle": "color",
        "values": ["red", "blue", "green", "black"],
        "createdAt": "2024-01-01T00:00:00.000Z",
        "updatedAt": "2024-01-01T00:00:00.000Z"
      }
    ]
  }
}
```

---

## Values

### Get Values by IDs
Retrieves values by their friendly IDs.

**Endpoint:** `GET /value`

**Query Format:** `?in=["id1","id2","id3"]`

**Query Parameters:**
| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `in` | string | Yes | JSON array of value friendly_ids | `?in=["red","blue","large"]` |

**Full Request URL:**
```
https://product-taxonomy.devmun.xyz/api/v2/en/value?in=["red","blue","large"]
```

**Request Example (curl):**
```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/value?in=[\"red\",\"blue\",\"large\"]"
```

**RTK Query Example:**
```javascript
const { data, isLoading } = useGetValuesQuery(["red", "blue", "large"]);
// Makes request to: /value?in=["red","blue","large"]
```

**Success Response (200 OK):**
```json
{
  "status": "success",
  "message": "Values fetched successfully",
  "total": 3,
  "payload": {
    "values": [
      {
        "_id": "67b5b8b8b8b8b8b8b8b8b8d0",
        "id": 101,
        "name": "Red",
        "friendly_id": "red",
        "handle": "red",
        "createdAt": "2024-01-01T00:00:00.000Z",
        "updatedAt": "2024-01-01T00:00:00.000Z"
      }
    ]
  }
}
```

---

## Return Reasons

### Get Return Reasons by IDs
Retrieves return reasons by their friendly IDs.

**Endpoint:** `GET /reasons`

**Query Format:** `?in=["id1","id2","id3"]`

**Query Parameters:**
| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `in` | string | Yes | JSON array of return reason friendly_ids | `?in=["damaged","wrong-item"]` |

**Full Request URL:**
```
https://product-taxonomy.devmun.xyz/api/v2/en/reasons?in=["damaged","wrong-item"]
```

**Request Example (curl):**
```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/reasons?in=[\"damaged\",\"wrong-item\"]"
```

**RTK Query Example:**
```javascript
const { data, isLoading } = useGetReturnReasonsQuery(["damaged", "wrong-item"]);
// Makes request to: /reasons?in=["damaged","wrong-item"]
```

**Success Response (200 OK):**
```json
{
  "status": "success",
  "message": "Values fetched successfully",
  "total": 2,
  "payload": {
    "values": [
      {
        "_id": "67b5b8b8b8b8b8b8b8b8b8e0",
        "id": 1,
        "name": "Damaged",
        "description": "Product arrived damaged",
        "friendly_id": "damaged",
        "handle": "damaged",
        "createdAt": "2024-01-01T00:00:00.000Z",
        "updatedAt": "2024-01-01T00:00:00.000Z"
      }
    ]
  }
}
```

---

## Error Handling

### Error Response Format
```json
{
  "status": "error",
  "message": "Error description here"
}
```

### Common Error Codes
| Status Code | Description | Example |
|------------|-------------|---------|
| **400 Bad Request** | Invalid query parameters | `{"status": "error", "message": "Invalid JSON in 'in' parameter"}` |
| **404 Not Found** | Resource not found | `{"status": "error", "message": "Can't find /api/v2/en/invalid on this server!"}` |
| **429 Too Many Requests** | Rate limit exceeded | `{"status": "error", "message": "Too many requests, please try again later."}` |
| **500 Internal Server Error** | Server error | `{"status": "error", "message": "YAML file not found"}` |
| **503/504 Service Unavailable** | Server wake-up delay | `{"status": "error", "message": "Server is waking up, please retry"}` |

---

## Rate Limiting

| Limit | Value |
|-------|-------|
| **Window** | 15 minutes |
| **Max Requests** | 100 per window |
| **Status Code on Limit** | 429 Too Many Requests |

---

## Quick Reference

| Resource | Method | Endpoint Format | RTK Hook |
|----------|--------|-----------------|----------|
| Categories | GET | `/category?in=["id1","id2"]` | `useGetCategoriesQuery()` |
| Attributes | GET | `/attribute?in=["id1","id2"]` | `useGetAttributesQuery()` |
| Values | GET | `/value?in=["id1","id2"]` | `useGetValuesQuery()` |
| Return Reasons | GET | `/reasons?in=["id1","id2"]` | `useGetReturnReasonsQuery()` |

---

## Server Information

| Detail | Information |
|--------|-------------|
| **Hosting** | Vercel (Free Tier) |
| **First Request** | 5-10 seconds wake-up time |
| **Retry Logic** | 5 automatic retries (RTK Query) |
| **Dormancy Period** | Server sleeps after inactivity |

---

**API Version:** 2.0.0  
**Base URL:** https://product-taxonomy.devmun.xyz/api/v2/en  
**Query Format:** `?in=["id1","id2","id3"]`  
**RTK Query Retries:** 5 times  
**Last Updated:** February 2026

---

## ⚡ Quick Start Guide

1. **Use exact query format**: `?in=["aa","ae","ap"]`
2. **Setup RTK Query with retry** (5 retries automatically)
3. **First API Call**: Allow 5-10 seconds for server wake-up
4. **RTK Query handles retries automatically**

```javascript
// Complete setup with exact query format
import { createApi, fetchBaseQuery, retry } from '@reduxjs/toolkit/query/react';

export const taxonomyApi = createApi({
  reducerPath: 'taxonomyApi',
  baseQuery: retry(
    fetchBaseQuery({
      baseUrl: 'https://product-taxonomy.devmun.xyz/api/v2/en',
    }),
    { maxRetries: 5 },
  ),
  endpoints: (builder) => ({
    getCategories: builder.query({
      query: (ids) => `/category?in=${JSON.stringify(ids)}`, // Creates: /category?in=["aa","ae","ap"]
    }),
  }),
});

// Usage
const categoryIds = ["aa","ae","ap","bi","bt","bu","co","el","fb","fr","gc","ha","hb","hg","lb","ma","me","na","os","pa","rc","se","sg","so","tg","vp"];
const { data } = useGetCategoriesQuery(categoryIds);
```
```