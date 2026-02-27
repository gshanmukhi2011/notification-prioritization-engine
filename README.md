# Notification Prioritization Engine

Author: Shanmukhi Gajula  
Submission for: Cyepro Solutions – AI-Native Solution Crafting Test  
Date: Feb 2026  

---

## 1. Problem Statement

Users receive too many notifications from multiple services such as messages, reminders, alerts, promotions, and system events.

The goal of this system is to decide, for each incoming notification event, whether it should be:

- **Now** (sent immediately)
- **Later** (deferred/scheduled)
- **Never** (suppressed)

The system must:
- Prevent duplicates
- Reduce alert fatigue
- Handle conflicting priorities
- Support configurable rules
- Provide clear explanations for each decision
- Fail safely during service outages

---

## 2. High-Level Architecture

The system includes:

- API Layer
- Decision Engine
- Duplicate Checker
- User State Store (Redis + Database)
- Scheduler
- Notification Dispatcher
- Audit Logging System

Detailed design available in:  
`architecture/design.md`

---

## 3. Decision Logic

The decision process follows two stages:

### Stage 1: Hard Rules
- Expired notifications → Never
- Critical alerts → Now
- Exact duplicates → Never

### Stage 2: Scoring Model

Final Score =  
Priority Score – Fatigue Penalty – Similarity Penalty

Final classification:
- Score > 70 → Now
- 40–70 → Later
- < 40 → Never

Full logic available in:  
`decision-engine/logic.md`

---

## 4. Duplicate Prevention Strategy

- Exact duplicates handled using dedupe_key stored in Redis.
- Near-duplicates detected using content similarity checks.
- All suppressed events are logged for auditing.

---

## 5. Alert Fatigue Mitigation

- Frequency caps (5 per 10 min, 10 per hour)
- Cooldown windows
- Priority-based filtering
- Deferred delivery
- Optional digest mode

---

## 6. Fallback & Reliability Strategy

- AI scoring failure → fallback to rule-based logic
- Redis failure → fallback to database counters
- Scheduler failure → immediate send for time-sensitive events
- No silent drops; all decisions logged

---

## 7. Data Model

Includes tables for:

- Notification events
- User notification stats
- Suppression records
- Scheduled notifications
- Audit logs

Details available in:  
`data-model/schema.md`

---

## 8. API Interfaces

Defined endpoints:

- POST /notifications/evaluate
- GET /notifications/{event_id}/audit
- PUT /config/rules
- GET /users/{user_id}/notification-summary

Full API definitions available in:  
`api/interfaces.md`

---

## 9. Monitoring & Metrics

Technical and business metrics defined to ensure:

- Low latency
- High reliability
- Good user experience
- Auditability

Details available in:  
`monitoring/metrics.md`

---

## 10. Tools Used

AI tools such as ChatGPT were used for brainstorming and structuring ideas.  
All architectural decisions and final documentation were reviewed and refined manually.

---

## Conclusion

This design prioritizes:

- User experience
- Reliability
- Explainability
- Scalability

It ensures important notifications are delivered while minimizing unnecessary interruptions.
