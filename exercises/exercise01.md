# Exercise 01: World Database SQL Practice

- Name: Sydney Sailors
- Course: Database for Analytics
- Module: 1
- Database Used: World Database

---

## Instructions

- Answer each question below.
- All SQL commands **must be executed** against the World database.
- For each SQL command:
  - Include the SQL in a fenced code block
  - Include a **screenshot** showing the command and results
- Store screenshots in the `screenshots/` folder and embed them below each answer.

---

## Question 1

**Compare and contrast the data types used for:**
- `country.Population`
- `country.LifeExpectancy`

Why were these data types selected?

### Answer
`country.Population` is stored as an integer data type and represents the whole-count of people, so decimals are not needed.
`country.LifeExpectancy` is stored as a decimal data type, which is appropriate as life expectancy is a statistical average.

### Screenshot

```sql
DESCRIBE country;
```

<img width="1440" height="900" alt="Screen Shot 2026-01-18 at 6 44 46 PM" src="https://github.com/user-attachments/assets/53c9cb41-ad03-4462-969a-856f8182d15a" />

---

## Question 2

**What is the data type of `country.IndepYear`?**
Why do you think this data type was selected?

### Answer
The data type of `country.IndepYear` is `smallInt`. `smallInt` is used to optimize database performance and storage efficiency. 

### Screenshot

```sql
DESCRIBE country;
```

<img width="1440" height="900" alt="Screen Shot 2026-01-18 at 6 55 46 PM" src="https://github.com/user-attachments/assets/7b68a352-9a2a-491d-b485-7b4b3a9b2ce3" />

---

## Question 3

**Make a case for a different data type for `country.IndepYear`.**
Explain why your proposed data type might be better in some situations.

### Answer
I believe `YEAR` would be better in some situations because you could leverage built-in SQL date and time funtions for extraction, comparison, or calculations. Queries could be more straightforward compared to using `country.IndepYear`.

---

## Question 4

Write a SQL command to **list the names of all cities in alphabetical order**.

### SQL

```sql
SELECT Name
FROM city
ORDER BY Name;
```

### Screenshot

<img width="1440" height="900" alt="Screen Shot 2026-01-18 at 7 04 37 PM" src="https://github.com/user-attachments/assets/33ce9637-c6c1-4813-977a-f1b59cb956b6" />

---

## Question 5

Write a SQL command to **list all forms of government from the `country` table**, showing **each only once**, sorted alphabetically.

### SQL

```sql
SELECT DISTINCT GovernmentForm
FROM country
ORDER BY GovernmentForm;
```

### Screenshot

<img width="1440" height="900" alt="Screen Shot 2026-01-18 at 7 10 26 PM" src="https://github.com/user-attachments/assets/6f205ab6-3c61-4b90-b05b-9429233cf3b0" />

---

## Question 6

Write a SQL command to **list all countries in the `Oceania` continent**.

### SQL

```sql
SELECT Name
FROM country
WHERE Continent = 'Oceania';
```

### Screenshot

![Q6 Screenshot](screenshots/q6_oceania.png)

---

## Question 7

Write a SQL command to **list the names and country code of all cities**.

### SQL

```sql
SELECT Name, CountryCode
FROM city;
```

### Screenshot

![Q7 Screenshot](screenshots/q7_city_countrycode.png)

---

## Question 8

Write a SQL command to **update the city named `"Nashville-Davidson"` to `"Nashville"`**.

### SQL

```sql
UPDATE city
SET Name = 'Nashville'
WHERE Name = 'Nashville-Davidson';
```

### Screenshot

![Q8 Screenshot](screenshots/q8_update_city.png)

---

## Question 9

Write a SQL command to **insert a new country named `"Narnia"`** with a country code of `"NAR"`.
Use reasonable values for the remaining columns.

### SQL

```sql
INSERT INTO country (Code, Name, Continent, Region, Population)
VALUES ('NAR', 'Narnia', 'Europe', 'Fantasy', 1000000);
```

### Screenshot

![Q9 Screenshot](screenshots/q9_insert_narnia.png)

---

## Question 10

Write a SQL command to **delete the country with the country code `"NAR"`**.

### SQL

```sql
DELETE FROM country
WHERE Code = 'NAR';
```

### Screenshot

![Q10 Screenshot](screenshots/q10_delete_narnia.png)
