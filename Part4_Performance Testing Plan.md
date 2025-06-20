1. Objectives
    Confirm the sign-up/login service can sustain 10,000 concurrent users
    Measure 90th-percentile response time (target ≤ 2 s)
    Ensure error rate < 1% under peak

2. Tools
    Apache JMeter
        Open-source, widely adopted
        Easy to distribute load across multiple generators
        Rich reporting (percentiles, throughput, error rate)


3. Test Case Scenarios
| ID   | Action            | Request Type | Endpoint              | Notes                                |
| ---- | ----------------- | ------------ | --------------------- | ------------------------------------ |
| PT-1 | User Registration | POST         | `/api/signup`         | payload: `{username,password,email}` |
| PT-2 | User Login        | POST         | `/api/login`          | payload: `{username,password}`       |
| PT-3 | Fetch Profile     | GET          | `/api/user/profile`   | after login, pass JWT token          |
| PT-4 | Password Reset    | POST         | `/api/password-reset` | optional extra scenario              |


4. Metrics to Monitor
    Response Time: average, 90th & 99th percentiles
    Throughput: requests/sec
    Error Rate: % of failed requests
    Resource Utilization: CPU, memory, network on server

5. Test Environment
    Servers: mirror production specs (e.g., 4 vCPU, 16 GB RAM)
    Network: simulate mobile-like latency using JMeter’s network emulator
    Distributed Load: 2–3 JMeter slave machines to generate 10k users
    Isolation: run tests in staging VPC to prevent external interference

6. Result Analysis
    Inspect JMeter dashboard for bottleneck patterns
    Correlate high response-time spikes with server CPU/memory graphs
    Identify failing endpoints and error codes
    Produce a report with tables/graphs and recommendations (e.g., caching, DB indexing)
