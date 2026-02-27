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
      query: (ids) => ({
        url: '/category',
        params: { in: ids }
      }),
    }),
    getAttributes: builder.query({
      query: (ids) => ({
        url: '/attribute',
        params: { in: ids }
      }),
    }),
    getValues: builder.query({
      query: (ids) => ({
        url: '/value',
        params: { in: ids }
      }),
    }),
    getReturnReasons: builder.query({
      query: (ids) => ({
        url: '/reasons',
        params: { in: ids }
      }),
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

**Query Parameters:**
| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `in` | array | Yes | Array of category IDs | `["aa","ae","ap"]` |

**Working curl commands:**

**Option 1:** Using double quotes (Windows CMD)
```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/category?in=[\"aa\",\"ae\",\"ap\",\"bi\",\"bt\",\"bu\",\"co\",\"el\",\"fb\",\"fr\",\"gc\",\"ha\",\"hb\",\"hg\",\"lb\",\"ma\",\"me\",\"na\",\"os\",\"pa\",\"rc\",\"se\",\"sg\",\"so\",\"tg\",\"vp\"]"
```

**Option 2:** Using single quotes (Git Bash / Linux / Mac)
```bash
curl -X GET 'https://product-taxonomy.devmun.xyz/api/v2/en/category?in=["aa","ae","ap","bi","bt","bu","co","el","fb","fr","gc","ha","hb","hg","lb","ma","me","na","os","pa","rc","se","sg","so","tg","vp"]'
```

**Option 3:** URL encoded (Works in all terminals)
```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/category?in=%5B%22aa%22,%22ae%22,%22ap%22,%22bi%22,%22bt%22,%22bu%22,%22co%22,%22el%22,%22fb%22,%22fr%22,%22gc%22,%22ha%22,%22hb%22,%22hg%22,%22lb%22,%22ma%22,%22me%22,%22na%22,%22os%22,%22pa%22,%22rc%22,%22se%22,%22sg%22,%22so%22,%22tg%22,%22vp%22%5D"
```

**RTK Query Example:**
```javascript
const { data, isLoading } = useGetCategoriesQuery([
  "aa","ae","ap","bi","bt","bu","co","el","fb","fr",
  "gc","ha","hb","hg","lb","ma","me","na","os","pa",
  "rc","se","sg","so","tg","vp"
]);
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

**Query Parameters:**
| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `in` | array | Yes | Array of attribute friendly_ids | `["color","size","material"]` |

**Working curl commands:**

**Option 1:** Using double quotes (Windows CMD)
```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/attribute?in=[\"color\",\"size\",\"material\"]"
```

**Option 2:** Using single quotes (Git Bash / Linux / Mac)
```bash
curl -X GET 'https://product-taxonomy.devmun.xyz/api/v2/en/attribute?in=["color","size","material"]'
```

**Option 3:** URL encoded
```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/attribute?in=%5B%22color%22,%22size%22,%22material%22%5D"
```

**RTK Query Example:**
```javascript
const { data, isLoading } = useGetAttributesQuery(["color", "size", "material"]);
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

**Query Parameters:**
| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `in` | array | Yes | Array of value friendly_ids | `["red","blue","large"]` |

**Working curl commands:**

**Option 1:** Using double quotes (Windows CMD)
```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/value?in=[\"red\",\"blue\",\"large\"]"
```

**Option 2:** Using single quotes (Git Bash / Linux / Mac)
```bash
curl -X GET 'https://product-taxonomy.devmun.xyz/api/v2/en/value?in=["red","blue","large"]'
```

**Option 3:** URL encoded
```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/value?in=%5B%22red%22,%22blue%22,%22large%22%5D"
```

**RTK Query Example:**
```javascript
const { data, isLoading } = useGetValuesQuery(["red", "blue", "large"]);
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

**Query Parameters:**
| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `in` | array | Yes | Array of return reason friendly_ids | `["damaged","wrong-item"]` |

**Working curl commands:**

**Option 1:** Using double quotes (Windows CMD)
```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/reasons?in=[\"damaged\",\"wrong-item\"]"
```

**Option 2:** Using single quotes (Git Bash / Linux / Mac)
```bash
curl -X GET 'https://product-taxonomy.devmun.xyz/api/v2/en/reasons?in=["damaged","wrong-item"]'
```

**Option 3:** URL encoded
```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/reasons?in=%5B%22damaged%22,%22wrong-item%22%5D"
```

**RTK Query Example:**
```javascript
const { data, isLoading } = useGetReturnReasonsQuery(["damaged", "wrong-item"]);
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

| Resource | Method | Endpoint | RTK Hook |
|----------|--------|----------|----------|
| Categories | GET | `/category?in=[]` | `useGetCategoriesQuery()` |
| Attributes | GET | `/attribute?in=[]` | `useGetAttributesQuery()` |
| Values | GET | `/value?in=[]` | `useGetValuesQuery()` |
| Return Reasons | GET | `/reasons?in=[]` | `useGetReturnReasonsQuery()` |

---

## Server Information

| Detail | Information |
|--------|-------------|
| **Hosting** | Vercel (Free Tier) |
| **First Request** | 5-10 seconds wake-up time |
| **Retry Logic** | 5 automatic retries (RTK Query) |
| **Dormancy Period** | Server sleeps after inactivity |

---

## Terminal Compatibility

| Terminal | Recommended curl command |
|----------|-------------------------|
| **Windows CMD** | Use double quotes with escape: `\"id\"` |
| **Git Bash** | Use single quotes: `'["id"]'` |
| **PowerShell** | Use URL encoded version |
| **Linux/Mac** | Use single quotes: `'["id"]'` |

---

**API Version:** 2.0.0  
**Base URL:** https://product-taxonomy.devmun.xyz/api/v2/en  
**RTK Query Retries:** 5 times  
**Last Updated:** February 2026

---

## ⚡ Quick Start Guide

1. **Choose your terminal type** and use the appropriate curl command
2. **Setup RTK Query with retry** (5 retries automatically)
3. **First API Call**: Allow 5-10 seconds for server wake-up
4. **RTK Query handles retries automatically**

```javascript
// Complete setup with RTK Query
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
      query: (ids) => ({
        url: '/category',
        params: { in: ids }
      }),
    }),
  }),
});

// Usage
const categoryIds = ["aa","ae","ap","bi","bt","bu","co","el","fb","fr","gc","ha","hb","hg","lb","ma","me","na","os","pa","rc","se","sg","so","tg","vp"];
const { data } = useGetCategoriesQuery(categoryIds);
```
```