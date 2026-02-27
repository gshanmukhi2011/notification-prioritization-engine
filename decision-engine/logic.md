# Decision Logic

The system classifies every notification into:

- Now
- Later
- Never


---

## 1. Step 1: Hard Rules (Highest Priority)

These rules are checked first.

1. If the notification is expired (expires_at < current time)  
   → Decision: Never

2. If the notification is marked as critical (system alert, security alert)  
   → Decision: Now

3. If the notification is an exact duplicate  
   → Decision: Never

If none of the above conditions apply, move to scoring.


---

## 2. Step 2: Scoring Model

Each notification gets a final score.

Final Score =

Priority Score  
- Fatigue Penalty  
- Duplicate Similarity Penalty  


### Priority Score

- High priority = 80
- Medium priority = 50
- Low priority = 20


### Fatigue Penalty

If user received many notifications recently:

- More than 5 in last 10 minutes → -40
- More than 10 in last 1 hour → -30
- Otherwise → 0


### Duplicate Similarity Penalty

If similar notification sent recently:

- High similarity → -50
- Medium similarity → -25
- No similarity → 0


---

## 3. Final Classification

After calculating Final Score:

- Score > 70 → Now
- Score between 40 and 70 → Later
- Score < 40 → Never


---

## 4. Conflict Handling

If a notification is high priority but user is in a noisy period:

- If critical → Always Now
- If not critical → Downgrade to Later

This ensures important notifications are not lost, while still reducing alert fatigue.


---

## 5. Explainability

For every decision, the system logs:

- Final Score
- Rules applied
- Penalties applied
- Final classification

This ensures the system is auditable and explainable.
