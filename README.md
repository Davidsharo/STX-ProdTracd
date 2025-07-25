
# STX-ProdTrace - SupplyChainTrackr Smart Contract

A Clarity-based smart contract for managing roles and tracking products through a decentralized supply chain using the Stacks blockchain. It ensures transparency and immutability in product handling from manufacturing to retail, with detailed location history.

---

## 📖 Table of Contents

* [Features](#features)
* [Smart Contract Structure](#smart-contract-structure)
* [Roles](#roles)
* [Product Lifecycle](#product-lifecycle)
* [Functions](#functions)

  * [Public Functions](#public-functions)
  * [Private/Internal Functions](#privateinternal-functions)
  * [Read-only Functions](#read-only-functions)
* [Error Codes](#error-codes)
* [Usage Flow](#usage-flow)
* [Security Notes](#security-notes)
* [Suggested Names](#suggested-names)

---

## ✅ Features

* Role-based access control (manufacturer, transporter, retailer).
* Track products through origin, location, and status.
* Maintain an immutable history of product movement.
* Validate string inputs to prevent malformed data.
* Centralized ownership control for administrative actions.

---

## 🏗 Smart Contract Structure

This contract is organized into:

* **Constants**: Predefined values for roles, error codes, and string limits.
* **Data Variables**: Track owner, product counter, and change counter.
* **Maps**: For storing roles, products, and product history.
* **Functions**: Role management, product addition, location updates, and history retrieval.

---

## 👥 Roles

Each `principal` (user) can be assigned a role:

* **manufacturer**: Can add new products.
* **transporter**: Can update product location.
* **retailer**: (Future implementation possible).
* Roles can be **assigned** or **revoked** only by the contract owner.

---

## 📦 Product Lifecycle

1. **Created** by the manufacturer.
2. **Location Updated** by transporter/manufacturer.
3. Each change is stored in an immutable **product-history** log.

---

## 🔧 Functions

### ✅ Public Functions

#### `assign-role (address, new-role)`

Assign a role to a user. Only callable by contract owner.

#### `revoke-role (address)`

Revoke a user's role. Only callable by contract owner.

#### `add-product (name, origin, location)`

Add a new product. Only callable by a manufacturer.

#### `update-location (product-id, new-location)`

Update the current location of a product. Callable by manufacturer or transporter. Also logs the update in product-history.

---

### 🔐 Private/Internal Functions

* `is-valid-role(role)`: Checks if the role is valid.
* `is-valid-string-length(str, max-len)`: Validates string length.
* `validate-strings(name, origin, location)`: Checks all string fields.
* `is-contract-owner()`: Verifies contract ownership.
* `check-role(address, role)`: Confirms user role and activity.
* `safe-get-role(address)`: Returns user role or default.
* `safe-get-product(product-id)`: Returns product if exists.

---

### 🔍 Read-only Functions

#### `get-product(product-id)`

Returns full product metadata.

#### `get-role(address)`

Returns assigned role and activity status.

#### `get-product-history(product-id)`

Returns the earliest history entry (`change-id = 0`) for a given product.

> *Note: Could be extended to return a full list of history entries.*

---

## ⚠ Error Codes

| Constant             | Code       | Meaning                        |
| -------------------- | ---------- | ------------------------------ |
| `ERR-NOT-AUTHORIZED` | `err u100` | Unauthorized action            |
| `ERR-INVALID-ROLE`   | `err u101` | Invalid role provided          |
| `ERR-NOT-FOUND`      | `err u102` | Product or role not found      |
| `ERR-INVALID-INPUT`  | `err u103` | String length or input invalid |
| `ERR-ALREADY-EXISTS` | `err u104` | Role or product already exists |

---

## 🔄 Usage Flow

1. **Admin** (contract owner) calls `assign-role` to authorize users.
2. **Manufacturer** calls `add-product` to register a product.
3. **Transporter/Manufacturer** calls `update-location` as the product moves.
4. Anyone can call `get-product` or `get-product-history` to view product data.

---
