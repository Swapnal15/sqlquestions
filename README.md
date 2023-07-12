# sqlquestions
Most asked sql questions
-- Create table employee and emplyoee_details
create table employee(
emp_id int not null,
emp_name varchar (50) not null,
gender varchar (20) not null,
salary int,
city varchar (20));

create table employee_details(
emp_id int not null,
project varchar (50) not null,
designation varchar (20) not null,
DOJ date);

-- Insert values
insert into 
employee (emp_id, emp_name, gender,salary, city)
values (1, 'Arjun', 'M', 75000, 'Pune'),
(2, 'Ekadanta', 'M', 125000, 'Bangalore'),
(3, 'Lalita', 'F', 150000 , 'Mathura'),
(4, 'Madhav', 'M', 250000 , 'Delhi'),
(5, 'Visakha', 'F', 120000 , 'Mathura');

insert into  employee_details
values (1, 'P1', 'Executive', '2019-01-26'),
(2, 'P2', 'Executive', '2023-03-11'),
(3, 'P1', 'Lead', '2020-04-21'),
(4, 'P3', 'Manager', '2020-08-12'),
(5, 'P2', 'Manager', '2018-03-16');

-- Find the list of employees whose salary ranges between 2L to 3L

select emp_id,emp_name,salary
from employee
where salary between 200000 and 300000;

-- Write a query to retrieve the list of employees from the same city

select e1.emp_name, e1.city
from employee e1 , employee e2
where  e1.city=e2.city and e1.emp_id!= e2.emp_id;

-- Query to find the null values in the Employee table.

select * from employee
where emp_id is null;

-- Query to find the cumulative sum of employee’s salary.

select emp_id, emp_name,salary, sum(salary) over (partition by emp_id) as cumulative_sum
from employee;

-- What’s the male and female employees ratio.

select round(
( 
select count(gender)
from employee
where gender='M')/count(gender) ,1) as maleratio
from employee;

select round(
( 
select count(gender)
from employee
where gender='F')/count(gender) ,1) as maleratio
from employee;

select round(
( 
select count(gender)
from employee
where gender='M')/count(gender) ,1) as malefemaleratio
from employee
where gender='F';

-- Write a query to fetch 50% records from the Employee table.
select * from employee
where emp_id<= (select count(emp_id) /2 as half from employee);

-- Query to fetch the employee’s salary but replace the LAST 2 digits with ‘XX’ i.e 12345 will be 123XX

select salary,
concat(SUBSTRING(cast(salary as char), 1, lenght(cast(salary as char))-2), 'XX') as masked_number
from employee;

-- Write a query to fetch even rows from Employee table.
select * from
(select  emp_name, emp_id, row_number() over (order by emp_id) as row_num from employee )
as even
where  row_num %2=0;

-- Write a query to find the list of Employee names which is starting with vowels (a, e, i, o, or u) and 
-- ending with vowels (a, e, i, o, or u), without duplicates
select * from employee;

SELECT DISTINCT emp_name
FROM employee
WHERE LOWER(emp_name) REGEXP '^[aeiou]';

SELECT DISTINCT emp_name
FROM employee
WHERE LOWER(emp_name) REGEXP '[aeiou]$';

-- Find Nth highest salary from employee table with and without using the TOP/LIMIT keywords.
select * from ( select emp_name, salary, row_number () over (order by salary desc) as row_num from employee) 
as salary_desc
where row_num = 1 ;

-- Query to retrieve the list of employees working in same project.
select * from employee;
select * from employee_details;


-- Query to retrieve the list of employees working in same project.
with CTE as

(select e.emp_id, e.emp_name, ed.project
from employee as e
inner join employee_details as ed
on e.emp_id = ed.emp_id)

select c1.emp_name, c2.emp_name, c1.project
from CTE c1, CTE c2
where c1.project = c2.project AND c1.emp_id != c2.emp_id AND c1.emp_id < c2.emp_id;


