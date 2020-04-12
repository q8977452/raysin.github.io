---
layout: post
title:  "Leetcode Database Challenge 1 "
date:   2020-04-12 01:26:30 +0800
categories: Leetcode
tags : Leetcode SQL Database
mathjax: true
---
# Leetcode Database Challenge 1 
* Combine Two Tables(175)

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

    
	* Code
    
    ```sql
    select FirstName, LastName, City, State 
    from Person 
        left join Address
        on Person.PersonId = Address.PersonId;
     ```
* Second Highest Salary(176)
Write a SQL query to get the second highest salary from the Employee table.

        +----+--------+
        | Id | Salary |
        +----+--------+
        | 1  | 100    |
        | 2  | 200    |
        | 3  | 300    |
        +----+--------+
    
	* Code
    ```sql
    SELECT DISTINCT 
        MAX(Salary) AS  SecondHighestSalary
    FROM 
        Employee
    Where Salary <
        (SELECT MAX(Salary) FROM Employee);
    ```
*  Nth Highest Salary(177)
Write a SQL query to get the Nth highest salary from the Employee table.

        +----+--------+
        | Id | Salary |
        +----+--------+
        | 1  | 100    |
        | 2  | 200    |
        | 3  | 300    |
        +----+--------+
    
	* Code
    ```sql
    CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
    BEGIN
         RETURN (
             # Write your MySQL query statement below.
             SELECT MAX(Salary) FROM Employee E1
             WHERE N - 1 =
             (SELECT COUNT(DISTINCT(E2.Salary)) FROM Employee E2
              WHERE E2.Salary > E1.Salary)
            );
    END
    ```
* Rank Scores(178)
Write a SQL query to rank scores. If there is a tie between two scores, both should have the same ranking. Note that after a tie, the next ranking number should be the next consecutive integer value. In other words, there should be no "holes" between ranks.

            Original            Ranked  
        +----+-------+     +-------+------+
        | Id | Score |     | Score | Rank |
        +----+-------+     +-------+------+
        | 1  | 3.50  |     | 4.00  | 1    |
        | 2  | 3.65  |     | 4.00  | 1    |
        | 3  | 4.00  |     | 3.85  | 2    |
        | 4  | 3.85  |     | 3.65  | 3    |
        | 5  | 4.00  |     | 3.65  | 3    |
        | 6  | 3.65  |     | 3.50  | 4    |
        +----+-------+     +-------+------+
    
	* Code
    ```sql
    SELECT s.Score, COUNT(DISTINCT t.Score) Rank
    FROM Scores s JOIN Scores t ON s.Score <= t.Score
    GROUP BY s.Id ORDER BY s.Score DESC;
    ```
* Consecutive Numbers (180)
Write a SQL query to find all numbers that appear at least three times consecutively.

        +----+-----+
        | Id | Num |
        +----+-----+
        | 1  |  1  |
        | 2  |  1  |
        | 3  |  1  |
        | 4  |  2  |
        | 5  |  1  |
        | 6  |  2  |
        | 7  |  2  |
        +----+-----+
    
	* Code
    ```sql
    SELECT DISTINCT
    l1.Num AS ConsecutiveNums
    FROM
       Logs l1,
       Logs l2,
       Logs l3
    WHERE
       l1.Id = l2.Id - 1
       AND l2.Id = l3.Id - 1
       AND l1.Num = l2.Num
        AND l2.Num = l3.Num
    ;
    ```