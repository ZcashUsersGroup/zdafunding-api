# ZDA Funding Wallet API


### Locally (for development)
```bash
node server.js
````

### Deployment 

This API is intended to be deployed to a public service (e.g., Render, Azure, or similar) so it can be accessed by the frontend and external users. After deployment, share the public URL with your frontend developer.


## API Endpoints

* `GET /api/v1/exchange-rate`
* `GET /api/v1/cards`
* `GET /api/v1/cards/{id}`

## Rate limit

10 requests per minute per IP


### 🧪 **Basic sanity checks**

* ✅ `/api/v1/exchange-rate` returns 200 + valid JSON
* ✅ `/api/v1/cards` returns 200 + correct pagination
* ✅ `/api/v1/cards/:id` returns 200 for valid ID, 404 for invalid ID
* ✅ Rate limit triggers at the set threshold
* ✅ Bad query params (e.g., `sort_by=badfield`) handled gracefully


## Future work: CI/CD and tests
"Write Jest + Supertest tests for the API endpoints and set up a GitHub Actions workflow to run them on each push."
