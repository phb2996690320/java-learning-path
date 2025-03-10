---
url: https://blog.csdn.net/zhou_zzzzhou/article/details/135122962
title: 【学习笔记】LeetCode SQL 刷题（高频 50 基础版 + 进阶版）_力扣数据库刷题 sql-CSDN 博客
date: 2025-03-10 15:45:33
tag: 
summary: 
---
## 一、高频 SQL50 题 (基础版)

<table><thead><tr><th>题目考查类型</th><th>题号</th></tr></thead><tbody><tr><td>查询</td><td>1-5</td></tr><tr><td>连接</td><td>6-14</td></tr><tr><td>聚合函数</td><td>15-22</td></tr><tr><td>排序和分组</td><td>23-29</td></tr><tr><td>高级查询和连接</td><td>30-36</td></tr><tr><td>子查询</td><td>37-43</td></tr><tr><td>高级字符串函数 / 正则表达式 / 子句</td><td>44-50</td></tr></tbody></table>

1.  **[1757. 可回收且低脂的产品](https://leetcode.cn/problems/recyclable-and-low-fat-products/)**

```
SELECT product_id
FROM Products 
WHERE low_fats='Y' and recyclable='Y';

```

2.  **[584. 寻找用户推荐人](https://leetcode.cn/problems/find-customer-referee/)**

```
select name
from Customer
where referee_id != 2 or referee_id is null

```

3.  **[595. 大的国家](https://leetcode.cn/problems/big-countries/)**

```
select name,population,area
from World
where area>=3000000 or population>=25000000

```

4.  **[1148. 文章浏览 I](https://leetcode.cn/problems/article-views-i/)**

```
select distinct author_id as id
from Views
where author_id=viewer_id
order by id

```

5.  **[1683. 无效的推文](https://leetcode.cn/problems/invalid-tweets/)**

```
select tweet_id
from Tweets
where length(content)>15

```

6.  **[1378. 使用唯一标识码替换员工 ID](https://leetcode.cn/problems/replace-employee-id-with-the-unique-identifier/)**

```
select unique_id,name
from Employees left join  EmployeeUNI
on EmployeeUNI.ID=Employees.ID

```

7.  **[1068. 产品销售分析 I](https://leetcode.cn/problems/product-sales-analysis-i/)**

```
select product_name,year,price
from Sales join Product
on Sales.product_id = Product.product_id

```

8.  **[1581. 进店却未进行过交易的顾客](https://leetcode.cn/problems/customer-who-visited-but-did-not-make-any-transactions/)**

```
select customer_id, count(*) as count_no_trans
from Visits v left join Transactions t on
v.visit_id = t.visit_id
where transaction_id is null
group by customer_id

```

9.  **[197. 上升的温度](https://leetcode.cn/problems/rising-temperature/)**

```
select w2.id id
from Weather w1 join Weather w2
on w1.recordDate = w2.recordDate-interval 1 day
where w1.Temperature < w2.Temperature

```

10.  **[1661. 每台机器的进程平均运行时间](https://leetcode.cn/problems/average-time-of-process-per-machine/)**

```
# 运行时间17%
select start.machine_id, round(avg(end.timestamp-start.timestamp),3) processing_time
from
(select  *
from Activity
where activity_type ='start') as start
join
(select   *
from Activity
where activity_type ='end') as end
on
start.machine_id=end.machine_id and start.process_id=end.process_id
group by start.machine_id

```

```
# 运行时间45%
select machine_id,
round((2*sum(timestamp*(case when activity_type = 'start' then -1 else 1 end)))/count(activity_type),3) as processing_time
from Activity
group by machine_id

```

11.**[577. 员工奖金](https://leetcode.cn/problems/employee-bonus/)**

```
select name,bonus
from Employee left join Bonus
on Employee.empId = Bonus.empid
where bonus<1000 or bonus is null

```

12.**[1280. 学生们参加各科测试的次数](https://leetcode.cn/problems/students-and-examinations/)**

```
select a.student_id,a.student_name,a.subject_name,ifnull(attended_exams,0) attended_exams
from

(select *
from subjects join students) a
left join

(select *,count(e.student_id) as attended_exams
from Examinations e
group by e.student_id,e.subject_name) b

on a.student_id = b.student_id
and a.subject_name = b.subject_name

order by a.student_id,a.subject_name

```

13.**[570. 至少有 5 名直接下属的经理](https://leetcode.cn/problems/managers-with-at-least-5-direct-reports/)**

```
select e2.name name
from Employee e2
left join Employee e1
on  e1.managerId=e2.id
group by e2.id
having count(*)>=5

```

14.**[1934. 确认率](https://leetcode.cn/problems/confirmation-rate/)**

```
select  s.user_id,round(sum(if(action='confirmed',1,0))/count(*),2) confirmation_rate
from Signups  s
left join Confirmations c
on s.user_id = c.user_id
group by s.user_id

```

15.**[620. 有趣的电影](https://leetcode.cn/problems/not-boring-movies/)**

```
select *
from cinema
where description!='boring' and id%2!=0
order by rating desc

```

16.**[1251. 平均售价](https://leetcode.cn/problems/average-selling-price/)**

```
select p.product_id,ifnull(round((sum(price*units)/sum(units)),2),0) as average_price
from Prices  p left join UnitsSold u
on p.product_id = u.product_id
and u.purchase_date between p.start_date and p.end_date
group by p.product_id

```

17.**[1075. 项目员工 I](https://leetcode.cn/problems/project-employees-i/)**

```
select project_id,round(avg(experience_years),2) as  average_years
from Project p left join Employee e
on p.employee_id = e.employee_id
group by project_id

```

18.**[1633. 各赛事的用户注册率](https://leetcode.cn/problems/percentage-of-users-attended-a-contest/)**

```
select contest_id,round(count(contest_id)/(select count(*) from Users)*100,2) as percentage
from Register r left join Users u
on r.user_id = u.user_id
group by contest_id
order by percentage desc,contest_id

```

19.**[1211. 查询结果的质量和占比](https://leetcode.cn/problems/queries-quality-and-percentage/)**

```
select query_name,round(avg(rating/position),2) as quality,
round((100*sum(case when rating<3 then 1 else 0 end)/count(*)),2)  as poor_query_percentage
from Queries
group by query_name
having query_name is not null

```

20.**[1193. 每月交易 I](https://leetcode.cn/problems/monthly-transactions-i/)**

```
select left(trans_date,7) as month,
country,count(*) as trans_count,
sum(case when state='approved' then 1 else 0 end) as approved_count,
sum(amount) as trans_total_amount,
sum((case when state='approved' then 1 else 0 end)*amount) as approved_total_amount
from Transactions
group by month,country

```

21.**[1174. 即时食物配送 II](https://leetcode.cn/problems/immediate-food-delivery-ii/)**

```
select round((sum(case when customer_pref_delivery_date=order_date then 1 else 0 end)*100/count(*)),2) as immediate_percentage
from
(select customer_id,min(order_date)  as order_date,min(customer_pref_delivery_date) as customer_pref_delivery_date
from Delivery
group by customer_id) as first_order

```

22.**[550. 游戏玩法分析 IV](https://leetcode.cn/problems/game-play-analysis-iv/)**

```
# 卡了很久最小时间
select  round(count(*)/(select count(distinct player_id) from Activity),2) as fraction
from
((select player_id,min(event_date) as event_date
from Activity 
group by player_id) as a1

join Activity a2
on a1.player_id=a2.player_id
and a1.event_date=a2.event_date - interval 1 day)

```

23.**[2356. 每位教师所教授的科目种类的数量](https://leetcode.cn/problems/number-of-unique-subjects-taught-by-each-teacher/)**

```
select teacher_id,count(distinct subject_id) as cnt
from teacher
group by teacher_id

```

24.**[1141. 查询近 30 天活跃用户数](https://leetcode.cn/problems/user-activity-for-the-past-30-days-i/)**

```
select activity_date as day,count(distinct user_id) as active_users
from Activity
group by activity_date
having activity_date between ("2019-07-27"- interval 29 day) and "2019-07-27"

```

25.**[1084. 销售分析 III](https://leetcode.cn/problems/sales-analysis-iii/)**

```
# 注意sum=count的用法，用于“所有都是……”的场景
select s.product_id,product_name
from Sales s left join Product p
on s.product_id=p.product_id
group by  s.product_id
having sum(s.sale_date between "2019-01-01" and "2019-03-31")=count(*)

```

26.  **[596. 超过 5 名学生的课](https://leetcode.cn/problems/classes-more-than-5-students/)**

```
select class
from Courses
group by class
having count(*)>=5

```

27.**[1729. 求关注者的数量](https://leetcode.cn/problems/find-followers-count/)**

```
select user_id,count(*) as followers_count
from Followers
group by user_id
order by user_id

```

28.**[619. 只出现一次的最大数字](https://leetcode.cn/problems/biggest-single-number/)**

```
select max(num) num
from
(select num
from MyNumbers
group by num
having count(*)=1) num1

```

29.**[1045. 买下所有产品的客户](https://leetcode.cn/problems/customers-who-bought-all-products/)**

```
select customer_id
from Customer
group by customer_id
having  count(distinct product_key)= (select count(*) from Product)

```

30.**[1731. 每位经理的下属员工数量](https://leetcode.cn/problems/the-number-of-employees-which-report-to-each-employee/)**

```
select  e2.employee_id,e2.name,count(*) as reports_count,
round(avg(e1.age),0) as average_age
from Employees e1 left join Employees e2
on e1.reports_to = e2.employee_id
group by e2.employee_id
having e2.employee_id is not null
order by employee_id

```

31.**[1789. 员工的直属部门](https://leetcode.cn/problems/primary-department-for-each-employee/)**

```
(select employee_id,department_id
from Employee
where primary_flag ='Y')
UNION
(select employee_id,department_id
from Employee
group by  employee_id
having count(*)=1)
order by employee_id

```

32.**[610. 判断三角形](https://leetcode.cn/problems/triangle-judgement/)**

```
select *,
(case when (x+y>z and x+z>y and z+y>x) then "Yes"
else "No"
end) as triangle
from Triangle

```

33.**[180. 连续出现的数字](https://leetcode.cn/problems/consecutive-numbers/)**

```
select distinct L1.num as ConsecutiveNums
from Logs L1
join Logs L2 on L1.id=L2.id-1
join Logs L3 on L2.id=L3.id-1
where L1.num=L2.num and L2.num=L3.num

```

34.**[1164. 指定日期的产品价格](https://leetcode.cn/problems/product-price-at-a-given-date/)**

```
select product_id, new_price as price
from Products
where (product_id,change_date) in
(select product_id,max(change_date)
from Products
where change_date<="2019-08-16"
group by product_id)

union

select product_id,10 as price
from Products
where product_id  not in (select product_id from Products where change_date<="2019-08-16")

```

⭐35.**[1204. 最后一个能进入巴士的人](https://leetcode.cn/problems/last-person-to-fit-in-the-bus/)**

```
select q1.person_name
from Queue q1
join Queue q2 on q1.turn>=q2.turn
group by q1.person_id
having sum(q2.weight)<=1000
order by q1.turn desc limit 1

```

36.**[1907. 按分类统计薪水](https://leetcode.cn/problems/count-salary-categories/)**

```
select "Low Salary" category,count(*) accounts_count
from Accounts
where income<20000

union

select "Average Salary" category,count(*) accounts_count
from Accounts
where income between 20000 and 50000

union

select "High Salary" category,count(*) accounts_count
from Accounts
where income>50000

```

37.**[1978. 上级经理已离职的公司员工](https://leetcode.cn/problems/employees-whose-manager-left-the-company/)**

```
select employee_id
from Employees
where salary<30000 and  manager_id not in (select employee_id from Employees)
order by employee_id

```

38.**[626. 换座位](https://leetcode.cn/problems/exchange-seats/)**

```
select
(case when id%2!=0 and id!=(select count(*) from Seat) then id+1
      when id%2=0  then id-1
      else id end) as id,student
from Seat
order by id

```

39.**[1341. 电影评分](https://leetcode.cn/problems/movie-rating/)**

```
(select name as results
from Users u
join MovieRating r1 on u.user_id = r1.user_id 
group by u.user_id
order by count(*) desc,name limit 1)

union all

(select title as results
from Movies m
join  MovieRating r2 on m.movie_id = r2.movie_id and left(r2.created_at,7) = "2020-02"
group by r2.movie_id 
order by avg(rating) desc,title limit 1)

```

⭐⭐40.**[1321. 餐馆营业额变化增长](https://leetcode.cn/problems/restaurant-growth/)**

```
#注意join时where的用法以及分组之后avg函数的使用
select a.visited_on,sum(c.amount) as amount,round((sum(c.amount))/7,2) as average_amount
from
(select distinct visited_on 
from Customer) as a
left join  customer c
on (c.visited_on>=a.visited_on - interval 6 day) and (c.visited_on<=a.visited_on)
where a.visited_on>=(select min(visited_on) from customer)+6
group by a.visited_on
order by a.visited_on

```

41.**[602. 好友申请 II ：谁有最多的好友](https://leetcode.cn/problems/friend-requests-ii-who-has-the-most-friends/)**

```
select a.id,count(*) as num
from
(select requester_id as id from RequestAccepted r1
union all
select accepter_id as id from RequestAccepted r2) as a
group by id
order by num desc limit 1

```

42.**[585. 2016 年的投资](https://leetcode.cn/problems/investments-in-2016/)**

```
select round(sum(tiv_2016),2) tiv_2016
from Insurance
where tiv_2015 in
(select tiv_2015 from Insurance 
group by tiv_2015 having count(*)>1)

and concat(lat, lon)  in 
(select concat(lat, lon) 
from Insurance 
group by concat(lat, lon) 
having count(*)=1)

```

⭐⭐⭐43.**[185. 部门工资前三高的所有员工](https://leetcode.cn/problems/department-top-three-salaries/)**

```
select d.name as  Department,e.name  as Employee,e.salary
from Employee e left join Department d
on e.departmentId=d.id
where e.id in
(select e1.id
from Employee e1 left join  Employee e2
on e1.departmentId=e2.departmentId and e1.salary<e2.salary
group by e1.id
having count(distinct e2.salary)<=2)

```

44.**[1667. 修复表中的名字](https://leetcode.cn/problems/fix-names-in-a-table/)**

```
select user_id,concat(upper(left(name,1)),lower(SUBSTRING(name,2))) name
from Users
order by user_id

```

45.**[1527. 患某种疾病的患者](https://leetcode.cn/problems/patients-with-a-condition/)**

```
select *
from Patients
where conditions like "DIAB1%" or conditions like "% DIAB1%"

```

46.**[196. 删除重复的电子邮箱](https://leetcode.cn/problems/delete-duplicate-emails/)**

```
delete from
Person
where id not in
(select id from(select min(id) id
from Person
group by email) as a)

```

47.**[176. 第二高的薪水](https://leetcode.cn/problems/second-highest-salary/)**

```
select ifnull((
select distinct salary
from Employee
order by  salary desc
limit 1 offset 1),null) as SecondHighestSalary

```

48.**[1484. 按日期分组销售产品](https://leetcode.cn/problems/group-sold-products-by-the-date/)**

```
select sell_date,count(distinct product) as num_sold,
group_concat(distinct product order by product SEPARATOR ',') as products
from Activities
group by sell_date
order by sell_date

```

49.**[1327. 列出指定时间段内所有的下单产品](https://leetcode.cn/problems/list-the-products-ordered-in-a-period/)**

```
select product_name,sum(unit) as unit
from Products p join Orders o
on p.product_id=o.product_id and  left(o.order_date,7)="2020-02"
group by product_name
having sum(unit)>=100

```

50.**[1517. 查找拥有有效邮箱的用户](https://leetcode.cn/problems/find-users-with-valid-e-mails/)**

```
SELECT user_id, name, mail
FROM Users
WHERE mail REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*\\@leetcode\\.com$';

```

## 二、高频 SQL50 题（进阶版）

<table><thead><tr><th>题目考查类型</th><th>题号</th></tr></thead><tbody><tr><td>查询</td><td>1-5</td></tr><tr><td>连接</td><td>6-11</td></tr><tr><td>聚合函数</td><td>12-19</td></tr><tr><td>排序和分组</td><td>20-26</td></tr><tr><td>高级查询和连接</td><td>27-35</td></tr><tr><td>子查询</td><td>36-43</td></tr><tr><td>高级字符串函数 / 正则表达式 / 子句</td><td>44-50</td></tr></tbody></table>

1.****[1821. 寻找今年具有正收入的客户](https://leetcode.cn/problems/find-customers-with-positive-revenue-this-year/)****

```
select customer_id
from Customers
where year=2021 and revenue>0

```

2.****183. 从不订购的客户****

```
select name as Customers
from Customers
where id not in
(select customerId from Orders)

```

3.  ****1873. 计算特殊奖金****

```
select employee_id,salary*(case when employee_id%2!=0 and left(name,1)!="M" then 1 else 0 end) as bonus
from Employees
order by employee_id

```

4.****1398. 购买了产品 A 和产品 B 却没有购买产品 C 的顾客****

```
# 方法一
select customer_id,customer_name
from Customers
where customer_id in
(select customer_id
from Orders
where product_name="A" or product_name="B"
group by customer_id
having count(distinct product_name)=2)

and customer_id not in
(select customer_id
from Orders
where product_name="C"
group by customer_id)

# 方法二：巧用sum
select customer_id,customer_name
from Customers
where customer_id in
(select customer_id
from Orders
group by customer_id
having sum(product_name="A")*sum(product_name="B")>0
and sum(product_name="C")=0)

```

5.****1112. 每位学生的最高成绩****

```
select student_id,min(course_id) course_id,grade
from Enrollments
where (student_id,grade) in
(
select student_id,max(grade) grade
from Enrollments 
group by student_id)
# 注意这里的group by，因为取了min，所有要group by
group by student_id
order by student_id,course_id


```

6.****175. 组合两个表****

```
select firstName,lastName,city,state
from Person p left join Address a on p.PersonId = a.personId

```

7.  ****[1607. 没有卖出的卖家](https://leetcode.cn/problems/sellers-with-no-sales/)****

```
select seller_name
from Seller s
where seller_id not in 
(select seller_id
from  Orders 
where left(sale_date,4)="2020")
order by seller_name

```

8.  ****1407. 排名靠前的旅行者****

```
select name,ifnull(sum(r.distance),0) travelled_distance
from Users u
left join Rides r on u.id=r.user_id
group by u.id
order by  travelled_distance desc,name

```

9.****607. 销售员****

```
select name
from SalesPerson
where sales_id not in
(select sales_id
from Orders o  join Company c
on o.com_id = c.com_id and c.name="RED" )

```

10.****1440. 计算布尔表达式的值****

```
select e.*,
(case when operator=">" and v1.value>v2.value then "true"
      when operator="<" and v1.value<v2.value then "true"
      when operator="=" and v1.value=v2.value then "true"
else "false"
end) as value
from Expressions e
left join Variables v1 on v1.name=e.left_operand
left join Variables v2 on v2.name=e.right_operand

```

11.****1212. 查询球队积分****

```
select t.team_id,t.team_name,
sum(case when m.host_team = t.team_id and host_goals>guest_goals then 3
      when m.guest_team = t.team_id and host_goals<guest_goals then 3
      when   host_goals=guest_goals then 1
else 0 
end) as num_points
from Matches m
right join Teams t  on m.host_team = t.team_id or m.guest_team=t.team_id
group by t.team_id
order by num_points desc,team_id

```

12.****1890. 2020 年最后一次登录****

```
select user_id,max(time_stamp) as last_stamp
from Logins
where left(time_stamp,4)='2020'
group by user_id

```

13.****511. 游戏玩法分析 I****

```
select player_id,min(event_date) as first_login
from Activity
group by player_id

```

14.****1571. 仓库经理****

```
select name as warehouse_name,sum(units*Width*Length*Height) as volume
from Warehouse w left join Products p
on w.product_id=p.product_id
group by w.name

```

15.****586. 订单最多的客户****

```
select customer_number
from Orders
group by customer_number
order by count(customer_number) desc limit 1

```

16.****1741. 查找每个员工花费的总时间****

```
select event_day as day,emp_id,sum(out_time-in_time) as total_time
from Employees
group by event_day,emp_id

```

17.****1173. 即时食物配送 I****

```
select round(100*sum(case when order_date=customer_pref_delivery_date then 1
        else 0
        end)/(count(delivery_id)),2) as immediate_percentage
from Delivery

```

18.****1445. 苹果和桔子****

```
select sale_date,sum(case when fruit="apples" then sold_num
                          else -sold_num
                          end ) as diff 
from Sales
group by sale_date

```

19.****1699. 两人之间的通话次数****

```
# 先找到适合的顺序
select 
(case when from_id<to_id then from_id  else to_id end)as person1,
(case when from_id<to_id then to_id  else from_id end)as person2,
count(*) as call_count,
sum(duration) as total_duration
from Calls
group by person1,person2

```

20.****1587. 银行账户概要 II****

```
select  u.name as NAME,sum(t.amount) as BALANCE
from Transactions t left join Users u on t.account=u.account
group by t.account
having sum(amount)>10000

```

21.****182. 查找重复的电子邮箱****

```
select email Email
from Person
group by email
having count(*)>=2

```

22.****1050. 合作过至少三次的演员和导演****

```
select actor_id,director_id
from ActorDirector
group by actor_id,director_id
having count(*)>=3

```

23.****1511. 消费者下单频率****

```
select o.customer_id,c.name
from Orders o  join Product p on o.product_id=p.product_id
               join Customers c on o.customer_id=c.customer_id
group by customer_id
having sum(case when left(o.order_date,7)="2020-06" then quantity*price else 0 end)>=100
and sum(case when left(o.order_date,7)="2020-07" then quantity*price else 0 end)>=100

```

24.****1693. 每天的领导和合伙人****

```
select date_id,make_name,count(distinct lead_id) unique_leads,count(distinct partner_id) unique_partners
from DailySales
group by date_id,make_name

```

25.****1495. 上月播放的儿童适宜电影****

```
select distinct title
from Content c left join TVProgram t on c.content_id =t.content_id 
where c.Kids_content='Y' and left(t.program_date,7)="2020-06" and content_type="Movies"

```

26.****1501. 可以放心投资的国家****

```
select co.name as  country
from Person p join Country co on left(p.phone_number,3)=co.country_code
              join Calls ca   on p.id=ca.caller_id or p.id=ca.callee_id
group by co.country_code
having avg(duration)>(select avg(duration) from Calls)

```

27.****603. 连续空余座位****

```
select distinct c1.seat_id
from Cinema c1 join Cinema c2
on abs(c1.seat_id - c2.seat_id)=1
where c1.free=1 and c2.free=1
order by  c1.seat_id

```

28.****1795. 每个产品在不同商店的价格****

```
select product_id,"store1" as store,store1 price
from Products
where store1 is not null
union all
select product_id,"store2" as store,store2 price
from Products
where store2 is not null
union all
select product_id,"store3" as store,store3 price
from Products
where store3 is not null

```

29.****613. 直线上的最近距离****

```
select min(abs(p1.x-p2.x)) as shortest
from Point p1  join Point p2 on p1.x!=p2.x

```

30.****1965. 丢失信息的雇员****

```
select employee_id
from Employees
where employee_id  not in (select employee_id from Salaries)
union
select employee_id
from Salaries
where employee_id  not in (select employee_id from Employees)
order by  employee_id

```

31.****1264. 页面推荐****

```
select distinct page_id as recommended_page
from Likes
where user_id in(
select user2_id user_id
from Friendship
where user1_id ="1"
union
select user1_id user_id
from Friendship
where user2_id ="1")
and page_id not in (select page_id from Likes where user_id=1)

```

32.****608. 树节点****

```
select id,(case when p_id is null then "Root"
                when id not in (select p_id from Tree where  p_id is not  null)then  "Leaf"
            else "Inner"
            end )type
from Tree

```

33.****534. 游戏玩法分析 III****

```
SELECT a2.player_id,a2.event_date,sum(a1.games_played) as games_played_so_far
FROM Activity a1 left join Activity a2  on a1.player_id=a2.player_id and a2.event_date>=a1.event_date
group by a2.event_date,player_id

```

34.****1783. 大满贯数量****

```
select p.player_id,p.player_name,sum(c.Wimbledon=p.player_id)+sum(c.Fr_open=p.player_id)+sum(c.US_open=p.player_id)+sum(c.Au_open=p.player_id)grand_slams_count
from Players p,Championships c
group by p.player_id
having grand_slams_count>0

```

35.****1747. 应该被禁止的 Leetflex 账户****

```
select account_id
from(select account_id,login,logout,lead (login,1) over() as ll,nums
from(select *,row_number() over (partition by account_id order by login) as nums
from loginfo)aaa)bbb
where nums=1 and ll between login and logout

```

36.**[1350. 院系无效的学生](https://leetcode.cn/problems/students-with-invalid-departments/)**

```
select id,name
from Students s 
where s.department_id not in (select id from Departments)

```

37.**[1303. 求团队人数](https://leetcode.cn/problems/find-the-team-size/)**

```
select e1.employee_id,count(*) as team_size
from Employee e1 left join Employee e2 on e1.team_id=e2.team_id
group by e1.employee_id

```

38.**[512. 游戏玩法分析 II](https://leetcode.cn/problems/game-play-analysis-ii/)**

```
select  player_id,device_id
from Activity
where (player_id,event_date) in
(select  player_id,min(event_date)
from Activity
group by player_id)

```

39.**[184. 部门工资最高的员工](https://leetcode.cn/problems/department-highest-salary/)**

```
select d.name Department,e.name Employee,e.salary
from  Employee e left join Department d on e.departmentId=d.id 
where (e.departmentId,e.salary) in (select departmentId,max(salary)
from  Employee  
group by departmentId )

```

40.**[1549. 每件商品的最新订单](https://leetcode.cn/problems/the-most-recent-orders-for-each-product/)**

```
select p.product_name,o.product_id,o.order_id,o.order_date
from Orders o left join Products p on o.product_id=p.product_id
where (o.product_id,o.order_date) in
(select product_id,max(order_date)
from Orders
group by product_id)
order by p.product_name,o.product_id,o.order_id

```

41.**[1532. 最近的三笔订单](https://leetcode.cn/problems/the-most-recent-three-orders/)**

```
select c.name as customer_name,c.customer_id,o2.order_id,o2.order_date
from Orders o1 left join Orders o2 on o1.customer_id=o2.customer_id and o1.order_date>=o2.order_date 
                left join Customers c on o1.customer_id=c.customer_id
group by o2.order_id
#比我日期更近的order_date不超过3个，所以是前三
having count(o1.order_date)<=3
order by c.name,c.customer_id,o2.order_date desc

```

42.**[1831. 每天的最大交易](https://leetcode.cn/problems/maximum-transaction-each-day/)**

```
select transaction_id
from Transactions
where (day,amount) in(select day,max(amount)
from Transactions
group by day)
order by transaction_id

```

43.****1077. 项目员工 III****

```
# 解题思路同39题
select p.project_id,p.employee_id
from Project p left join Employee e on p.employee_id=e.employee_id
where (p.project_id,e.experience_years) in
(select p.project_id,max(experience_years)
from Project p left join Employee e on p.employee_id=e.employee_id
group by p.project_id)

```

44.****1285. 找到连续区间的开始和结束数字****

```
#开窗函数，还看不太懂
select min(log_id) as start_id,max(log_id) as end_id
from
(SELECT
    log_id,
    log_id - row_number() over() diff
FROM logs)as t
group by diff

```

45.****1596. 每位顾客最经常订购的商品****

```
select o.customer_id,o.product_id,p.product_name
from
(select customer_id,product_id,
rank() over(partition by  customer_id order by count(product_id) desc)rnk 
from Orders
group by customer_id,product_id)o
join products p on o.product_id=p.product_id
where rnk=1

```

46.****1709. 访问日期之间最大的空档期****

```
select user_id,max(datediff(next_day,visit_date)) as  biggest_window
from
(select user_id,visit_date,LEAD(visit_date,1,'2021-1-1') over(partition by user_id order by visit_date)as next_day
from  UserVisits)as tmp
group by user_id
order by user_id

```

47.****1270. 向公司 CEO 汇报工作的所有人****

```
select distinct employee_id
from 
(select employee_id from Employees where manager_id=1
union all
(select employee_id from Employees
where manager_id  in (select employee_id from Employees where manager_id=1 ))
union all
select employee_id from Employees
where manager_id  in (select employee_id from Employees
where manager_id  in (select employee_id from Employees where manager_id=1 )))e
where employee_id!=1

SELECT e1.employee_id
FROM Employees e1
JOIN Employees e2 ON e1.manager_id = e2.employee_id
JOIN Employees e3 ON e2.manager_id = e3.employee_id
WHERE e1.employee_id != 1 AND e3.manager_id = 1

```

48.****1412. 查找成绩处于中游的学生****

```
select tmp.student_id,s.student_name
from
(select *,
if(dense_rank() over(partition by exam_id order by score desc)=1,1,0) d_rank,
if(dense_rank() over(partition by exam_id order by score )=1,1,0) a_rank
from Exam)tmp
left join Student s on tmp.student_id=s.student_id
group by tmp.student_id
having sum(d_rank)=0 and sum(a_rank)=0
order by tmp.student_id

```

⭐⭐⭐49.****1767. 寻找没有被执行的任务对****

```
with recursive table1 as(select task_id,subtasks_count subtask_id 
from Tasks
union all
select task_id,subtask_id-1 from table1 where  subtask_id > 1)

select task_id,subtask_id
from table1
left join Executed E using(task_id, subtask_id)
where E.task_id is null

```

⭐⭐⭐****1225. 报告系统状态的连续日期****

```
select type as period_state,min(date) as start_date,max(date) as end_date
from
(select type,date,subdate(date,row_number() over(partition by type order by date))as diff
from
(select "failed" as type,fail_date as date from Failed
union all
select "succeeded" as type,success_date  as date from Succeeded)tmp1)tmp2
where date between "2019-01-01" and "2019-12-31"
group by type,diff
order by  start_date

```