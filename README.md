
# STX-NFTNest - Dynamic NFT-Backed Loan Smart Contract

## Overview

This Clarity smart contract enables decentralized loans using **Dynamic NFTs** as collateral. NFTs in this system evolve their attributes based on loan repayment behavior, encouraging responsible financial actions and creating a new form of credit-based NFT evolution.

## Key Features

* **NFT Minting:** Users can mint unique dynamic NFTs with modifiable attributes.
* **Loan Listings:** NFT owners can list their NFTs as collateral for loan requests.
* **Decentralized Lending:** Lenders offer loans by specifying amount, interest, and duration.
* **Collateral Management:** NFTs are transferred to the contract during loan periods and returned or reassigned based on repayment.
* **Attribute Evolution:** NFT attributes like condition and power-level are upgraded/downgraded depending on repayment punctuality.
* **Loan Closure:** Loans are closed by the borrower or automatically after expiration, and NFTs are returned or forfeited accordingly.

---

## 📦 Contract Components

### 📘 Constants

* **CONTRACT\_OWNER**: Initial deployer of the contract.
* **Error Codes**: `ERR_NOT_AUTHORIZED`, `ERR_NFT_NOT_FOUND`, etc.

### 🖼 NFT Definition

* Defines a `dynamic-nft` as a unique token with an ID (`uint`).

### 🗃 Maps

* `token-attributes`: Stores NFT evolution data (e.g., rarity, condition).
* `loan-details`: Stores loan metadata.
* `token-loans`: Links NFTs to active loans.
* `loan-listings`: Records NFTs listed for borrowing.

### 🧮 Variables

* `next-token-id`: Auto-increment ID for NFT minting.
* `next-loan-id`: Auto-increment ID for loans.

---

## 🔍 Read-Only Functions

* `get-token-attributes (token-id)`: Returns the dynamic attributes of an NFT.
* `get-loan-details (loan-id)`: Returns full loan data.
* `get-token-loan (token-id)`: Finds active loan for a token.
* `get-loan-listing (token-id)`: Returns listing details.

---

## 🚀 Public Functions

### ✅ `mint-nft(recipient)`

* Mints a new dynamic NFT to the specified user with initial attributes.

### ✅ `list-nft-for-loan(token-id, requested-amount, min-duration, max-interest)`

* Allows NFT owners to list their tokens as collateral.

### ✅ `offer-loan(token-id, amount, interest-rate, duration)`

* Enables users to fund loans based on listed NFT terms.

### ✅ `close-loan(loan-id)`

* Closes the loan once duration is complete. Returns NFT to borrower if repaid or to lender if defaulted.

---

## 🔧 Internal Helpers

### 🔐 `calculate-payment(loan)`

* Calculates per-block payment amount based on interest and duration.

### 🔐 `update-nft-attributes(loan-id, payment, payment-due)`

* Adjusts NFT attributes based on whether repayment was made on time or missed.

### 🔐 `min-uint(a, b) / max-uint(a, b)`

* Utility functions to ensure attribute values stay in bounds.

---

## 🔁 NFT Evolution Mechanics

* **Timely Repayment:**

  * `condition +5` (capped at 100)
  * `power-level +3` (capped at 100)
* **Missed or Late Repayment:**

  * `condition -10` (min 1)
  * `power-level -7` (min 1)

This mechanism builds a **financial reputation layer** into each NFT.

---

## 🔐 Security & Authorization

* Only the NFT owner can list an NFT.
* Only the lender can initiate loans.
* NFT transfers occur only via the contract for full custody control.
* Contract enforces listing uniqueness and loan limits.

---
