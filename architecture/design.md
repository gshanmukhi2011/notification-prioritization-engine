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
