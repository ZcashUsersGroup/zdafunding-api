# ZDA Funding Wallet API

Public-facing API for ZDA funding wallet project cards and exchange rate data.

## 🌐 Base URL
```
https://zdafunding-api.onrender.com/api/v1

```

---

## 🚀 Endpoints

### `GET /exchange-rate`
Returns the current (mocked) ZEC → USD exchange rate.
✅ Example:
```
https://zdafunding-api.onrender.com/api/v1/exchange-rate

````
✅ Response:
```json
{
  "zec_to_usd": 72.55,
  "timestamp": "2025-06-22T20:22:37.568Z"
}
````

---

### `GET /cards`

```
https://zdafunding-api.onrender.com/api/v1/cards
```

Returns a list of project cards.

| Name                | Type          | Usage           | Allowed Values / Notes                                                                       |
| ------------------- | ------------- | --------------- | -------------------------------------------------------------------------------------------- |
| `page`              | integer       | Query parameter | Page number (default `1`)                                                                    |
| `per_page`          | integer       | Query parameter | Cards per page (default `10`, max `100`)                                                     |
| `sort_by`           | string        | Query parameter | `last_updated`, `priority`, `percent_funded`, `date` (default `last_updated`)                |
| `sort_dir`          | string        | Query parameter | `asc`, `desc` (default `desc`)                                                               |
| `priority`          | string        | Query parameter | `HIGH`, `MEDIUM`, `LOW`                                                                      |
| `status`            | string        | Query parameter | `COMPLETE`, `IN PROGRESS`, `BLOCKED`                                                         |
| `stage`             | string        | Query parameter | `NOT STARTED`, `REQUIREMENTS ANALYSIS`, `DESIGN`, `DEVELOPMENT`, `DEPLOYMENT`, `MAINTENANCE` |
| `tags`              | string        | Query parameter | Comma-separated tags (e.g. `privacy,communication`)                                          |
| `id`                | uuid          | Response field  | Unique ID of the card                                                                        |
| `title`             | text          | Response field  | Project title                                                                                |
| `description`       | text          | Response field  | Project description                                                                          |
| `creators`          | text\[]       | Response field  | List of creators                                                                             |
| `date`              | timestamptz   | Response field  | Project creation date                                                                        |
| `contributors`      | integer       | Response field  | Number of contributors                                                                       |
| `tags`              | text\[]       | Response field  | Tags assigned to the project                                                                 |
| `priority`          | text          | Response field  | Priority level                                                                               |
| `funding_earned`    | numeric(20,8) | Response field  | ZEC earned                                                                                   |
| `funding_spent`     | numeric(20,8) | Response field  | ZEC spent                                                                                    |
| `funding_requested` | numeric(20,8) | Response field  | ZEC requested                                                                                |
| `funding_received`  | numeric(20,8) | Response field  | ZEC received                                                                                 |
| `funding_available` | numeric(20,8) | Response field  | ZEC available                                                                                |
| `percent_funded`    | numeric       | Response field  | Percent funded                                                                               |
| `visibility`        | text          | Response field  | Always `PUBLIC`                                                                              |
| `milestones`        | jsonb         | Response field  | Milestones data                                                                              |
| `status`            | text          | Response field  | Project status                                                                               |
| `stage`             | text          | Response field  | Project stage                                                                                |
| `created_by`        | text          | Response field  | Creator ID                                                                                   |
| `owned_by`          | text          | Response field  | Owner ID                                                                                     |
| `last_updated`      | timestamptz   | Response field  | Last update timestamp                                                                        |
| `wallet_addresses`  | text\[]       | Response field  | Transparent addresses linked to project                                                      |
| `view_keys`         | text\[]       | Response field  | View keys linked to project                                                                  |


---

✅ Example:

---

#### 1️⃣ All public cards (default, no query params needed)

```
https://zdafunding-api.onrender.com/api/v1/cards
```

---

#### 2️⃣ Limit 10 (default behavior is per\_page=10)

```
https://zdafunding-api.onrender.com/api/v1/cards?per_page=10
```

---

#### 3️⃣ Pagination with offset (page 2, per\_page 10 → offset 10)

```
https://zdafunding-api.onrender.com/api/v1/cards?page=2&per_page=10
```

---

#### 4️⃣ Order by last\_updated descending (default)

```
https://zdafunding-api.onrender.com/api/v1/cards?sort_by=last_updated&sort_dir=desc
```

---

#### 5️⃣ Filter by priority HIGH

```
https://zdafunding-api.onrender.com/api/v1/cards?priority=HIGH
```

---

#### 6️⃣ Filter by priority HIGH and status IN PROGRESS

```
https://zdafunding-api.onrender.com/api/v1/cards?priority=HIGH&status=IN%20PROGRESS
```

---

### `GET /cards/{id}`

Returns a single project card by ID (UUID).
✅ Example:

```
https://zdafunding-api.onrender.com/api/v1/cards/4c999973-8929-4f51-8964-21f2677935e4
```

---

## 🛡 Rate limit

10 requests per minute per IP. Exceeding this returns:

```json
{
  "message": "Too many requests, please try again later."
}
```

---

## ⚙ Response headers

* `X-API-Version: v1`
* `Cache-Control: public, max-age=30`

---

## 📂 Code repository

[https://github.com/explore-eda/zdafunding-api](https://github.com/explore-eda/zdafunding-api)

---

## 🔑 CORS

All origins allowed.

---

## ❗ Error codes

* `400` — bad query
* `404` — resource not found
* `429` — too many requests
* `500` — internal server error

---

## 🚀 Next Steps Plan
 
🚧 "Write Jest + Supertest tests for the API endpoints and set up a GitHub Actions workflow to run them on each push."


### 1️⃣ Live exchange rate

✅ Replace the hardcoded zec_to_usd with real data.

👉 Plan:

Use a reliable API (e.g., CoinGecko or CoinMarketCap)

In your /exchange-rate route, fetch the ZEC/USD price at request time or cache it briefly

### 2️⃣ Real wallet balances

✅ Instead of storing static funding numbers, query the actual wallet addresses linked to grants.

👉 Plan:

Identify and store associated ZEC wallet addresses in your cards table

Use a Zcash block explorer API 

On API calls or via scheduled jobs, fetch current balances + transactions

### 3️⃣ Enhance database + API

✅ Update API:

Compute funding_earned, received, available dynamically or via precomputed cache

### 4️⃣ Optional: Build caching layer

✅ Cache exchange rates + wallet data for short periods (e.g., 30s–5m) to reduce external API calls and speed up responses.

