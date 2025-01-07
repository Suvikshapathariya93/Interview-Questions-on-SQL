**1. Select the most popular client_id based on a count of the number of users who have at least 50% of their events from the following list: 'video call received','video call sent','voice call received','voice call sent'**
Objective : Most popular client_id
```sql
SELECT client_id
FROM (SELECT client_id, COUNT(DISTINCT user_id) AS user_count
    FROM (SELECT user_id, client_id,
               SUM(CASE WHEN event IN ('video call received', 'video call sent', 'voice call received', 'voice call sent') THEN 1 ELSE 0 END) AS matching_events, COUNT(*) AS total_events
        FROM events_table
        GROUP BY 1, 2) user_event_stats
    WHERE matching_events >= 0.5 * total_events
    GROUP BY client_id
) client_user_counts
ORDER BY user_count DESC
LIMIT 1;
```
