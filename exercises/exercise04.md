# Exercise 04: Advanced SQL, Jupyter, and Visualization

- Name: Sydney Sailors
- Course: Database for Analytics
- Module: 04
- Database Used: World Database
- Tools Used: PostgreSQL, SQLAlchemy, Pandas, Jupyter Notebooks

---

## Instructions

- Complete each task using the **World database** installed earlier.
- For SQL questions:
  - Write the SQL command in a fenced code block
  - Execute the command and include a **screenshot of the results**
- For Jupyter Notebook questions:
  - Include the required Python statements
  - Include **screenshots of the notebook output**
- Store all screenshots in the `screenshots/` folder and embed them below each question.

---

## Question 1

Considering the World database, write a SQL statement that will **display the names of countries that speak more than two official languages**, along with the **number of official languages spoken**.

- Sort the results by **number of languages**, from **most to least**.
- *Hint: There are fewer than 10 countries in the results.*

### SQL

```sql
SELECT c.name AS country_name, COUNT(cl.language) AS number_of_official_languages
FROM country c
JOIN countrylanguage cl
ON c.code = cl.countrycode
WHERE cl.isofficial = 'T'
GROUP BY c.name
HAVING COUNT(cl.language) > 2
ORDER BY number_of_official_languages DESC;
```

### Screenshot

![Q1 Screenshot](screenshots/q1_official_language_counts.png)

---

## Question 2

Using **Jupyter Notebooks**, you must use the `create_engine` command to connect to your database.

After the `create_engine` command is executed, **what are the three statements required to execute the query from Question 1 and display the results in the notebook**?

### Python Code

```python
query = """
SELECT 
    c.name AS country_name, 
    COUNT(cl.language) AS number_of_official_languages
FROM country c
JOIN countrylanguage cl
    ON c.code = cl.countrycode
WHERE cl.isofficial = 'T'
GROUP BY c.name
HAVING COUNT(cl.language) > 2
ORDER BY number_of_official_languages DESC;
"""

df = pd.read_sql(query, engine)

df.index = df.index + 1

df
```

### Screenshot

![Q2 Screenshot](screenshots/q2_jupyter_query_results.png)

---

## Question 3

Using **Jupyter Notebooks**, write the Python code needed to produce the following graph:

![countries.jpg](./instructions/04-countries.jpg)

(The graph shows country-level results derived from the World database.)

### Python Code

```python
plt.figure(figsize=(12,6))
plt.bar(df["country_name"], df["number_of_official_languages"])
plt.xlabel("Country")
plt.ylabel("Number of Official Languages")
plt.title("Countries with More Than 2 Official Languages")
plt.xticks(rotation=90)
plt.tick_params(axis='x', pad=2)
plt.subplots_adjust(bottom=0.18)
plt.tight_layout()
plt.show()
```

### Screenshot

![Q3 Screenshot](screenshots/q3_countries_graph.png)
