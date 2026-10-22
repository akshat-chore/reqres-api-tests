# Reqres API Test Suite (version 3)

A professional API test suite built with Postman and executed via Newman CLI, designed as a portfolio project demonstrating API testing skills for QA/SDET roles.

## Project Structure

```
reqres-api-tests/
├── .github/
│   └── workflows/
│       └── api-tests.yml
├── collections/
│   └── reqres-api-tests.postman_collection.json
├── environments/
│   └── reqres-environment.postman_environment.json
├── reports/
│   └── newman-report.html
├── package.json
└── README.md
```

## What's Tested

**Base URL:** https://reqres.in

| Suite | Requests | Assertions | Description |
|---|---|---|---|
| Users & Products - GET | 5 | 17 | User listing, pagination, single user retrieval, 404 handling, product listing |
| Auth - POST | 5 | 11 | Login and registration — valid and invalid flows, token extraction |
| CRUD | 6 | 30 | Create, full update, partial update, delete, edge cases |

**Total: 16 requests, 58 assertions**

## Test Coverage

- Status code validation (200, 201, 204, 400, 404)
- Response body structure and field type assertions
- Field value assertions against request body (name, job matching)
- ISO date string validation for createdAt and updatedAt fields
- Negative testing — missing fields, invalid IDs, empty request body
- Response time thresholds (under 2000ms–3000ms)
- Content-Type header validation
- Auth token extraction and collection variable chaining
- Business logic intent assertions on mock API limitations

## Tools Used

- **Postman** — collection and environment setup
- **Newman CLI** — command line test execution
- **newman-reporter-htmlextra** — HTML report generation
- **GitHub Actions** — CI/CD pipeline for automated test runs on every push

## Known API Limitations (Reqres.in Mock Behavior)

These tests are intentionally written against **business logic intent**, not mock API behavior. Failures below are expected and document real gaps in the mock API.

| Request | Expected (Business Logic) | Actual (Mock API) | Intentional Failure |
|---|---|---|---|
| Login - Invalid Credentials | 401 Unauthorized | 200 with token | Yes |
| Login - Register | 201 Created | 200 OK | Yes |
| Login - Register Invalid Credentials | 400 Bad Request | 200 OK | Yes |
| Create User with Empty Body | 400 Bad Request | 201 Created | Yes |
| Update Non-Existent User (id: 9999) | 404 Not Found | 200 OK | Yes |
| GET after POST/PUT/PATCH | Updated data | Original data | Mock does not persist data |

In a real-world API, all of the above would be valid and passing test scenarios.

## How to Run

### Prerequisites
- Node.js installed
- Newman and htmlextra reporter installed globally:

```bash
npm install -g newman
npm install -g newman-reporter-htmlextra
```

### Run the test suite

```bash
npm test
```

Report will be generated at `reports/newman-report.html`. Open it in any browser.

### Environment Setup

The environment file requires an `api_key` value. Get a free key at [app.reqres.in/api-keys](https://app.reqres.in/api-keys) and update the `value` field for `api_key` in `environments/reqres-environment.postman_environment.json`.

## CI/CD

This project uses GitHub Actions to automatically run the full test suite on every push to `main` and on pull requests. The HTML report is uploaded as a build artifact after each run and can be downloaded from the Actions tab.

## Results

| Metric | Count |
|---|---|
| Total Requests | 16 |
| Total Assertions | 58 |
| Intentional Failures | 11 (business logic intent vs mock API) |
| Skipped | 0 |
