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

**2. Write a query that returns the company(customer_id column) with the highest number of users that use desktop only**
```sql
SELECT customer_id, COUNT(DISTINCT user_id) AS desktop_only_users
FROM usage_table
WHERE device_type = 'desktop' AND user_id NOT IN
 (SELECT DISTINCT user_id
 FROM usage_table
 WHERE device_type != 'desktop')
GROUP BY customer_id
ORDER BY desktop_only_users DESC
LIMIT 1;
```

**3. Write a query that returns a list of the bottom 2 companies by mobile usage. Company is defined in the customer_id column. Mobile usage is defined the number of events registered on a client_id == 'mobile'. Order the result by the number of events scending. In the case where there are multiple companies tied for the bottom ranks(rank 1 or 2), return all the companies, Output the customer_id and number of events**
```sql

```



















