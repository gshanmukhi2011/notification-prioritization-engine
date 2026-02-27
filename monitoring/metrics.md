# Monitoring & Metrics

The system includes both technical and business monitoring.

---

## 1. Technical Metrics

These ensure system health and performance.

- Decision latency (time to classify notification)
- Queue processing delay
- API response time
- Redis availability
- Scheduler success rate
- AI timeout rate

If any of these exceed thresholds, alerts are triggered.

---

## 2. Business Metrics

These measure user experience impact.

- Suppression rate
- Notification send rate
- Deferred (Later) rate
- User engagement rate
- Notification opt-out rate

---

## 3. Logging & Observability

- Every decision (Now/Later/Never) is logged.
- All suppressions are logged with reasons.
- Failures are logged and monitored.

Monitoring tools such as Prometheus, Grafana, or CloudWatch can be used for tracking and alerts.

---

## Monitoring Principle

The system must be observable, auditable, and continuously monitored to ensure reliability and good user experience.
