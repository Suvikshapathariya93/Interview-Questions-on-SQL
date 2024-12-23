## Airbnb SQL Interview Questions and Answers

# SQL Interview Questions based on multiple companies.

**1. Select project with highest budget per employee ratio from two tables (projects and employees).**
```sql
select p.project_id, p.project_name, p.budget,
count(e.employee_id) as total_employees,
(p.budget /count(e.employee_id) as budget_per_employee
from projects p
left join employee e
on p.project_id = e.project_id
group by 1,2,3
order by budget_per_employee desc;
limit 1;
```
- `left join`: Joins the projects table with the employees table on the project_id.
- `count(e.employee_id)`: Counts the number of employees assigned to each project.
- `p.budget / count(e.employee_id)`: Calculates the budget per employee ratio for each project.
- `group by`: Groups the results by project ID, project name, and budget to calculate aggregates like the employee count.
- `order by budget_per_employee DESC`: Sorts the results by the budget per employee ratio in descending order.
- `limit 1`: Fetches only the project with the highest ratio.

**2. Total number of transaction per user for each day**
```sql
select user_id, date(transaction_date) as transaction_day, count(transcation_id) as total_transactions
from transcations
group by 1,2
order by transaction_day, user_id desc;
```
- `date(transaction_date)`: Extracts the date from the timestamp to group transactions by day.
- `count(transaction_id)`: Counts the number of transactions for each user on a particular day.
- `group by 1,2` : Groups the results by user and day.
- `order by transaction_day, user_id`: Sorts the output by the date and user for better readability.

**3. Find all Airbnb listings in San Francisco and New York that have at least 10 reviews and an average rating equal to or above 4.5**
- tables:
   - listings: `listing_id`, `name`, `city`, `review_count`
   - reviews: `listing_id`, `review_id`, `stars`, `submit_date`
- query: 
```sql
select l.listing_id, l.name, l.city, l.reviews_count, avg(r.stars) as average_rating
from listings l
join reviews r
on l.listing_id = r.listing_id
where l.city in ('San Francisco', 'New York') and l.review_count >=10
group by 1,2,3,4
having average_rating >=4;
```

**4. Write a SQL query that will find the average number of guests per booking in each city**
- tables:
   - bookings: `booking_id`, `property_id`, `guests`, `booking_date`
   - properties: `property_id`, `city`
- query:
```sql
select b.booking_id, b.property_id, p. city, avg(b.guests) as average_guests
from bookings b
join properties p
on b.property_id = p.property_id
group by 1,2,3
order by average_guests desc;
```

**5. To analyze the click-through conversion rates(CTRs) of their listings. The CTR is calculated by the dividing 
the number of booking by the number of listing views, giving a proportion of views that resulted in a booking.**
- tables:
   - listing_views: `view_id`, `user_id`, `visit_date`, `listing_id`
   - bookings: `booking_id`, `user_id`, `booking_date`, `listing_id`
- query:
```sql
select l.listing_id, count(distinct b.booking_id) as total_bookings, count(distinct l.view_id) as total_views,
case when count(distinct l.view_id) = 0 then 0
else count(distinct b.booking_id) * 1.0/ count(distinct l.view_id)
end as ctr
from listing_views l
join bookings b
on l.listing_id = b.lisitng_id
group by 1
order by ctr desc;
```

**6. To write a query that outputs the average vacant days across the airbnb in 2021. Some properties have gone out of business so 
you should only analyze rentals that are currently active. Round the results to a whole number.
- assumptions:
   - `is_active` field equals 1 when the property is active and 0 otherwise.
   - in case where the check-in or check-out date is another year other than 2021, limit the calculation to the beginning end of the year 2021 respectively
   - a listing can be active even if there are no bookings throughout the year.
- tables:
   - bookings: `lisiting_id`, `checkin_date`, `checkout_date`
   - listings: `listing_id`, `is_active`
- query:
```sql
WITH date_range_adjusted_bookings AS (
    SELECT l.listing_id,
        GREATEST(DATE('2021-01-01'), b.checkin_date) AS adjusted_checkin,
        LEAST(DATE('2021-12-31'), b.checkout_date) AS adjusted_checkout
    FROM bookings b
    JOIN listings l 
    ON b.listing_id = l.listing_id
    WHERE l.is_active = 1 AND (b.checkin_date <= DATE('2021-12-31') AND b.checkout_date >= DATE('2021-01-01'))),
booked_days_per_listing AS (
    SELECT listing_id, SUM(DATEDIFF(adjusted_checkout, adjusted_checkin) + 1) AS total_booked_days
    FROM date_range_adjusted_bookings
    GROUP BY listing_id),
vacant_days_per_listing AS (
    SELECT l.listing_id, 365 - COALESCE(b.total_booked_days, 0) AS vacant_days_in_2021
    FROM listings l
    LEFT JOIN booked_days_per_listing b
    ON l.listing_id = b.listing_id
    WHERE l.is_active = 1)
SELECT ROUND(AVG(vacant_days_in_2021)) AS average_vacant_days
FROM vacant_days_per_listing;
```
- `date_range_adjusted_bookings` CTE: Adjusts the checkin_date and checkout_date for bookings to the boundaries of 2021 using GREATEST and LEAST functions.
- `booked_days_per_listing` CTE: Calculates the total booked days for each active listing in 2021 by summing the number of days between adjusted checkin_date and checkout_date.
- `vacant_days_per_listing` CTE: Calculates the vacant days for each listing by subtracting the total booked days from 365. Uses COALESCE to handle listings with no bookings (assigns 0 booked days).
- Final Query: Calculates the average number of vacant days across all active listings in 2021 and rounds the result to the nearest whole number using ROUND.
- **Listings with no bookings are treated as fully vacant for 2021.**
- **Only active listings (`is_active` = 1) are analyzed.**
- **Bookings that partially overlap with 2021 are adjusted to include only the days within 2021.**



