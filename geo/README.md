# 🌐 Administrative Services API (GADM)

A high-performance Node.js/TypeScript API designed to serve hierarchical geographical data. This service utilizes the **GADM (Global Administrative Areas)** dataset to provide a drill-down interface for administrative boundaries.

---

## 📑 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Hierarchy Levels](#-hierarchy-levels)
- [API Endpoints Table](#-api-endpoints-table)
- [Data Schema & Indexing](#-data-schema--indexing)
- [Validation](#-validation)

---

## ✨ Features

- **Hierarchical Drill-Down:** Seamlessly navigate from country level down to local wards/unions.
- **Auto-Level Detection:** The root endpoint calculates the maximum administrative depth available for each country.
- **Optimized Queries:** Uses MongoDB aggregation pipelines with indexed fields for sub-second responses.
- **Type Safety:** Fully implemented in TypeScript with strict interface definitions.

---

## 🛠 Tech Stack

- **Runtime:** Node.js (TypeScript)
- **Framework:** Express.js
- **Database:** MongoDB via Mongoose
- **Validation:** Express-Validator
- **Architecture:** Controller-Service Pattern

---

## 🗂 Hierarchy Levels

The API follows a parent-name filtering logic. To fetch children, you must pass the string name of the immediate parent.

| Level | Description            | MongoDB Property |
| :---- | :--------------------- | :--------------- |
| **0** | Country                | `COUNTRY`        |
| **1** | Division / State       | `NAME_1`         |
| **2** | District / County      | `NAME_2`         |
| **3** | Upazila / Sub-District | `NAME_3`         |
| **4** | Union / Parish         | `NAME_4`         |
| **5** | Sub-Union / Ward       | `NAME_5`         |
| **6** | Local Area             | `NAME_6`         |

---

## 🔗 API Endpoints Table

| Method  | Endpoint          | Required Parameter  | Description                            |
| :------ | :---------------- | :------------------ | :------------------------------------- |
| **GET** | `/name-0`         | None                | Lists all countries & their max depth. |
| **GET** | `/name-1/:name_0` | `name_0` (Country)  | Fetches Divisions for a Country.       |
| **GET** | `/name-2/:name_1` | `name_1` (Division) | Fetches Districts for a Division.      |
| **GET** | `/name-3/:name_2` | `name_2` (District) | Fetches Upazilas for a District.       |
| **GET** | `/name-4/:name_3` | `name_3` (Upazila)  | Fetches Unions for an Upazila.         |
| **GET** | `/name-5/:name_4` | `name_4` (Union)    | Fetches Level 5 areas for a Union.     |
| **GET** | `/name-6/:name_5` | `name_5` (Level 5)  | Fetches Level 6 areas.                 |

---

## 📦 Data Schema & Indexing

The `gadm` collection is optimized for read-heavy operations. The following indexes are applied to ensure high-speed filtering:

```typescript
// Indexes applied in Mongoose for performance
GadmSchema.index({ 'properties.NAME_1': 1 });
GadmSchema.index({ 'properties.NAME_2': 1 });
GadmSchema.index({ 'properties.NAME_3': 1 });
GadmSchema.index({ 'properties.NAME_4': 1 });
```
