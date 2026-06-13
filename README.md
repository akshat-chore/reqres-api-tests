# Reqres API Test Suite

A professional API test suite built with Postman and executed via Newman CLI, designed as a portfolio project demonstrating API testing skills for QA/SDET roles.

## Project Structure

```
reqres-api-tests/
├── collections/
│   └── Reqres API Tests.postman_collection.json
├── environments/
│   └── Reqres Environment.postman_environment.json
├── reports/
│   └── newman-report.html
├── package.json
└── README.md
```

## What's Tested

**Base URL:** https://reqres.in

| Suite | Requests | Description |
|---|---|---|
| Users & Products - GET | 5 | User listing, single user retrieval, 404 handling, product listing |
| Auth - POST | 5 | Login and registration — valid and invalid flows |
| CRUD | 6 | Create, update (full & partial), delete, edge cases |

**Total: 16 requests, 40 assertions**

## Test Coverage

- Status code validation (200, 201, 204, 400, 404)
- Response body structure and field type assertions
- Negative testing — missing fields, invalid IDs
- Response time threshold (under 2000ms)
- Content-Type header validation
- Auth token extraction and collection variable chaining

## Tools Used

- **Postman** — collection and environment setup
- **Newman CLI** — command line test execution
- **newman-reporter-htmlextra** — HTML report generation

## Known API Limitations (Reqres.in Mock Behavior)

| Scenario | Expected | Actual | Reason |
|---|---|---|---|
| Login with wrong password | 400 | 200 | Mock API does not validate credentials |
| Register with wrong password | 400 | 200 | Mock API does not validate credentials |
| PUT on non-existent user (id: 9999) | 404 | 200 | Mock API does not check if resource exists |
| POST with empty body | 400 | 201 | Mock API does not validate request body |
| GET after POST/PUT/PATCH | Updated data | Original data | Mock API does not persist data |

These are documented behaviors of the mock API, not test failures. In a real-world API these would be valid negative test scenarios.

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

The environment file requires an `api_key` value. Get a free key at [app.reqres.in/api-keys](https://app.reqres.in/api-keys) and update the `value` field for `api_key` in `environments/Reqres Environment.postman_environment.json`.

## Results

| Metric | Count |
|---|---|
| Total Requests | 16 |
| Total Assertions | 40 |
| Passed | 38 |
| Failed | 2 (known mock API limitations) |
| Skipped | 0 |