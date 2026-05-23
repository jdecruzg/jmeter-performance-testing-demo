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

This scenario represents a simplified industrial workflow similar to what could happen in internal systems such as ERP, WMS, MES or production management tools.

---

## Technologies

- Apache JMeter
- WSL/Linux
- JSON Extractor
- Response Assertions
- HTTP Header Manager
- HTML Dashboard Report
- Local Node.js API as system under test

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
```

---

## Prerequisites

This project uses the API created in the Postman/Newman project as the system under test.

Start the API in one terminal:

```bash
cd ~/api-testing-postman-newman-demo
npm start
```

Check that the API is running:

```bash
curl http://127.0.0.1:4000/api/health
```

Expected response:

```json
{
  "status": "ok",
  "service": "Industrial API",
  "environment": "local"
}
```

---

## Install JMeter in WSL

Install Java:

```bash
sudo apt update
sudo apt install -y openjdk-17-jre wget tar
```

Download and extract JMeter:

```bash
cd ~
mkdir -p tools
cd tools

wget https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-5.6.3.tgz
tar -xzf apache-jmeter-5.6.3.tgz
```

Add JMeter to the PATH:

```bash
echo 'export JMETER_HOME="$HOME/tools/apache-jmeter-5.6.3"' >> ~/.bashrc
echo 'export PATH="$JMETER_HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Check JMeter installation:

```bash
jmeter --version
```

---

## Load Test Configuration

The test plan is parameterized. The number of users, ramp-up and duration can be changed from the command line.

| Parameter | Description | Example |
|---|---|---:|
| `users` | Number of concurrent users | `10` |
| `rampup` | Time to start all users | `30` seconds |
| `duration` | Total test duration | `120` seconds |

---

## Run a Short Test

Use this command to validate that the test plan works correctly:

```bash
jmeter -n \
  -t test-plan/industrial-api-load-test.jmx \
  -l results/industrial-api-load-test.jtl \
  -Jusers=2 \
  -Jrampup=5 \
  -Jduration=20
```

This test simulates:

| Parameter | Value |
|---|---:|
| Users | 2 |
| Ramp-up | 5 seconds |
| Duration | 20 seconds |

---

## Run Portfolio Test

Use this command for the main portfolio execution:

```bash
rm -f results/industrial-api-load-test.jtl
rm -rf reports/html

jmeter -n \
  -t test-plan/industrial-api-load-test.jmx \
  -l results/industrial-api-load-test.jtl \
  -Jusers=10 \
  -Jrampup=30 \
  -Jduration=120

jmeter -g results/industrial-api-load-test.jtl -o reports/html
```

This test simulates:

| Parameter | Value |
|---|---:|
| Users | 10 |
| Ramp-up | 30 seconds |
| Duration | 120 seconds |
| Think time | 500 ms |

---

## Generate HTML Report

If the `.jtl` result file already exists, generate the HTML dashboard with:

```bash
jmeter -g results/industrial-api-load-test.jtl -o reports/html
```

Open the report from WSL:

```bash
explorer.exe reports/html
```

Then open:

```text
index.html
```

---

## Metrics Reviewed

The generated JMeter report allows reviewing:

| Metric | Meaning |
|---|---|
| Samples | Total number of requests executed |
| Average | Average response time |
| Min | Minimum response time |
| Max | Maximum response time |
| Error % | Percentage of failed requests |
| Throughput | Number of requests processed per second/minute |
| 90th percentile | 90% of requests responded below this time |
| 95th percentile | 95% of requests responded below this time |
| 99th percentile | 99% of requests responded below this time |

---

## Test Flow

Each simulated user executes the following sequence:

```text
01 - Health Check
02 - Login
03 - Get Existing Part
04 - Create Production Order
05 - Get Created Production Order
```

The complete technical flow is:

```text
Check API availability
Login with valid credentials
Extract Bearer token
Use token in protected endpoints
Get existing part
Create production order
Extract created order ID
Retrieve created order
Validate response codes with assertions
Generate performance results
```

---

## QA Validations

The test plan includes:

- HTTP response code assertions
- Bearer token extraction
- Protected endpoint testing
- JSON response data extraction
- Order ID extraction
- Consistency check between created and retrieved order
- Think time between requests
- Configurable load parameters

---

## Acceptance Criteria

The test is considered acceptable if:

- Error rate remains below 1%.
- Most requests respond under 2 seconds.
- The API remains stable during the full test duration.
- Authentication and protected endpoints work correctly under load.
- The generated report contains valid throughput, error rate and percentile data.

---

## QA Value

This project demonstrates performance testing skills in an API context:

- Concurrent users
- Ramp-up configuration
- Duration-based execution
- Bearer token extraction
- Protected endpoint testing
- Response assertions
- HTML performance reporting
- Throughput and percentile analysis
- Execution from WSL/Linux in non-GUI mode

---

## Interview Summary

This project shows how Apache JMeter can be used to evaluate the behavior of an API under concurrent load.

The test simulates users performing a realistic industrial workflow: health check, login, token extraction, protected resource access, order creation and order retrieval.

It also demonstrates the ability to analyze performance metrics such as response time, throughput, error rate and percentiles, which are essential for validating the stability of business-critical systems.
