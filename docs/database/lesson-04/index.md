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
# Lesson 4 - Data Definition Language (DDL)

## Abstract

In this lesson we will cover using DDL to fist update the table defintion of two tables in the database.
Next, we will create a table and use DDL to to referrence an existing table.
Lastly, we will cover the symantics of adding columns to existing tables.

## Step 1: Fixing two tables

### Customer Table
If we review the customer table, we see the following constraints:

* customer_pkey = The primary key (customer_id) on the table.
* customer_support_rep_id_key = The foreign key connecting the customer to the employee that helped them.
* customer_support_rep_id_idx = An index on the support_rep_id to help find the support rep faster.
* first_name, last_name, and email can not be null.

What this table lacks is business integrity. As covered in lesson business keys / natural keys are
an important part of defining how a table works. Consider the following:

> In the current definition of the table it's possible to have two customers with the same email.
> In addition, you could have two customers with the same name, address, phone twice or more.

Here are two simple examples of things we can improve by adding a business key.
So what are some steps we can can take to make this table better for our business?

1. Let's make sure that all email addresses are unique, we can do this by creating a unique key.
2. We can make a reasonable assumption that first_name, last_name, email should also be unique, and we can create a unique key for all three of those values.

DDL
```sql
ALTER TABLE customer ADD CONSTRAINT uq_customer_email UNIQUE(email);

ALTER TABLE customer ADD CONSTRAINT uq_customer_id UNIQUE(first_name, last_name, email);

ALTER TABLE customer DROP CONSTRAINT uq_customer_email
```

These two simple statements fix the problems identified above.

### Customer Invoice Table

As we continue our review of the database, we want to do a financial audit
and notice that the invoice table can accidently have multiple invoices.

What columns on this table can be fixed to disallow dupliacte entries?

```sql
ALTER TABLE invoice ADD CONSTRAINT uq_invoice ON unique(customer_id, invoice_date);
```

Now we can be sure that if our application does not handle double click correctly, we will not get duplicate invoices.

## Step 2 - Adding a Table

### Narrative

The owner of Chinook Media comes to you and asks: "I'd like to offer a
discounted price on some titles, but only during a certain time period.
Can you help? By the way, I'd like them to be at really odd hours like 2AM to 3AM,
so I don't think it should be manual"

Here, as a developer there are several ways to solve the problem, take a
look at the track table and consider how you'd do this.

> Pause here and review the track table, what are some of it's challenges?

Given that the owner is excited to get the work done, you quickly decide
to create a new table using the following DDL:

```sql
-- create a new table
CREATE TABLE track_discount(
    track_discount_id SERIAL,
    track_id INT NOT NULL,
    discount numeric(10, 2) NOT NULL,
    offer_date TIMESTAMP NOT NULL,
    close_date TIMESTAMP NOT NULL
);
-- Add a primary key to the table
ALTER TABLE track_discount ADD CONSTRAINT track_discount_pkey PRIMARY KEY(track_discount_id);
-- Add the foreign key
ALTER TABLE track_discount ADD CONSTRAINT track_discount_track_fkey FOREIGN KEY(track_id) REFERENCES track(track_id);

ALTER TABLE track_discount ADD CONSTRAINT track_discount_date_chk CHECK (offer_date < close_date);
```

> How can I fix a constraint if I don't like it? 

The answer is, we can always drop an recreate constraints WITHOUT dropping the table or the data.

```sql
ALTER TABLE track_discount DROP CONSTRAINT track_discount_pkey;
ALTER TABLE track_discount DROP CONSTRAINT track_discount_track_fkey;

-- Add a primary key to the table again with a better name
ALTER TABLE track_discount ADD CONSTRAINT track_discount_pk PRIMARY KEY(track_discount_id);
-- Add the foreign key to the table again with a better name
ALTER TABLE track_discount ADD CONSTRAINT track_discount_track_fk FOREIGN KEY(track_id) REFERENCES track(track_id);
```

## Step 3 - Adding columns to existing tables

In our last step, we will add a column to an existing table. 
Since the discount impact overall sales, the owner asks:
"Can you add an employee ID to the discount table to track
who made the change?"

As you consider this, you realize you'll have to make two changes,
one to the employee table, the other to the discount table.

In an audit, records can't be removed from the database,
and currently the only way an employee is if the row is deleted.

So we need to add a termination_date to the employee table:

```sql
ALTER TABLE employee ADD termination_date TIMESTAMP;
-- We also see that the hire date is nullable which makes NO sense
-- so we decide to fix that too. In order to do this, YOU MUST
-- verify that all rows have a start date.
ALTER TABLE employee ALTER COLUMN hire_date SET NOT NULL; 
ALTER TABLE employee ADD CONSTRAINT employee_chk CHECK(termination_date > hire_date);
```

Once the employee table has been updated, we can alter the
track_discount table by adding some information:

```sql
ALTER TABLE track_discount ADD employee_id int NOT NULL;

ALTER TABLE track_discount ADD CONTRAINT track_discound_employee_fk FOREIGN KEY(employee_id) REFERENCES employee(employee_id);
```

Rules:

* In order to add a column to the table, it must be either nullable, or have a default value.
* In order to update a column on a table, the current value MUST fit into the definition of the update. So floats can't become ints.
* If you change a column to NOT NULL, none of the existing values may be null.

## Homework

Based on the work we've done above, create a customer review table
that allows the customer to rate a track they have
purchased on a scale of 1 to 5 and leave a short comment.

> * What are the risks of this type of review?
> * How can you mitigate those risks?