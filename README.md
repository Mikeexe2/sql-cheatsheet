## Shortest city name
```
SELECT CITY, LENGTH(CITY)
FROM STATION
ORDER BY LENGTH(CITY) ASC, CITY ASC
LIMIT 1;
```

## Longest city name
```
SELECT CITY, LENGTH(CITY)
FROM STATION
ORDER BY LENGTH(CITY) DESC, CITY ASC
LIMIT 1;
```

```
SELECT COUNT(CITY) - COUNT(DISTINCT CITY)
FROM STATION;
```

```
SELECT DISTINCT CITY
FROM STATION
WHERE MOD(ID, 2) = 0;
```

```
SELECT DISTINCT CITY
FROM STATION
WHERE CITY LIKE 'A%' OR CITY LIKE 'E%' OR CITY LIKE 'I%' OR CITY LIKE 'O%' OR CITY LIKE 'U%';
```

```
SELECT DISTINCT(CITY)
FROM STATION
WHERE CITY LIKE '%a'
   OR CITY LIKE '%e'
   OR CITY LIKE '%i'
   OR CITY LIKE '%o'
   OR CITY LIKE '%u';
```

```
SELECT DISTINCT CITY
FROM STATION
WHERE CITY REGEXP '^[AEIOU]'
  AND CITY REGEXP '[AEIOU]$';
```

```
SELECT DISTINCT CITY
FROM STATION
WHERE CITY NOT REGEXP '^[aeiouAEIOU].*';
```

```
SELECT SUM(CITY.Population)
FROM CITY
INNER JOIN COUNTRY ON CITY.CountryCode = COUNTRY.Code
WHERE COUNTRY.Continent = 'Asia';
```

```
SELECT city.NAME
FROM city
INNER JOIN country ON city.CountryCode = country.Code
WHERE country.continent = 'Africa';
```

```
SELECT COUNTRY.Continent, FLOOR(AVG(CITY.Population)) AS avg_city_population
FROM CITY
INNER JOIN COUNTRY ON CITY.CountryCode = COUNTRY.Code
GROUP BY COUNTRY.Continent;
```

```
SELECT
    CASE WHEN Grade < 8 THEN NULL ELSE Name END AS Name,
    Grade,
    Marks
FROM Students
JOIN Grades ON Marks BETWEEN Min_Mark AND Max_Mark
ORDER BY Grade DESC, Name ASC, Marks ASC;
```

```
SELECT
    h.hacker_id,
    h.name
FROM Hackers h
JOIN Submissions s ON h.hacker_id = s.hacker_id
JOIN Challenges c ON s.challenge_id = c.challenge_id
JOIN Difficulty d ON c.difficulty_level = d.difficulty_level
WHERE s.score = d.score
GROUP BY h.hacker_id, h.name
HAVING COUNT(s.submission_id) > 1
ORDER BY COUNT(s.submission_id) DESC, hacker_id;
```

## better approach?
```
select h.hacker_id, h.name from Hackers h
inner join Submissions s on s.hacker_id = h.hacker_id
inner join Challenges c on c.challenge_id = s.challenge_id 
inner join Difficulty d on d.difficulty_level = c.difficulty_level
where d.score = s.score
group by h.hacker_id having count(*) > 1
order by count(*) desc, h.hacker_id asc;
```

```
SELECT e.employee_ID, e.name
FROM employee_information e
INNER JOIN last_quarter_bonus b ON e.employee_ID = b.employee_ID
WHERE b.bonus >= 5000;
```

```
select distinct e.Ename as Employee, m.mgr as reports_to, m.Ename as Manager
from EMPLOYEES e
inner join Employees m on e.mgr = m.EmpID;
```

```
select department, AVG(salary) AS average
FROM staff
GROUP BY department
ORDER BY average DESC;
```

## take the first 3 characters  (1 is starting position, 3 is Number of charcters)
```
SELECT SUBSTRING(firstname, 1, 3) 
FROM Customers 
```

## Email generator
```
SELECT LOWER(CONCAT(firstname||'.'||lastname||'@company.com')) AS email FROM Employees
ORDER BY email ASC;
```

## Email generator
```
SELECT CONCAT(LOWER(firstname), '.', LOWER(lastname), '@company.com') AS email FROM Employees ORDER BY email ASC
```

```
SELECT A.name AS name,
CASE WHEN COUNT(B.id) IS NULL THEN 0 ELSE COUNT(B.id) END AS books
FROM Authors A
LEFT JOIN Books B ON A.id = B.author_id
GROUP BY A.name
ORDER BY books DESC
```

```
SELECT name, year
FROM Books
WHERE Books.year > 1900
UNION ALL
SELECT name, 2022 AS year
FROM New
ORDER BY name;
```

## the count also includes 0 values: that's because we used a LEFT JOIN and some of the customers don't have any matching phone numbers.
```
SELECT C.id, COUNT(PN.number) AS count
FROM Customers AS C LEFT JOIN PhoneNumbers AS PN
ON C.id = PN.customer_id
GROUP BY C.id
```

## GROUP BY SHOULD ALWAYS BEFORE ORDER BY
```
SELECT nickname, MAX(score) AS best
FROM Scores
GROUP BY nickname
ORDER BY best DESC
```

```
SELECT si.roll_number, si.name
FROM student_information si
INNER JOIN examination_marks em ON si.roll_number = em.roll_number
WHERE (em.subject_one + em.subject_two + em.subject_three) < 100;
```

```
SELECT w.id, sub.age, sub.coins_needed, sub.power
FROM (
    SELECT wp.age, w.power, MIN(w.coins_needed) AS coins_needed
    FROM Wands w
    LEFT JOIN Wands_Property wp
    ON w.code = wp.code
    WHERE wp.is_evil = 0
    GROUP BY wp.age, w.power
) sub
INNER JOIN Wands w
ON sub.coins_needed = w.coins_needed AND sub.power = w.power
INNER JOIN Wands_Property wp
ON w.code = wp.code AND sub.age = wp.age
ORDER BY sub.power DESC, sub.age DESC;
```

## 1st SELECTING THE VALUES requested to be printed
```
SELECT h.hacker_id, h.name, COUNT(c.challenge_id) AS challenge_counter
FROM hackers h
JOIN challenges c
	ON h.hacker_id = c.hacker_id
GROUP BY h.hacker_id, h.name

## 2nd applying the values found before

HAVING challenge_counter IN (
	SELECT aux_table.counter
	FROM(
		SELECT hacker_id, COUNT(challenge_id) AS counter 
		FROM challenges
		GROUP BY hacker_id
		ORDER BY counter DESC
	) AS aux_table
	GROUP BY aux_table.counter 
	HAVING COUNT(aux_table.counter) = 1
)
OR
challenge_counter =(
	SELECT MAX(aux_table.counter)
	FROM(
		SELECT hacker_id, COUNT(challenge_id) AS counter
		FROM challenges
		GROUP BY hacker_id
		ORDER BY counter DESC
	) AS aux_table)
## Finally we order as requested (by counter and hacker_id)
ORDER BY challenge_counter DESC, h.hacker_id ASC;

##CASE must be ended with the keyword END
SELECT CASE             
            WHEN A + B > C AND B + C > A AND A + C > B THEN
                CASE 
                    WHEN A = B AND B = C THEN 'Equilateral'
                    WHEN A = B OR B = C OR A = C THEN 'Isosceles'
                    ELSE 'Scalene'
                END
            ELSE 'Not A Triangle'
        END
FROM TRIANGLES;
```

## using concat method i.e.: enclosed in parentheses)
```
SELECT 
    CONCAT(Name,'(',LEFT(Occupation,1), ')' )as name_ocuppation 
FROM OCCUPATIONS
ORDER BY NAME ASC;
```

```
SELECT 
    CONCAT('There are a total of ', COUNT(Occupation), ' ', LOWER(Occupation),'s.') AS solution 
FROM OCCUPATIONS 
GROUP BY Occupation
ORDER BY COUNT(Occupation);
```

## PIVOT 
transformation that reorganizes the data in a way that changes the structure of the table. Specifically, it involves converting rows into columns or vice versa.pivots 

eg. pivot the OCCUPATION column so that each name is sorted alphabetically and displayed underneath its corresponding occupation. If there are no more names corresponding to an occupation, it prints NULL. The MIN function ensures that the minimum name is selected for each occupation group, and the GROUP BY OCCUS ensures that the names are grouped by their row numbers.

```
SELECT 
MIN(CASE WHEN OCCUPATION = 'DOCTOR'      THEN NAME ELSE NULL END) ,
MIN(CASE WHEN OCCUPATION = 'PROFESSOR'   THEN NAME ELSE NULL END) ,
MIN(CASE WHEN OCCUPATION = 'SINGER'      THEN NAME ELSE NULL END) ,
MIN(CASE WHEN OCCUPATION = 'ACTOR'       THEN NAME ELSE NULL END)
FROM
(SELECT OCCUPATION, NAME, ROW_NUMBER() OVER (PARTITION BY OCCUPATION ORDER BY NAME) AS OCCUS
FROM OCCUPATIONS) AS temp
GROUP BY OCCUS
```

```
SELECT firstname, lastname, salary,
CASE 
    WHEN salary BETWEEN 0 AND 1500 THEN salary * 0.1 
    WHEN salary BETWEEN 1501 AND 2000 THEN salary * 0.2 
    WHEN salary > 2001 THEN salary * 0.3 
END AS tax
FROM Employees 
ORDER BY lastname ASC;
```

```
SELECT Books.name AS name, 
       Books.year, 
       Authors.name AS author
FROM Books
JOIN Authors ON Books.author_id = Authors.id
ORDER BY Authors.name ASC, Books.year ASC;
```

```
SELECT N,
 CASE 
When P is NULL THEN 'Root' 
When N NOT IN (Select P from BST where P is not Null ) THEN 'Leaf' 
Else 'Inner' 
END as nodes FROM BST 
Order by N;
```

```
SELECT 
    c.company_code,
    c.founder,
    COUNT(DISTINCT l.lead_manager_code) AS "T_lead_Manager",
    COUNT(DISTINCT s.senior_manager_code) AS "T_senior_manager_code",
    COUNT(DISTINCT m.manager_code) AS "T_manager_code",
    COUNT(DISTINCT e.employee_code) AS "T_employee_code"
FROM 
    Company c
INNER JOIN 
    Lead_manager l ON c.company_code = l.company_code
INNER JOIN 
    Senior_Manager s ON l.company_code = s.company_code
INNER JOIN 
    Manager m ON s.company_code = m.company_code
INNER JOIN 
    Employee e ON m.company_code = e.company_code
GROUP BY 
    c.company_code, c.founder
ORDER BY 
    c.company_code;

with recursive tblnums
as (
	select 2 as nums
	union all
	select nums+1 
	from tblnums
	where nums<1000)  
	
select group_concat(tt.nums order by tt.nums separator '&')  as nums
from tblnums tt
where not exists 
	( select 1 from tblnums t2 
	where t2.nums <= tt.nums/2 and mod(tt.nums,t2.nums)=0) 
```

## AGGREGATION
```
SELECT CEIL(AVG(Salary)-AVG(REPLACE(Salary,'0',''))) FROM EMPLOYEES;
```

## find the maximum total earnings for all employees as well as the total number of employees who have maximum total earnings.
```
SELECT salary * months AS earnings, COUNT(*)
FROM Employee
GROUP BY earnings
ORDER BY earnings DESC
LIMIT 1;
```

## ROUNDING VALUE TO TWO DECIMAL PLACES
```
SELECT ROUND(SUM(LAT_N),2) AS a, ROUND(SUM(LONG_W),2) AS t FROM STATION;
```

```
SELECT ROUND(SUM(LAT_N),4) AS a FROM STATION WHERE LAT_N < 137.2345 AND LAT_N >38.7880
```

```
SELECT ROUND(LONG_W,4) FROM STATION WHERE LAT_N = (SELECT MAX(LAT_N) FROM STATION WHERE LAT_N < 137.2345)
```

```
SELECT MIN(ROUND((LAT_N),4)) FROM STATION WHERE LAT_N > 38.7780
```

```
SELECT ROUND(LONG_W,4) FROM STATION WHERE LAT_N = (SELECT MIN(LAT_N) FROM STATION WHERE LAT_N > 38.7880)
```

```
SELECT ROUND(ABS(MIN(LAT_N) - MAX(LAT_N)) + ABS(MIN(LONG_W) - MAX(LONG_W)), 4) AS Manhattan_Distance
FROM STATION;
```

## percent_rank usage to calculate the median in MySQL
```
select round(lat_n,4) from (select lat_n, percent_rank() over (order by lat_n) as quart from station) as temp where quart=0.5 order by lat_n limit 1;
```

## using DATEDIFF to calculate the data difference
## disable the strict mode in MySQL. When strict mode is enabled, MySQL performs additional checks and validations on data, which can help prevent certain types of errors or data inconsistencies. By setting it to an empty string, you effectively turn off strict mode.
```
SET sql_mode = '';
SELECT Start_Date, End_Date
FROM 
    (SELECT Start_Date FROM Projects WHERE Start_Date NOT IN (SELECT End_Date FROM Projects)) a,
    (SELECT End_Date FROM Projects WHERE End_Date NOT IN (SELECT Start_Date FROM Projects)) b 
WHERE Start_Date < End_Date
GROUP BY Start_Date 
ORDER BY DATEDIFF(End_Date, Start_Date), Start_Date
```

```
SELECT
    S.Name AS Student_Name
FROM Students S
INNER JOIN Friends F ON S.ID = F.ID
INNER JOIN Students F_S ON F.Friend_ID = F_S.ID
INNER JOIN Packages S_P ON S.ID = S_P.ID
INNER JOIN Packages F_P ON F_S.ID = F_P.ID
WHERE F_P.Salary > S_P.Salary
ORDER BY F_P.Salary;
```

## advanced join
```
SELECT f1.X, f1.Y
FROM Functions AS f1
WHERE f1.X = f1.Y
  AND (SELECT COUNT(*) FROM Functions WHERE X = f1.X AND Y = f1.Y) > 1
UNION
SELECT f1.X, f1.Y
FROM Functions AS f1
WHERE EXISTS (SELECT X, Y FROM Functions WHERE f1.X = Y AND f1.Y = X AND f1.X < X)
ORDER BY X;
```

```
WITH SUM_View_Stats AS (
SELECT challenge_id
    , total_views = sum(total_views)
    , total_unique_views = sum(total_unique_views)
FROM View_Stats 
GROUP BY challenge_id
)
,SUM_Submission_Stats AS (
SELECT challenge_id
    , total_submissions = sum(total_submissions)
    , total_accepted_submissions = sum(total_accepted_submissions)
FROM Submission_Stats 
GROUP BY challenge_id
)
```

## left join
```
select con.contest_id,
        con.hacker_id, 
        con.name, 
        sum(total_submissions), 
        sum(total_accepted_submissions), 
        sum(total_views), sum(total_unique_views)
from contests con 
join colleges col on con.contest_id = col.contest_id 
join challenges cha on  col.college_id = cha.college_id 
left join
(select challenge_id, sum(total_views) as total_views, sum(total_unique_views) as total_unique_views
from view_stats group by challenge_id) vs on cha.challenge_id = vs.challenge_id 
left join
(select challenge_id, sum(total_submissions) as total_submissions, sum(total_accepted_submissions) as total_accepted_submissions from submission_stats group by challenge_id) ss on cha.challenge_id = ss.challenge_id
    group by con.contest_id, con.hacker_id, con.name
        having sum(total_submissions)!=0 or 
                sum(total_accepted_submissions)!=0 or
                sum(total_views)!=0 or
                sum(total_unique_views)!=0
            order by contest_id;
```

## getting data from multiple tables (explicit join - without using join command)
```
select * from inventory,sale
where sale.inventoryid=inventory.inventoryid
```

```
select
inventoryname,saledate,saleunitprice,salequantity,salequantity*saleunitprice 
as [Total amount]
from sale,inventory
where sale.inventoryid=inventory.inventoryid
group by sale.inventoryid,inventoryname,saledate,salequantity,saleunitprice
order by inventoryname
```

## getting data from multiple tables (implicit join - using join command)
## inner join
```
select * from inventory 
inner join sale
on sale.inventoryid=inventory.inventoryid
select
inventoryname,saledate,saleunitprice,salequantity,saleunitprice*salequantity 
as [Total Amount]
from inventory inner join sale 
on sale.inventoryid=inventory.inventoryid 
order by inventoryname
```

## full outer join (shows everything)
```
select sale.inventoryid,inventoryname
from inventory 
full outer join sale on
sale.inventoryid=inventory.inventoryid 
where sale.inventoryid is NULL
```

## left join (might have NULL value, since some inventory might not have sales)
```
select inventory.inventoryid,inventoryname
from inventory left join sale on
sale.inventoryid=inventory.inventoryid
```

## left join
```
select inventory.inventoryid,inventoryname
from inventory left join sale on
sale.inventoryid=inventory.inventoryid 
where sale.inventoryid is NULL
```

## without join: use subquery
```
select inventoryid,inventoryname from inventory
where inventoryid not in (select inventoryid from sale)
```

## right join
```
select sale.inventoryid,inventoryname
from inventory right join sale on
sale.inventoryid=inventory.inventoryid 
```

## Self Join
## commonly used in processing hierarchy
```
select E.employeeID, E.employeefirstname+' '+E.employeelastname as [Full
Name], E.managerID, , M.employeefirstname+' '+M.employeelastname as
[Manager Name]
from staff E
inner join staff M
on E.managerID = M.employeeID
```

## left outer join (list all the employees)
```
select E.employeeID, E.employeefirstname+' '+E.employeelastname as [F
Name], E.managerID, , M.employeefirstname+' '+M.employeelastname as
[Manager Name]
from staff E
left outer join staff M
on E.managerID = M.employeeID
```

## Cross Join
## generate all combination of records (all possibility)(Cartesian Product)
```
select * from inventory1
cross join inventory2
```

## SQL UNIONS
allow you to combine two tablestogether (but the no. of columns & each column’s data types for 2 tables must be match)
don't need common key, only need common attributes merge, not showing duplicate record
```
select cust_lname,cust_fname from customer
union
select cust_lname,cust_fname from customer_2
```

## Union all
merge, but show you everything, even the duplicate record
```
select cust_lname,cust_fname from customer
union all 
select cust_lname,cust_fname from customer_2
```

## Intersect
keep only the rows in common to both query not showing duplicate record
```
select cust_lname,cust_fname from customer
intersect
select cust_lname,cust_fname from customer_2
```

```
select c.cust_lname,c.cust_fname from customer c,customer_2 c2
where c.cust_lname=c2.cust_lname and c.cust_fname=c2.cust_fname
```

## Except
generate only the records that are unique to the CUSTOMER table
```
select cust_lname,cust_fname from customer
except
select cust_lname,cust_fname from customer_2
```

## use subquery
```
select cust_lname,cust_fname from customer
where(cust_lname) not in
(select cust_lname from customer_2) and
(cust_fname) not in
(select cust_fname from customer_2)
```

## SQL RANKS
ROW_NUMBER() --get a unique sequential number for each row
get different ranks for the row having similar values

```
SELECT *,
 ROW_NUMBER() OVER(ORDER BY Salary DESC) SalaryRank
FROM EmployeeSalary
```

## RANK() - specify rank for each row in the result set
use PARTITION BY to performs calculation on each group
each subset get rank as per Salary in descending order

## USING PARTITION BY
```
SELECT *,
 RANK() OVER(PARTITION BY JobTitle ORDER BY Salary DESC)
SalaryRank
FROM EmployeeSalary
ORDER BY JobTitle, SalaryRank
```

## NOT USING PARTITION BY
get SAME ranks for the row having similar values
```
SELECT *,
 RANK() OVER(ORDER BY Salary DESC) SalaryRank
FROM EmployeeSalary
ORDER BY SalaryRank
```

## DENSE_RANK()
if have duplicate values, SQL assigns different ranks to those rows. 
-will get the same rank for duplicate or similar values
```
SELECT *,
 DENSE_RANK() OVER(ORDER BY Salary DESC) SalaryRank
FROM EmployeeSalary
ORDER BY SalaryRank
```

```
Select * from einvpoinvoiceheader order by recid;
##select * from einvpoinvoiceline order by recid;
Select top 1 * from vendtable where name like 'SYARIKAT BERJAYA%';
Select top 1 * from vendtable where name like 'yushan%';
Select top 1 * from vendtable where name like 'nano%';
```
