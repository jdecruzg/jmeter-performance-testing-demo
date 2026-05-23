# JMeter Performance Testing Demo

Performance testing project using **Apache JMeter** against a local industrial API.

The objective is to demonstrate basic-intermediate performance testing skills by simulating concurrent users, ramp-up, duration control, token extraction, protected endpoints and HTML report generation.

---

## Scenario

The test simulates users executing the following API flow:

1. Health check
2. Login
3. Token extraction
4. Get existing industrial part
5. Create production order
6. Extract created order ID
7. Get created production order

---

## Technologies

- Apache JMeter
- Node.js API as system under test
- JSON Extractor
- Response Assertions
- HTML Dashboard Report
- WSL/Linux execution

---

## Project Structure

```text
jmeter-performance-testing-demo/
│
├── test-plan/
│   └── industrial-api-load-test.jmx
│
├── results/
│   └── industrial-api-load-test.jtl
│
├── reports/
│   └── html/
│
├── docs/
│   └── performance-test-summary.md
│
├── .gitignore
└── README.md