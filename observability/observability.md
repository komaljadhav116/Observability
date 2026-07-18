# Introduction to Observability


## What is Observability?

Observability is the ability to understand the internal state of a system by analyzing the data it generates, such as logs, metrics, and traces. It helps teams detect, troubleshoot, and fix issues quickly by identifying the root cause of problems.

---

# Three Pillars of Observability


Observability is built on three major pillars:


```
                    Observability


                         |
        +----------------+----------------+
        |                |                |
        v                v                v

     Metrics           Logs            Traces

   Monitoring       Logging          Tracing


- **Monitoring:** Checks system health using metrics and alerts when something goes wrong.  
  **Example:** CPU usage reaches 90%, alert is triggered.

- **Logging:** Records events and errors that happen in the system.  
  **Example:** "Database connection failed" is saved in logs.

- **Tracing:** Tracks the path of a request through different services to find where the problem occurs.  
  **Example:** User request → API → Payment Service → Database.

```
# Why Monitoring?

Monitoring helps us keep an eye on our systems to ensure they are working properly.

**Purpose:** Maintaining the health, performance, and security of IT environments.

It enables early detection of issues, ensuring that they can be addressed before causing significant downtime or data loss.

---

# Why Observability?

Observability helps us understand the internal state of our systems by analyzing logs, metrics, and traces.

It helps teams detect issues, troubleshoot problems, and identify the root cause quickly.

It enables better system reliability, faster problem resolution, and improved performance of applications.

---
# Monitoring vs Observability

Monitoring tells us **when** and **what** is happening in a system error.

Observability tells us **why** and **how** the error happened by analyzing logs, metrics, and traces.

---
# What Can Be Monitored?

- **Infrastructure:** CPU usage, memory usage, disk I/O, network traffic.
- **Applications:** Response times, error rates, throughput.
- **Databases:** Query performance, connection pool usage, transaction rates.
- **Network:** Latency, packet loss, bandwidth usage.
- **Security:** Unauthorized access attempts, vulnerability scans, firewall logs.

---

# What Can Be Observed?

- **Logs:** Detailed records of events and transactions within the system.
- **Metrics:** Quantitative data points like CPU load, memory consumption, and request counts.
- **Traces:** Data that shows the flow of requests through various services and components.

---

# What are the Tools Available?

## Monitoring Tools

- Prometheus
- Grafana
- Nagios
- Zabbix
- PRTG

## Observability Tools

- ELK Stack (Elasticsearch, Logstash, Kibana)
- EFK Stack (Elasticsearch, FluentBit, Kibana)
- Splunk
- Jaeger
- Zipkin
- New Relic
- Dynatrace
- Datadog