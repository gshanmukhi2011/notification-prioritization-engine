# Data Model

The system requires the following main data structures.


---

## 1. Notification Events Table

Stores every incoming notification event.

Fields:

- event_id (UUID)
- user_id
- event_type
- source
- channel (push/email/SMS/in-app)
- priority_hint (high/medium/low)
- message
- timestamp
- expires_at
- dedupe_key (optional)


---

## 2. User Notification Stats Table

Stores user notification history and counters.

Fields:

- user_id
- last_10_min_count
- last_1_hour_count
- last_sent_timestamp
- cooldown_until


---

## 3. Suppression Records Table

Stores suppressed notifications for auditing.

Fields:

- event_id
- user_id
- suppression_reason
- related_event_id (if duplicate)
- timestamp


---

## 4. Scheduled Notifications Table

Stores deferred notifications.

Fields:

- event_id
- user_id
- scheduled_time
- reason
- status (pending/sent/cancelled)


---

## 5. Audit Logs Table

Stores explanation of every decision.

Fields:

- event_id
- user_id
- decision (Now/Later/Never)
- final_score
- rules_applied
- penalties_applied
- decision_timestamp


---

## Design Notes

- Frequently accessed counters (like last_10_min_count) should be stored in fast storage (Redis).
- Audit logs must be stored permanently for explainability.
- Important notifications must never be deleted without logging.
