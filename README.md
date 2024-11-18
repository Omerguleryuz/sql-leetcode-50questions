# sql-leetcode-50questions

1) Write a solution to find the ids of products that are both low fat and recyclable.

```sql
select 
    product_id
from Products
where 
    low_fats = 'Y' and
    recyclable = 'Y'
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/1.PNG)

2) Find the names of the customer that are not referred by the customer with id = 2.

```sql
select
    name
from Customer
where referee_id != 2
    or referee_id is null
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/2.PNG)

3) A country is big if:
•	it has an area of at least three million (i.e., 3000000 km2), or
•	it has a population of at least twenty-five million (i.e., 25000000).
Write a solution to find the name, population, and area of the big countries.

```sql
select
    name,
    population,
    area
from World
where area >= 3000000
    or population >= 25000000
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/3.PNG)

4) Write a solution to find all the authors that viewed at least one of their own articles.
Return the result table sorted by id in ascending order.

```sql
select
    distinct author_id as id
from Views
where author_id = viewer_id
order by author_id
```

5) Write a solution to find the IDs of the invalid tweets. The tweet is invalid if the number of characters used in the content of the tweet is strictly greater than 15.

```sql
select
    tweet_id	
from Tweets
where length(content) > 15
```


6) Write a solution to show the unique ID of each user, If a user does not have a unique ID replace just show null.

```sql
select
    unique_id,
    name
from Employees
    left join EmployeeUNI
        on Employees.id = EmployeeUNI.id
```


7) Write a solution to report the product_name, year, and price for each sale_id in the Sales table.

```sql
select
    product_name,
    year,
    price
from Sales
    left join Product
        on Sales.product_id = Product.product_id
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/7.PNG)


8) Write a solution to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.

```sql
select  
    customer_id,
    count(customer_id) as count_no_trans
from Visits
    left join Transactions
        on Visits.visit_id = Transactions.visit_id
where transaction_id is null
group by customer_id
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/8.PNG)


9) Write a solution to find all dates' id with higher temperatures compared to its previous dates (yesterday).

```sql
with all_days_weathers as (
select
    *,
    lag(temperature, 1) over (order by id) as previous_temperature
from Weather
)

select 
    id
from all_days_weathers
where temperature > previous_temperature
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/9.PNG)


10) There is a factory website that has several machines each running the same number of processes. Write a solution to find the average time each machine takes to complete a process.
The time to complete a process is the 'end' timestamp minus the 'start' timestamp. The average time is calculated by the total time to complete every process on the machine divided by the number of processes that were run.
The resulting table should have the machine_id along with the average time as processing_time, which should be rounded to 3 decimal places.


```sql
with whole_table as (
    select
        *,
        lag(timestamp) over(partition by machine_id, process_id order by timestamp) as start_of_the_process
    from Activity
),
process_times as (
    select
        machine_id,
        process_id,
        timestamp - start_of_the_process as processing_time
    from whole_table
)
select
    machine_id,
    avg(processing_time) as processing_time
from process_times
group by machine_id;
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/10.PNG)


11) Write a solution to report the name and bonus amount of each employee with a bonus less than 1000.

```sql
select
    name,
    bonus
from Employee
    left join Bonus
        on Employee.empId = Bonus.empId
where bonus < 1000
    or bonus is null
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/11.PNG)


12) Write a solution to find the number of times each student attended each exam.

```sql
select
    Students.student_id,
    student_name,
    Subjects.subject_name,
    count(Examinations.student_id) as attended_exams
from Students
    cross join Subjects
    left join Examinations
        on Students.student_id = Examinations.student_id
        and Subjects.subject_name = Examinations.subject_name 
group by Students.student_id, student_name, Subjects.subject_name
order by Students.student_id, student_name, Subjects.subject_name
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/12.PNG)


13) Write a solution to find managers with at least five direct reports.

```sql
with employee_counts as
(
select
    managerId,
    count(id)
from Employee
group by managerId
)

select 
    name
from employee_counts
    left join Employee
        on employee_counts.managerId = Employee.id
where count >= 5
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/13.PNG)


14) The confirmation rate of a user is the number of 'confirmed' messages divided by the total number of requested confirmation messages. The confirmation rate of a user that did not request any confirmation messages is 0. Round the confirmation rate to two decimal places.
Write a solution to find the confirmation rate of each user.

```sql
with all_actions as
(
select
    Signups.user_id,
    action,
    case when action = 'confirmed' then action else null end as confirmed_actions
from Signups
    left join Confirmations 
        on Signups.user_id = Confirmations.user_id
)

select
    user_id,
    round(count(confirmed_actions) * 1.0 / count(user_id), 1) as confirmation_rate
from all_actions
group by user_id
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/14.PNG)


15) Write a solution to report the movies with an odd-numbered ID and a description that is not "boring".
Return the result table ordered by rating in descending order.

```sql
select
    *
from Cinema
where mod(id, 2) = 1 
    and description != 'boring'
order by rating desc
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/15.PNG)


16) Write a solution to find the average selling price for each product. average_price should be rounded to 2 decimal places.
If a product does not have any sold units, its average selling price is assumed to be 0.
Return the result table in any order.


```sql
select
    Prices.product_id,
    round(sum(price * units) * 1.0 / sum(units), 2) as average_price
from Prices
    left join UnitsSold
        on Prices.product_id = UnitsSold.product_id
        and UnitsSold.purchase_date between Prices.start_date 
        and Prices.end_date
group by Prices.product_id
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/16.PNG)


17) Write an SQL query that reports the average experience years of all the employees for each project, rounded to 2 digits.

```sql
select 
    project_id,
    round(sum(experience_years) * 1.0 / count(project_id), 2) as average_years
from Project 
    left join Employee
        on Project.employee_id = Employee.employee_id
group by
    project_id
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/17.PNG)


18) Write a solution to find the percentage of the users registered in each contest rounded to two decimals.
Return the result table ordered by percentage in descending order. In case of a tie, order it by contest_id in ascending order.

```sql
select
    contest_id,
    round(count(Users.user_id) * 100.0 / (select 
                                count(distinct user_id) as total_user_count
                            from Users), 2) as percentage
from Register
    left join Users
        on Register.user_id = Users.user_id
group by contest_id
order by 
    percentage desc,
    contest_id asc
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/18.PNG)


19) We define query quality as:
The average of the ratio between query rating and its position.
We also define poor query percentage as:
The percentage of all queries with rating less than 3.
Write a solution to find each query_name, the quality and poor_query_percentage.
Both quality and poor_query_percentage should be rounded to 2 decimal places.

```sql
with poor_rating_counts as
(
select
    query_name,
    count(rating) as poor_rating_count
from Queries
where rating < 3  
group by query_name
)

select
    Queries.query_name,
    round(sum(rating * 1.0 / position) / count(position), 2) as quality,
    round(poor_rating_count * 100.0 / count(rating), 2) as poor_query_percentage
from Queries
    left join poor_rating_counts
        on Queries.query_name = poor_rating_counts.query_name
group by Queries.query_name,
    poor_rating_counts.poor_rating_count
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/19.PNG)


20) Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

```sql
select
    to_char(trans_date, 'YYYY-MM') as month,
    country,
    count(id) as trans_count,
    sum(case when state = 'approved' then 1 else 0 end) as approved_count,
    sum(amount) as trans_total_amount,
    sum(case when state = 'approved' then amount else 0 end) as approved_total_amount
from Transactions
group by 
    month,
    country
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/20.PNG)


21) If the customer's preferred delivery date is the same as the order date, then the order is called immediate; otherwise, it is called scheduled.
The first order of a customer is the order with the earliest order date that the customer made. It is guaranteed that a customer has precisely one first order.
Write a solution to find the percentage of immediate orders in the first orders of all customers, rounded to 2 decimal places.

```sql
with order_rankings as
(
select
    *,
    rank() over(partition by customer_id order by order_date) as order_ranking
from Delivery
),
first_orders as
(
select 
    *
from order_rankings
where order_ranking = 1
)

select
    round(sum(case when order_date = customer_pref_delivery_date then 1 else 0 end) * 100.0
    / count(customer_id), 2) as immediate_percentage
    
from first_orders
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/21.PNG)


22) Write a solution to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places. In other words, you need to count the number of players that logged in for at least two consecutive days starting from their first login date, then divide that number by the total number of players.


```sql
with login_dates as 
(
select
    *,
    lag(event_date, 1) over (partition by player_id order by event_date) as next_login_date
from Activity
)

select
    round(count(case when event_date = next_login_date + INTERVAL '1 day' then 1 else 0 end) 
    * 1.0 / (select
        count(distinct player_id)
    from Activity), 2) as fraction 
from login_dates
where event_date = next_login_date + INTERVAL '1 day'
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/22.PNG)


23) Write a solution to calculate the number of unique subjects each teacher teaches in the university.

```sql
select
    teacher_id,
    count(distinct subject_id) as cnt
from Teacher
group by teacher_id
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/23.PNG)


24) Write a solution to find the daily active user count for a period of 30 days ending 2019-07-27 inclusively. A user was active on someday if they made at least one activity on that day.

```sql
select
    activity_date as day,
    count(distinct user_id) as active_users
from Activity
where activity_date >= date '2019-07-27' - INTERVAL '7 day'
group by activity_date
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/24.PNG)


25) Write a solution to select the product id, year, quantity, and price for the first year of every product sold.

```sql
with rankings as (
select
    *,
    rank() over(partition by product_id order by year) as rank
from Sales
)

select 
    product_id,
    year as first_year,
    quantity,
    price
from rankings
where rank = 1
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/25.PNG)


26) Write a solution to find all the classes that have at least five students.

```sql
with major_classes as 
(
select
    class,
    count(class)
from Courses
group by class
having count(class) >= 5
)

select
    class
from major_classes
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/26.PNG)


27) Write a solution that will, for each user, return the number of followers.

```sql
select
    user_id,
    count(distinct follower_id) as followers_count
from Followers
group by user_id
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/27.PNG)


28) A single number is a number that appeared only once in the MyNumbers table.
Find the largest single number. If there is no single number, report null.

```sql
with num_counts as
(
select
    num,
    count(num) as num_count
from MyNumbers
group by num
)

select  
    max(num) as num
from num_counts
where num_count = 1
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/28.PNG)


29) Write a solution to report the customer ids from the Customer table that bought all the products in the Product table.

```sql
with total_products as
(
select
    count(distinct 
    product_key) as total_product_count
from Product
),
bought_products as 
(
select
    customer_id,
    count(distinct product_key) as bought_distinct_product_count
from Customer
    join total_products 
        on 1 = 1
group by customer_id
)

select
    customer_id
from bought_products
    join total_products
        on 1 = 1
where bought_distinct_product_count = total_product_count
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/29.PNG)


30) For this problem, we will consider a manager an employee who has at least 1 other employee reporting to them.
Write a solution to report the ids and the names of all managers, the number of employees who report directly to them, and the average age of the reports rounded to the nearest integer.
Return the result table ordered by employee_id.

```sql
with managers as 
(
select
    e2.employee_id,
    e2.name,
    count(e1.employee_id) as reports_count,
    avg(e1.age) as average_age
from Employees e1
    join Employees e2
        on e1.reports_to = e2.employee_id
group by 
    e2.employee_id,
    e2.name
)

select
    employee_id,
    name,
    reports_count,
    round(average_age) as average_age
from managers
order by employee_id asc
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/30.PNG)


31) Employees can belong to multiple departments. When the employee joins other departments, they need to decide which department is their primary department. Note that when an employee belongs to only one department, their primary column is 'N'.
Write a solution to report all the employees with their primary department. For employees who belong to one department, report their only department.

```sql
select   
    distinct employee_id, 
    department_id
from Employee
where primary_flag = 'Y'
   or (primary_flag = 'N' and employee_id in 
    (select employee_id 
       from Employee
       group by  employee_id
       having count(department_id) = 1)
    and employee_id not in (
       select employee_id
       from Employee
       where primary_flag = 'Y'
   ))
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/31.PNG)


32) Report for every three line segments whether they can form a triangle.

```sql
select  
    x,
    y,
    z,
    case
        when x + y > z and
        y + z > x and
        z + x > y 
        then 'Yes'
        else 'No'
        end as triangle
from Triangle
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/32.PNG)


33) Find all numbers that appear at least three times consecutively.

```sql
with numbers as 
(
select
    id,
    num,
    lead(num, 1) over (order by id) as lead1,
    lead(num, 2) over (order by id) as lead2
from Logs
)

select
    id as ConsecutiveNums
from numbers
where 
    num = lead1 and
    lead1 = lead2
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/33.PNG)


34) Write a solution to find the prices of all products on 2019-08-16. Assume the price of all products before any change is 10.

```sql
SELECT
    product_id,
    COALESCE(new_price, 10) AS price
FROM 
    (
        SELECT DISTINCT
            product_id
        FROM
            Products
    ) AS UniqueProducts
    LEFT JOIN
    (
        SELECT 
            product_id,
            new_price
        FROM
            Products
            JOIN
            (
                SELECT
                    product_id,
                    max(change_date) AS change_date
                FROM 
                    Products
                WHERE
                    change_date <= '2019-08-16'
                GROUP BY
                    product_id
            ) AS LastChangeDate
            USING(product_id, change_date)
    ) AS LastChangedPrice
    USING(product_id)
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/34.PNG)


35) There is a queue of people waiting to board a bus. However, the bus has a weight limit of 1000 kilograms, so there may be some people who cannot board.
Write a solution to find the person_name of the last person that can fit on the bus without exceeding the weight limit.
The test cases are generated such that the first person does not exceed the weight limit.

```sql
with CumulativeWeights as 
(
select 
    person_id, 
    person_name, 
    weight, 
    turn,
    sum(weight) over (order by turn) as cumulative_weight
from Queue
)
select 
    person_name
from CumulativeWeights
where cumulative_weight <= 1000
order by cumulative_weight desc
limit 1;
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/35.PNG)


36) Write a solution to calculate the number of bank accounts for each salary category. The salary categories are:
•	"Low Salary": All the salaries strictly less than $20000.
•	"Average Salary": All the salaries in the inclusive range [$20000, $50000].
•	"High Salary": All the salaries strictly greater than $50000.
The result table must contain all three categories. If there are no accounts in a category, return 0.

```sql
select 'Low Salary' as category, 
       count(case when income < 20000 then 1 else null end) as accounts_count
from Accounts
union all
select 'Average Salary', 
       count(case when income >= 20000 and income <= 50000 then 1 else null end)
from Accounts
union all
select 'High Salary', 
       count(case when income > 50000 then 1 else null end)
from Accounts;
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/36.PNG)


37) Find the IDs of the employees whose salary is strictly less than $30000 and whose manager left the company. When a manager leaves the company, their information is deleted from the Employees table, but the reports still have their manager_id set to the manager that left.
Return the result table ordered by employee_id.

```sql
select
    employee_id
from Employees
where salary < 30000 and 
    manager_id not in (select employee_id from Employees)
order by employee_id
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/37.PNG)


38) Exchange Seats
Write a solution to swap the seat id of every two consecutive students. If the number of students is odd, the id of the last student is not swapped.
Return the result table ordered by id in ascending order.

```sql
select 
  (case when id % 2 = 1 and id = (select max(id) from Seat) then id when id % 2 = 1 then id + 1 when id % 2 = 0 then id - 1 end) as id, 
  student 
from Seat 
order by id;
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/39.PNG)


39) Write a solution to:
•	Find the name of the user who has rated the greatest number of movies. In case of a tie, return the lexicographically smaller user name.
•	Find the movie name with the highest average rating in February 2020. In case of a tie, return the lexicographically smaller movie name.

```sql
(select u.name as results
from MovieRating m join users u on u.user_id = m.user_id
group by u.name
order by count(u.name) desc, u.name
limit 1)

union all

(select m.title as results
from movies m join movierating mr on m.movie_id = mr.movie_id
where mr.created_at >= '2020-02-01' and mr.created_at <= '2020-02-28'
group by m.title
order by avg(mr.rating) desc, m.title
limit 1)
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/39.PNG)


40) You are the restaurant owner and you want to analyze a possible expansion (there will be at least one customer every day).
Compute the moving average of how much the customer paid in a seven days window (i.e., current day + 6 days before). average_amount should be rounded to two decimal places.
Return the result table ordered by visited_on in ascending order.

```sql
with temp as
(
select 
  distinct visited_on from Customer where (visited_on - (select min(visited_on) from Customer)) >= 6), 
  temp2 as (
            select 
              c.*, 
              t.visited_on as vo from Customer c join temp t on t.visited_on - c.visited_on between 0 and 6) select vo as visited_on, 
              sum(amount) as amount, 
              round(sum(amount) / 7::numeric, 2
            ) as average_amount 
from temp2 
group by vo;
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/40.PNG)


41) Write a solution to find the people who have the most friends and the most friends number.
The test cases are generated so that only one person has the most friends.

```sql
select 
  accepter_id id, 
  count(accepter_id) num from ( select accepter_id from requestaccepted union all select requester_id accepter_id from requestaccepted ) a 
group by accepter_id 
order by num desc 
limit 1;
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/41.PNG)


42) Write a solution to report the sum of all total investment values in 2016 tiv_2016, for all policyholders who:
•	have the same tiv_2015 value as one or more other policyholders, and
•	are not located in the same city as any other policyholder (i.e., the (lat, lon) attribute pairs must be unique).
Round tiv_2016 to two decimal places.

```sql
select 
  round(sum(tiv_2016)::numeric, 2) as tiv_2016 
from insurance 
where (tiv_2015) in (select tiv_2015 from insurance 
group by tiv_2015 having count(*) > 1) and (lat, lon) in (select lat, lon from insurance group by lat, lon having count(*) = 1);
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/42.PNG)


43) A company's executives are interested in seeing who earns the most money in each of the company's departments. A high earner in a department is an employee who has a salary in the top three unique salaries for that department.
Write a solution to find the employees who are high earners in each of the departments.
Return the result table in any order.

```sql
with rankedEmployeeByDep as (
    select 
            emp.id,
            emp.name as employeeName,
            dep.name as departmentName,
            emp.salary as salary,
            dense_rank() over(partition by emp.departmentId order by emp.salary desc) as rank 
        from Employee as emp
            left join Department as dep on dep.id = emp.departmentId
) select result.departmentName as Department, result.employeeName as Employee, result.salary as Salary
from rankedEmployeeByDep as result
where result.rank <=3
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/43.PNG)


44) Write a solution to fix the names so that only the first character is uppercase and the rest are lowercase.
Return the result table ordered by user_id.

```sql
select
    user_id, 
    upper(substring(name from 1 for 1)) || lower(substring(name from 2)) as name 
from Users 
order by user_id
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/44.PNG)


45) Write a solution to find the patient_id, patient_name, and conditions of the patients who have Type I Diabetes. Type I Diabetes always starts with DIAB1 prefix.

```sql
select
    patient_id,
    patient_name,
    conditions
from Patients
where 
    conditions like 'DIAB1%' or 
          conditions like '% DIAB1%'
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/45.PNG)


46) Write a solution to delete all duplicate emails, keeping only one unique email with the smallest id.
For SQL users, please note that you are supposed to write a DELETE statement and not a SELECT one.
For Pandas users, please note that you are supposed to modify Person in place.
After running your script, the answer shown is the Person table. The driver will first compile and run your piece of code and then show the Person table.
The final order of the Person table does not matter.

```sql
delete from Person
where id not in 
(
    select min(id)
    from Person
    group by email
);
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/46.PNG)


47) Write a solution to find the second highest distinct salary from the Employee table. If there is no second highest salary, return null (return None in Pandas).

```sql
with distinct_salaries as
(
select
    distinct salary,
    id
from Employee 
),
ordered_salaries as
(
select
    salary
from distinct_salaries
order by salary desc
limit 2
)

select
    salary as SecondHighestSalary
from ordered_salaries
order by salary asc
limit 1
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/47.PNG)


48) Write a solution to find for each date the number of different products sold and their names.
The sold products names for each date should be sorted lexicographically.
Return the result table ordered by sell_date.

```sql
select
    sell_date,
    count(distinct product) as num_sold,
    string_agg(distinct product, ',' order by product) as products
from Activities
group by sell_date
order by sell_date
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/48.PNG)


49) Write a solution to get the names of products that have at least 100 units ordered in February 2020 and their amount.

```sql
select
    product_name,
    sum(unit) as unit
from Orders
    join Products
    using(product_id)
where 
    to_char(order_date, 'YYYY-MM') = '2020-02'
group by 
    product_id,
    product_name
having sum(unit) >= 100
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/49.PNG)


50) Write a solution to find the users who have valid emails.
A valid e-mail has a prefix name and a domain where:
•	The prefix name is a string that may contain letters (upper or lower case), digits, underscore '_', period '.', and/or dash '-'. The prefix name must start with a letter.
•	The domain is '@leetcode.com'.

```sql
select
  *
from Users
where mail ~ '^[a-zA-Z]+[a-zA-Z0-9_.-]*@leetcode\.com$'
```

![alt text](https://github.com/Omerguleryuz/sql-leetcode-50questions/blob/main/Screenshots/50.PNG)












































