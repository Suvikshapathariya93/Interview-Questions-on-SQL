# Walmart SQL Interview Questions and Answers

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
