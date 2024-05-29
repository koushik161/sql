# sql
Gist of my leetcode practice problems 

#1 Recyclable and Low Fat Products 
SELECT product_id FROM Products 
WHERE low_fats = 'Y' AND recyclable = 'y'
GROUP BY product_id

#2 Find Customer Referee
SELECT name from Customer 
WHERE  referee_id <> 2 or referee_id is null

#3 Big Countries
SELECT name, population, area 
from World
where area >= 3000000 or population >= 25000000

#4 Articles views 1
select distinct author_id as id 
from Views
where author_id = viewer_id
ORDER BY 1

#5 Invalid Tweets
select tweet_id 
from tweets
where length(content)>15

#6 Replace Employee ID with the Unique Identifier
SELECT unique_id,name 
from employees e
left join employeeuni eu on e.id=eu.id

#7 Product Sales Analysis I
select product_name, year, price 
from sales s inner join product p
on s.product_id=p.product_id

#8 Customer Who Visited But Did Not Make Any Transactions
SELECT customer_id, COUNT(*) as count_no_trans
FROM Visits 
WHERE visit_id NOT IN (SELECT DISTINCT visit_id FROM Transactions)
GROUP BY customer_id

#9 Rising Temperature
SELECT w1.id 
FROM Weather w1, Weather w2
WHERE DATEDIFF(w1.recordDate, w2.recordDate) = 1
AND w1.temperature > w2.temperature

#10 Average Time of Process Per Machine
SELECT machine_id, ROUND(AVG(end - start), 3) AS processing_time
FROM 
(SELECT machine_id, process_id, 
    MAX(CASE WHEN activity_type = 'start' THEN timestamp END) AS start,
    MAX(CASE WHEN activity_type = 'end' THEN timestamp END) AS end
 FROM Activity 
  GROUP BY machine_id, process_id) AS subq
GROUP BY machine_id

#11 Employee Bonus
select a.name, b.bonus 
from employee a
left join bonus b on a.empid=b.empid
where bonus<1000
or bonus is null

#12 Students and Examinations
select stu.student_id, 
       stu.student_name,
       sub.subject_name,
       count(exam.subject_name) as attended_exams
from students stu
cross join subjects sub
left join examinations exam
on stu.student_id = exam.student_id
and sub.subject_name = exam.subject_name
group by 1,2,3
order by 1,2

#13 Managers with at Least 5 Direct Reports
select a.name
from employee a
join employee b on a.id=b.managerid
group by b.managerid
having COUNT(*)>=5

#14 Confirmation Rate
with cte as (
    select a.user_id,
        sum(case when action = 'confirmed' then 1 else 0 end) as confirmed,
        count(action) as total
    from Signups a
    left join Confirmations b
    on a.user_id = b.user_id
    group by 1
)

select user_id,
       round(coalesce(confirmed/total,0),2) as confirmation_rate
from cte
group by 1

#15 Not Boring Movies
select id,movie,description,rating
from cinema
where id%2 != 0
and description != 'boring'
order by rating  desc

#16 Average Selling Price
select a.product_id,
       coalesce(round((sum(price * units) / sum(units)), 2),0) as average_price
from Prices a
left join UNitsSold b
on a.product_id = b.product_id
and b.purchase_date between a.start_date and a.end_date
group by 1

#17 Project Employees I
select project_id,ROUND(avg(experience_years),2) as average_years
from project a
left join employee b
on a.employee_id=b.employee_id
group by project_id

#18 Percentage of Users Attended a Contest
SELECT r.contest_id,
       ROUND(COUNT(DISTINCT r.user_id) * 100 / (SELECT COUNT(DISTINCT user_id) FROM Users), 2) AS percentage
FROM Register r
GROUP BY r.contest_id
ORDER BY percentage DESC, r.contest_id ASC;

#19 Queries Quality and Percentage
select query_name,
       round(sum(rating/position) / count(query_name),2) as quality,
       round(100*sum(case when rating < 3 then 1 else 0 end) / count(query_name),2) as poor_query_percentage
from Queries
where query_name is not null
group by 1

#20 Monthly Transactions I
select left(trans_date,7) as month,
       country,
       count(id) as trans_count,
       sum(case when state = 'approved' then 1 else 0 end) as approved_count,
       sum(amount) as trans_total_amount,
       sum(case when state = 'approved' then amount else 0 end) as approved_total_amount

from Transactions
group by 1,2

#21 Immediate Food Delivery II
SELECT
    ROUND((COUNT(CASE WHEN d.order_date = d.customer_pref_delivery_date THEN 1 END) / COUNT(*)) * 100, 2)  immediate_percentage
FROM Delivery d
WHERE d.order_date = (
    SELECT
    MIN(order_date)
    FROM Delivery
    WHERE customer_id = d.customer_id
    );


#22 
     
