# Exercise 05: SQLDA Database - Dates, Data Quality, Arrays, and JSON

- Name: Sydney Sailors
- Course: Database for Analytics
- Module: 05
- Database Used:  `sqlda` (Sample Datasets)
- Tools Used: PostgreSQL (pgAdmin or psql)

---

## Instructions

- Use the **sqlda** database from the "Loading the Sample Datasets" instructions.
- For each SQL task:
  - Include your SQL in a fenced code block
  - Execute it and include a **screenshot** showing the query and results
- Store screenshots in the `screenshots/` folder and embed them below each answer.
- For explanation questions:
  - Write your answer in complete sentences
  - Include a screenshot if requested

---

## Question 1

Using the `sqlda` database, write the SQL needed to show a **list of years** that emails were sent.

Your results should list years like this (order matters):

```
year
2011
2013
2014
2015
2016
2017
2018
2019
```

### SQL

```sql
SELECT DISTINCT 
EXTRACT(YEAR FROM sent_date) AS year
FROM emails
ORDER BY year;
```

### Screenshot

<img width="1440" height="900" alt="Screen Shot 2026-02-10 at 11 33 17 AM" src="https://github.com/user-attachments/assets/39604a07-2409-4e77-ab21-9afefb20c91b" />

---

## Question 2

Using the `sqlda` database, write the SQL needed to show the **number of messages sent by year**, ordered by year (as shown in the prompt).

Output should resemble:

```
count   year
...
```

### SQL

```sql
SELECT 
COUNT (*) AS count,
EXTRACT(YEAR FROM sent_date) AS year
FROM emails
GROUP BY year
ORDER BY year;
```

### Screenshot

<img width="1440" height="900" alt="Screen Shot 2026-02-10 at 11 37 23 AM" src="https://github.com/user-attachments/assets/9ee383ee-da06-4f67-9367-77585fc9ab98" />

---

## Question 3

Using the `sqlda` database, write the SQL needed to show:
- the **sent date**
- the **opened date**
- the **interval** between the two

Only include emails that contain **both** a sent date and an opened date.

### SQL

```sql
SELECT
sent_date,
opened_date,
opened_date - sent_date AS interval
FROM emails
WHERE sent_date IS NOT NULL
AND opened_date IS NOT NULL;
```

### Screenshot

<img width="1440" height="900" alt="Screen Shot 2026-02-10 at 11 42 02 AM" src="https://github.com/user-attachments/assets/c174ff58-cb5d-49fc-a3a7-36185cab084e" />

---

## Question 4

Using the `sqlda` database, write the SQL needed to show emails that contain an **opened date BEFORE the sent date**.

### SQL

```sql
SELECT
email_id,
sent_date,
opened_date
FROM emails
WHERE sent_date IS NOT NULL
AND opened_date IS NOT NULL
AND opened_date < sent_date
ORDER BY email_id;
```

### Screenshot

<img width="1440" height="900" alt="Screen Shot 2026-02-10 at 11 45 35 AM" src="https://github.com/user-attachments/assets/d11f5222-532b-466d-9369-ac77370585ce" />

---

## Question 5

Using the `sqlda` database: there are **over 100 emails** that contain an opened date **BEFORE** the sent date.

After looking at the data, **why is this the case?**

### Answer

It appears that the sent_date timestamps are not true event times in this dataset as they are all set to the same default time (15:00). The 'opened_date' contains real times (ex. 14:32:11; 13:12:22; etc). This shows a data quality issue rather than emails actually being opened before they were sent.

---

## Question 6

Using the `sqlda` database, explain in your own words what the following code does:

```sql
CREATE TEMP TABLE customer_points AS (
    SELECT
        customer_id,
        point(longitude, latitude) AS lng_lat_point
    FROM customers
    WHERE longitude IS NOT NULL
    AND latitude IS NOT NULL
);

CREATE TEMP TABLE dealership_points AS (
    SELECT
        dealership_id,
        point(longitude, latitude) AS lng_lat_point
    FROM dealerships
);

CREATE TEMP TABLE customer_dealership_distance AS (
    SELECT
       customer_id,
       dealership_id,
       c.lng_lat_point <@> d.lng_lat_point AS distance
    FROM customer_points c
    CROSS JOIN dealership_points d
);
```

### Answer

The first one is creating temporary table of customer locations. It converts longitude and latitude into a geographical point then displays the information of customers and their geographical location. The second one is creating a temporary table of dealership points. It converts longitude and latitude into a geographical point again then displays the dealership by their id and their geographical point. The third one is creating a temporary table of distances between every customer and dealership. It performs a cross join where it pairs every customer with every dealership then calculates the distance between their two location points using <@>. It displays the customer id, dealership id, and the distance between them.

---

## Question 7

Using the `sqlda` database, write SQL to display an **array of salespeople for each dealership**, sorted by dealership.

For example - dealership 1 is below:

```text
"{""Fidell,Granville"",""Onele,Jereme"",""Sheriff,Lelia"",""McSpirron,Massimiliano"",""Rennick,Nadia"",""Mace,Eveleen"",""Oxteby,Dukie"",""Spong,Marcos"",""Wogden,Quent"",""Duny,Sandye"",""Loraine,Englebert"",""Meere,Ira"",""Gibbens,Cristine"",""Prine,Lyda"",""McCoughan,Sheff"",""Schule,Giselbert"",""McAndie,Eleen"",""Dosedale,Dorie"",""Nafziger,Shay""}"
```

### SQL

```sql
SELECT
    dealership_id,
    ARRAY_AGG(
        last_name || ',' || first_name
    ) AS salespeople
FROM salespeople
GROUP BY dealership_id
ORDER BY dealership_id;
```

### Screenshot

<img width="1440" height="900" alt="Screen Shot 2026-02-10 at 1 50 40 PM" src="https://github.com/user-attachments/assets/2e45d76b-e498-4ae2-8ee2-4d0e6d350f54" />

---

## Question 8

Using the `sqlda` database, write SQL to display:
- an **array of salespeople for each dealership**
- the **state** of the dealership
- the **number of salespeople** for the dealership

Sort by **state**.

Reference image:

![05-ExerciseArray](./instructions/05-ExerciseArray.jpg)

### SQL

```sql
SELECT
    d.dealership_id,
    d.state,
	COUNT(s.salesperson_id) AS count,
    ARRAY_AGG(
        s.last_name || ',' || s.first_name
    ) AS salespeople
FROM dealerships d
JOIN salespeople s
  ON d.dealership_id = s.dealership_id
GROUP BY d.dealership_id, d.state
ORDER BY d.state;
```

### Screenshot

![Q8 Screenshot](screenshots/q8_salespeople_array_state_count.png)

---

## Question 9

Using the `sqlda` database, write the SQL needed to convert the **customers** table to **JSON**.

### SQL

```sql
SELECT json_agg(row_to_json(c))
FROM customers c;
```

### Screenshot

![Q9 Screenshot](screenshots/q9_customers_to_json.png)

---

## Question 10

Using the `sqlda` database, write SQL to display:
- an **array of salespeople for each dealership**
- the **state**
- the **number of salespeople**
- sorted by **state**

Then **convert this result to JSON**.

Reference image:

![05-ExerciseArray-1](./instructions/05-ExerciseArray-1.jpg)

### SQL

```sql
SELECT row_to_json(t)
FROM (
    SELECT
        d.dealership_id,
        d.state,
        ARRAY_AGG(s.last_name || ',' || s.first_name ORDER BY s.last_name, s.first_name) AS salespeople,
        COUNT(s.salesperson_id) AS num_salespeople
    FROM dealerships d
    JOIN salespeople s
      ON d.dealership_id = s.dealership_id
    GROUP BY d.dealership_id, d.state
    ORDER BY d.state
) t;
```

### Screenshot

![Q10 Screenshot](screenshots/q10_salespeople_array_to_json.png)
