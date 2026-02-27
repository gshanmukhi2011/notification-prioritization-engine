# System Architecture

## 1. Problem Overview

Users receive too many notifications from different services like messages, reminders, alerts, promotions, and system updates. 

The system must decide for each notification event whether to:

- Send Now
- Send Later (schedule)
- Never send (suppress)

The system must also:
- Prevent duplicate notifications
- Reduce alert fatigue
- Be explainable and auditable
- Fail safely if any service is unavailable


---

## 2. High-Level Components

The system contains the following components:

1. API Layer  
   - Receives incoming notification events.

2. Decision Engine  
   - Core logic that decides: Now / Later / Never.

3. Duplicate Checker  
   - Prevents exact and near-duplicate notifications.

4. User State Store  
   - Stores user notification history and counters.

5. Scheduler  
   - Handles deferred notifications (Later).

6. Notification Dispatcher  
   - Sends notifications via push, email, SMS, or in-app.

7. Audit Logger  
   - Stores explanation for every decision.


---

## 3. Data Flow

Step 1: A notification event is received through the API.

Step 2: The event is passed to the Decision Engine.

Step 3: The system checks:
- Expiry
- Duplicates
- User notification frequency
- Priority

Step 4: The system classifies the event:
- Now → Sent immediately
- Later → Scheduled
- Never → Suppressed

Step 5: The system logs the decision with explanation.


---

## 4. Design Principles

- Low latency decisions
- High reliability
- Explainability for every decision
- Important notifications must never be silently lost
- Human rules configurable without redeployment
---

## 5. Duplicate Prevention Strategy

The system prevents both exact and near-duplicate notifications.

### Exact Duplicate Handling

- If a dedupe_key is provided, it is stored in a fast storage (Redis) with a time-to-live (TTL).
- If another event arrives with the same dedupe_key within the TTL window, it is suppressed.
- The suppression is logged in the audit table.

### Near-Duplicate Handling

If dedupe_key is missing or unreliable:

- The system compares message content and event type.
- If a similar notification was sent to the same user within a short time window (for example, 5 minutes), it is considered a near-duplicate.
- A similarity penalty is applied in the scoring model.

### Safety Principle

Important notifications are never suppressed silently.  
Every suppressed event is logged with a reason for auditing.
---

## 6. Alert Fatigue Mitigation Strategy

The system reduces alert fatigue using the following mechanisms:

### 1. Frequency Caps

- Maximum 5 notifications per 10 minutes.
- Maximum 10 notifications per 1 hour.
- If limits are exceeded, lower-priority notifications are delayed or suppressed.

### 2. Cooldown Window

If a user receives too many notifications in a short time:

- A cooldown period is applied.
- During cooldown, only high-priority notifications are allowed.

### 3. Priority-Based Filtering

- Critical notifications always bypass fatigue limits.
- Promotional notifications are suppressed during noisy periods.

### 4. Deferred Delivery

Medium-priority notifications may be scheduled for later instead of being sent immediately.

### 5. Digest Mode (Optional Enhancement)

Low-priority notifications can be grouped and sent as a single summary instead of multiple individual alerts.

---

### Design Principle

The system balances user experience and business needs by ensuring important notifications are delivered while minimizing unnecessary interruptions.
