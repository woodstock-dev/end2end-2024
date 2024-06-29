<!--
Copyright 2024 Ryan McGuinness

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->
# Lesson 3

## Lesson 2 Q&A

After learning about select statements and joins you were assigned a task:
"Find the total sales (in dollars) for the album, invoce_line, and track tables, and order from the most to the least."

You were given a link to learn more about SQL functions that would help you solve the problem.

> Note: Pause and share your work, challenges, and triumphs

Does your code look similar to this:

```sql
select album.title,
       sum(invoice.unit_price*invoice.quantity) sales_total,
       sum(invoice.quantity) as tot_qty from
Album album join Track track ON track.album_id = album.album_id
JOIN invoice_line invoice ON track.track_id = invoice.track_id
GROUP BY (album.title)
ORDER BY sales_total desc;
```

## Review Challenge

### Employees

How many employees are also customers?

Who doesn't report to anyone?

### Customers

How many customers do we have?

What's the average number of items bought per customer?

### Media

How many movies do we sell?

What's the most common media type in our inventory?

## Understanding Performance

SQL has an amazing tool called "explain" and it can show you the complexity and the cost of your query.
Explain is easy to use:

```sql
explain select * from employee;
```

Here, we get an estimation to see that the weighted cost (projected time) of the execution is 0 to 10.8
with 80 rows (total reserved size of the table) and 978 characters wide (The sum of the length of all fields + a header and footer).

```shell
Seq Scan on employee  (cost=0.00..10.80 rows=80 width=978)
```

In order to make sense of explain, we use it along with "analyze", this gives us a litte more human readable output.

```sql
explain analyze select * from employee
```

Now, in addition to the aformentioned explain, we get an actual analysis, 
so everthting aftward is the exact about of time of execution took. That being said,
these numbers will change dependent on other workloads on the system.

```shell
Seq Scan on employee  (cost=0.00..10.80 rows=80 width=978) (actual time=0.009..0.011 rows=8 loops=1)
Planning Time: 0.057 ms
Execution Time: 0.023 ms
```

## Sub Selects

Aside from joins there are other ways to relate tables to one another,
one is called a sub-select, or an imbedded select statement. This type
of select statement let's us filter our data a little more intellegently
and allows us to deal with collections of data.

For example, if we wanted to find all of our customer who shared the same
first name with our employees, we could execute somthing like this:

```sql
SELECT e.first_name, e.last_name, c.first_name, c.last_name
FROM employee e, customer c
WHERE e.first_name IN 
      (SELECT first_name 
       FROM customer 
       WHERE c.first_name = e.first_name) 
ORDER BY e.last_name, e.first_name;
```

> NOTE: We have introduced the "IN" clause, that is like saying the subset contains the value.

> By executing this query, we can see that only two of our employees share
> the first name of our customers.

Question: How many share the same last name?

## Homework

Find the most efficient query to determine if a customer is an employee.

> Hint: Analyze the data, try multiple queries, and use explain and analyze to justify your answer.

