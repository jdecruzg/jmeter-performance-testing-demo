# Performance Test Summary

## Objective

The objective of this test is to evaluate the behavior of a local industrial API under concurrent load using Apache JMeter.

## Scenario

The test simulates users executing the following business flow:

1. Health check
2. Login
3. Token extraction
4. Get existing part
5. Create production order
6. Extract created order ID
7. Get created production order

## Load Configuration

| Parameter | Value |
|---|---:|
| Users | 10 |
| Ramp-up | 30 seconds |
| Duration | 120 seconds |
| Think time | 500 ms |
| Base URL | http://127.0.0.1:4000 |

## Acceptance Criteria

The test will be considered acceptable if:

- Error rate remains below 1%.
- Most requests respond under 2 seconds.
- The API remains stable during the full test duration.
- Authentication and protected endpoints work correctly under load.

## Key Metrics to Review

- Average response time
- 90th percentile
- 95th percentile
- 99th percentile
- Throughput
- Error percentage
- Maximum response time

## QA Value

This test demonstrates basic-intermediate performance testing skills using JMeter, including concurrent users, ramp-up, duration control, token extraction, protected endpoints and HTML reporting.
