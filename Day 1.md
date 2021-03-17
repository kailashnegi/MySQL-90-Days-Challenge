# MYSQL - Day 1 #

### ================== ###

### Query 1 : Select from two tables ###

Table: Person

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId is the primary key column for this table.

Table: Address

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId is the primary key column for this table.

Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for each of those people:

FirstName, LastName, City, State


#### Solution: ####

SELECT P.FirstName, P.LastName, A.City, A.State 
FROM Person P LEFT JOIN Address A ON P.PersonId = A.PersonId


### Query 2 : 2nd Highest Salary ###

Write a SQL query to get the second highest salary from the Employee table.

+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
For example, given the above Employee table, the query should return 200 as the second highest salary. If there is no second highest salary, then the query should return null.

+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+


#### Method 1 ####

SELECT (SELECT DISTINCT Salary FROM Employee ORDER BY SALARY DESC LIMIT 1 OFFSET 1) AS SecondHighestSalary;

#### Method 2 ####

SELECT MAX(Salary) AS SecondHighestSalary
FROM Employee
WHERE Salary < (SELECT MAX(Salary) FROM Employee);


### Query 3 : Nth Highest Salary ###

Write a SQL query to get the nth highest salary from the Employee table.

+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
For example, given the above Employee table, the nth highest salary where n = 2 is 200. If there is no nth highest salary, then the query should return null.

+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+


#### Solution : ####

CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
SET N = N-1;
RETURN (
      # Write your MySQL query statement below.
      SELECT DISTINCT Salary as "getNthHighestSalary"
      FROM Employee ORDER BY Salary DESC LIMIT 1 OFFSET N
      
  );
END


### Query 4 : Rank Scores ###

Write a SQL query to rank scores. If there is a tie between two scores, both should have the same ranking. Note that after a tie, the next ranking number should be the next consecutive integer value. In other words, there should be no "holes" between ranks.

+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
For example, given the above Scores table, your query should generate the following report (order by highest score):

+-------+---------+
| score | Rank    |
+-------+---------+
| 4.00  | 1       |
| 4.00  | 1       |
| 3.85  | 2       |
| 3.65  | 3       |
| 3.65  | 3       |
| 3.50  | 4       |
+-------+---------+

Important Note: For MySQL solutions, to escape reserved words used as column names, you can use an apostrophe before and after the keyword. For example `Rank`.


#### Solution : ####

##Apostrophe is being used because Rank is a keyword in MySQL.

SELECT Score,
    DENSE_RANK() OVER (
        ORDER BY Score DESC
    ) `Rank`
FROM
    Scores;
    

### Query 4 : Consecutive Numbers ###

Table: Logs

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+
id is the primary key for this table.
 

Write an SQL query to find all numbers that appear at least three times consecutively.

Return the result table in any order.

The query result format is in the following example:

 

Logs table:
+----+-----+
| Id | Num |
+----+-----+
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |
+----+-----+

Result table:
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
1 is the only number that appears consecutively for at least three times.


#### Solution : ####

SELECT DISTINCT L.Num as 'ConsecutiveNums'
FROM
    (SELECT Id,Num,LAG(Num) OVER (ORDER BY Id) AS 'Lag', LEAD(Num) OVER (ORDER BY Id) AS 'Lead' FROM Logs) L
WHERE L.Num=L.Lag AND L.Lag=L.Lead
ORDER BY 1



```python

```
