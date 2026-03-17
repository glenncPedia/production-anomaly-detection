# production-anomaly-detection
Production anomaly detection for large-scale distributed systems using robust statistics and dimensional telemetry signals.

# Production Anomaly Detection for Distributed Systems

Detecting production regressions in large-scale distributed systems is difficult.

Telemetry signals are noisy, traffic patterns change constantly, and failures often
appear only within narrow slices of the system.

This project describes the design of a statistical anomaly detection framework built
to surface production regressions across thousands of dimensional telemetry signals.


---

# Problem

Modern distributed platforms generate enormous volumes of telemetry data.

A checkout or transaction system may produce signals segmented by dimensions such as:

- brand
- device type
- supplier
- geography
- itinerary structure
- payment method
- feature flags

These combinations quickly produce **thousands of telemetry signals**.

Traditional monitoring systems struggle in this environment because:

- conversion metrics are inherently noisy
- traffic mix changes constantly
- dimensional slices often have low volume
- large outages distort statistical baselines
- short spikes create excessive alert noise

As a result, severe customer-facing failures can persist undetected.


---

# System Architecture

The anomaly detection system processes clickstream telemetry and produces
statistical signals used to detect regressions.

High-level pipeline:

Clickstream Logs
      │
      ▼
Session Reconstruction
(PySpark)
      │
      ▼
Feature Engineering
(thousands of dimensional signals)
      │
      ▼
Rolling Window Statistics
(Median + MAD)
      │
      ▼
Persistence Filtering
      │
      ▼
Anomaly Alerts
