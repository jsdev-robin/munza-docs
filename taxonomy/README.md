# Taxonomy API Documentation

## Base URL

```
https://product-taxonomy.devmun.xyz/api/v2/en
```

## Table of Contents

- [Categories](#categories)
- [Attributes](#attributes)
- [Values](#values)
- [Return Reasons](#return-reasons)
- [Error Handling](#error-handling)
- [Rate Limiting](#rate-limiting)

---

## Categories

### Get Categories by IDs

Retrieves categories with their hierarchy levels. The level is automatically calculated based on the ID structure (number of hyphens).

**Endpoint:** `GET /category`

**Query Parameters:**
| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `in` | string | Yes | JSON array of category IDs | `["cat-1", "cat-1-2", "cat-2-1-3"]` |

**Request Example:**

```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/category?in=%5B%22cat-1%22,%22cat-1-2%22%5D"
```

**Success Response (200 OK):**

```json
{
  "status": "success",
  "message": "Categories fetched successfully",
  "total": 2,
  "payload": {
    "categories": [
      {
        "_id": "67b5b8b8b8b8b8b8b8b8b8b8",
        "id": "cat-1",
        "name": "Electronics",
        "children": ["cat-1-1", "cat-1-2"],
        "attributes": ["color", "size"],
        "return_reasons": ["damaged", "wrong-item"],
        "level": 1,
        "createdAt": "2024-01-01T00:00:00.000Z",
        "updatedAt": "2024-01-01T00:00:00.000Z"
      },
      {
        "_id": "67b5b8b8b8b8b8b8b8b8b8b9",
        "id": "cat-1-2",
        "name": "Smartphones",
        "children": [],
        "attributes": ["screen-size", "ram"],
        "return_reasons": ["defective", "not-as-described"],
        "level": 2,
        "createdAt": "2024-01-01T00:00:00.000Z",
        "updatedAt": "2024-01-01T00:00:00.000Z"
      }
    ]
  }
}
```

### Bulk Create Categories

Upload multiple categories from YAML files.

**Endpoint:** `POST /category/create-many`

**Request Example:**

```bash
curl -X POST "https://product-taxonomy.devmun.xyz/api/v2/en/category/create-many"
```

**Success Response (201 Created):**

```json
{
  "status": "success",
  "message": "25 categories uploaded successfully"
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
| `in` | string | Yes | JSON array of attribute friendly_ids | `["color", "size", "material"]` |

**Request Example:**

```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/attribute?in=%5B%22color%22,%22size%22%5D"
```

**Success Response (200 OK):**

```json
{
  "status": "success",
  "message": "Attributes fetched successfully",
  "total": 2,
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
      },
      {
        "_id": "67b5b8b8b8b8b8b8b8b8b8c1",
        "id": 2,
        "name": "Size",
        "description": "Product size options",
        "friendly_id": "size",
        "handle": "size",
        "values": ["xs", "s", "m", "l", "xl"],
        "createdAt": "2024-01-01T00:00:00.000Z",
        "updatedAt": "2024-01-01T00:00:00.000Z"
      }
    ]
  }
}
```

### Bulk Create Attributes

Upload multiple attributes from YAML file.

**Endpoint:** `POST /attribute/create-many`

**Request Example:**

```bash
curl -X POST "https://product-taxonomy.devmun.xyz/api/v2/en/attribute/create-many"
```

**Success Response (201 Created):**

```json
{
  "status": "success",
  "message": "15 attributes uploaded successfully"
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
| `in` | string | Yes | JSON array of value friendly_ids | `["red", "blue", "large"]` |

**Request Example:**

```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/value?in=%5B%22red%22,%22blue%22,%22large%22%5D"
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
      },
      {
        "_id": "67b5b8b8b8b8b8b8b8b8b8d1",
        "id": 102,
        "name": "Blue",
        "friendly_id": "blue",
        "handle": "blue",
        "createdAt": "2024-01-01T00:00:00.000Z",
        "updatedAt": "2024-01-01T00:00:00.000Z"
      },
      {
        "_id": "67b5b8b8b8b8b8b8b8b8b8d2",
        "id": 201,
        "name": "Large",
        "friendly_id": "large",
        "handle": "large",
        "createdAt": "2024-01-01T00:00:00.000Z",
        "updatedAt": "2024-01-01T00:00:00.000Z"
      }
    ]
  }
}
```

### Bulk Create Values

Upload multiple values from YAML file.

**Endpoint:** `POST /value/create-many`

**Request Example:**

```bash
curl -X POST "https://product-taxonomy.devmun.xyz/api/v2/en/value/create-many"
```

**Success Response (201 Created):**

```json
{
  "status": "success",
  "message": "50 values uploaded successfully"
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
| `in` | string | Yes | JSON array of return reason friendly_ids | `["damaged", "wrong-item", "not-as-described"]` |

**Request Example:**

```bash
curl -X GET "https://product-taxonomy.devmun.xyz/api/v2/en/reasons?in=%5B%22damaged%22,%22wrong-item%22%5D"
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
      },
      {
        "_id": "67b5b8b8b8b8b8b8b8b8b8e1",
        "id": 2,
        "name": "Wrong Item",
        "description": "Received incorrect product",
        "friendly_id": "wrong-item",
        "handle": "wrong-item",
        "createdAt": "2024-01-01T00:00:00.000Z",
        "updatedAt": "2024-01-01T00:00:00.000Z"
      }
    ]
  }
}
```

### Bulk Create Return Reasons

Upload multiple return reasons from YAML file.

**Endpoint:** `POST /reasons/create-many`

**Request Example:**

```bash
curl -X POST "https://product-taxonomy.devmun.xyz/api/v2/en/reasons/create-many"
```

**Success Response (201 Created):**

```json
{
  "status": "success",
  "message": "8 return_reasons uploaded successfully"
}
```

---

## Error Handling

### Error Response Format

All errors follow this format:

```json
{
  "status": "error",
  "message": "Error description here"
}
```

### Common Error Codes

| Status Code                   | Description              | Example                                                                           |
| ----------------------------- | ------------------------ | --------------------------------------------------------------------------------- |
| **400 Bad Request**           | Invalid query parameters | `{"status": "error", "message": "Invalid JSON in 'in' parameter"}`                |
| **404 Not Found**             | Resource not found       | `{"status": "error", "message": "Can't find /api/v2/en/invalid on this server!"}` |
| **429 Too Many Requests**     | Rate limit exceeded      | `{"status": "error", "message": "Too many requests, please try again later."}`    |
| **500 Internal Server Error** | Server error             | `{"status": "error", "message": "YAML file not found"}`                           |

### 404 Example

```json
{
  "status": "error",
  "message": "Can't find /api/v2/en/nonexistent on this server!"
}
```

### Rate Limit Example

```json
{
  "status": "error",
  "message": "Too many requests, please try again later."
}
```

### YAML File Error Example

```json
{
  "status": "error",
  "message": "No valid base_attributes found in YAML file"
}
```

---

## Rate Limiting

The API implements rate limiting to prevent abuse.

| Limit                    | Value                    |
| ------------------------ | ------------------------ |
| **Window**               | 15 minutes               |
| **Max Requests**         | 100 per window           |
| **Headers**              | Draft-7 standard headers |
| **Status Code on Limit** | 429 Too Many Requests    |

When rate limit is exceeded, the API returns a 429 status code with an error message.

---

## Data Models

### Category Object

| Field            | Type   | Description                           |
| ---------------- | ------ | ------------------------------------- |
| `_id`            | string | MongoDB ObjectId                      |
| `id`             | string | Category identifier (e.g., "cat-1-2") |
| `name`           | string | Category name                         |
| `children`       | array  | Array of child category IDs           |
| `attributes`     | array  | Array of attribute friendly_ids       |
| `return_reasons` | array  | Array of return reason friendly_ids   |
| `level`          | number | Auto-calculated hierarchy level       |
| `createdAt`      | string | ISO timestamp                         |
| `updatedAt`      | string | ISO timestamp                         |

### Attribute Object

| Field         | Type   | Description                 |
| ------------- | ------ | --------------------------- |
| `_id`         | string | MongoDB ObjectId            |
| `id`          | number | Numeric identifier          |
| `name`        | string | Attribute name              |
| `description` | string | Attribute description       |
| `friendly_id` | string | URL-friendly identifier     |
| `handle`      | string | Unique handle               |
| `values`      | array  | Array of value friendly_ids |
| `createdAt`   | string | ISO timestamp               |
| `updatedAt`   | string | ISO timestamp               |

### Value Object

| Field         | Type   | Description             |
| ------------- | ------ | ----------------------- |
| `_id`         | string | MongoDB ObjectId        |
| `id`          | number | Numeric identifier      |
| `name`        | string | Value name              |
| `friendly_id` | string | URL-friendly identifier |
| `handle`      | string | Unique handle           |
| `createdAt`   | string | ISO timestamp           |
| `updatedAt`   | string | ISO timestamp           |

### Return Reason Object

| Field         | Type   | Description             |
| ------------- | ------ | ----------------------- |
| `_id`         | string | MongoDB ObjectId        |
| `id`          | number | Numeric identifier      |
| `name`        | string | Reason name             |
| `description` | string | Reason description      |
| `friendly_id` | string | URL-friendly identifier |
| `handle`      | string | Unique handle           |
| `createdAt`   | string | ISO timestamp           |
| `updatedAt`   | string | ISO timestamp           |

---

## Quick Reference

| Resource       | Method | Endpoint                 | Description                        |
| -------------- | ------ | ------------------------ | ---------------------------------- |
| Categories     | GET    | `/category?in=[]`        | Get categories by IDs              |
| Categories     | POST   | `/category/create-many`  | Bulk upload categories             |
| Attributes     | GET    | `/attribute?in=[]`       | Get attributes by friendly_ids     |
| Attributes     | POST   | `/attribute/create-many` | Bulk upload attributes             |
| Values         | GET    | `/value?in=[]`           | Get values by friendly_ids         |
| Values         | POST   | `/value/create-many`     | Bulk upload values                 |
| Return Reasons | GET    | `/reasons?in=[]`         | Get return reasons by friendly_ids |
| Return Reasons | POST   | `/reasons/create-many`   | Bulk upload return reasons         |

---

**API Version:** 1.0.0  
**Last Updated:** February 2026

```

```
