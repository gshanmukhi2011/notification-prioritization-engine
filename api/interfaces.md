# API Interfaces

The system exposes the following APIs.


---

## 1. Evaluate Notification

Endpoint:

POST /notifications/evaluate

Purpose:
Receives a notification event and returns decision: Now / Later / Never.


### Sample Request

{
  "user_id": "123",
  "event_type": "new_message",
  "source": "chat-service",
  "channel": "push",
  "priority_hint": "medium",
  "message": "You have a new message",
  "timestamp": "2026-02-27T10:30:00Z"
}


### Sample Response

{
  "decision": "Later",
  "final_score": 55,
  "reason": "User exceeded 5 notifications in last 10 minutes",
  "scheduled_time": "2026-02-27T11:00:00Z"
}


---

## 2. Get Audit Log

Endpoint:

GET /notifications/{event_id}/audit

Purpose:
Returns explanation of decision for a notification.


### Sample Response

{
  "event_id": "abc123",
  "decision": "Never",
  "final_score": 30,
  "rules_applied": ["duplicate_check"],
  "penalties_applied": ["fatigue_penalty"],
  "decision_timestamp": "2026-02-27T10:31:00Z"
}


---

## 3. Update Rules Configuration

Endpoint:

PUT /config/rules

Purpose:
Allows admin to update rule thresholds without redeployment.


### Sample Request

{
  "max_notifications_10_min": 5,
  "max_notifications_1_hour": 10,
  "high_priority_score": 80
}


---

## 4. Get User Notification Summary

Endpoint:

GET /users/{user_id}/notification-summary

Purpose:
Returns current notification statistics for a user.


### Sample Response

{
  "user_id": "123",
  "last_10_min_count": 4,
  "last_1_hour_count": 9,
  "cooldown_until": null
}
