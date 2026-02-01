# Exercise 03: MongoDB – Document Queries and Analysis

- Name: Sydney Sailors
- Course: Database for Analytics
- Module: 3
- Database Used: MongoDB
- Dataset: `restaurants-json.json`

---

## Instructions

- Import the provided `restaurants-json.json` file into MongoDB.
- All commands must be **executed by you** in the MongoDB shell or MongoDB Compass.
- For each query:
  - Include the MongoDB command in a fenced code block
  - Include a **screenshot** showing the command and its result
- Store screenshots in the `screenshots/` folder and embed them below each answer.

---

## Question 1

When importing the documents from `restaurants-json.json`, **how many documents were imported into your collection**?

### Answer

25358

### Screenshot

```javascript
db.restaurants.countDocuments()
```

<img width="423" height="177" alt="Screen Shot 2026-02-01 at 11 41 02 AM" src="https://github.com/user-attachments/assets/c7a5de6c-a368-4af6-bdee-82bf04cf9521" />

---

## Question 2

Before writing queries on the data, **what command do you use to set the MongoDB shell to operate on the `44661` database**?

### MongoDB Command

```javascript
use 44661
```

### Screenshot

<img width="361" height="83" alt="Screen Shot 2026-02-01 at 4 25 56 PM" src="https://github.com/user-attachments/assets/57350bfb-1e34-4647-8ca3-5e09a0511938" />

---

## Question 3

Using your `restaurants` collection in the `44661` database, write the MongoDB query needed to **locate all documents in the `"Queens"` borough**.

### MongoDB Query

```javascript
db.restaurants.find({borough:'Queens'})
```

### Screenshot

<img width="1440" height="900" alt="Screen Shot 2026-02-01 at 4 45 58 PM" src="https://github.com/user-attachments/assets/480e0cdf-65bb-462e-bd81-7465f68c7e7d" />

---

## Question 4

Using your `restaurants` collection in the `44661` database, write the MongoDB query needed to **find the number of restaurants in the `"Queens"` borough**.

### MongoDB Query

```javascript
db.restaurants.countDocuments({borough:'Queens'})
```

### Screenshot

<img width="1440" height="900" alt="Screen Shot 2026-02-01 at 4 48 39 PM" src="https://github.com/user-attachments/assets/1f581675-2e5c-4c65-a0eb-0a15098d0bc4" />

---

## Question 5

Using your `restaurants` collection in the `44661` database, write the MongoDB query needed to **find the number of restaurants in the `"Queens"` borough whose cuisine is `"Hamburgers"`**.

### MongoDB Query

```javascript
db.restaurants.countDocuments({borough:'Queens',cuisine:'Hamburgers'})
```

### Screenshot

<img width="590" height="45" alt="Screen Shot 2026-02-01 at 4 58 00 PM" src="https://github.com/user-attachments/assets/1a849412-7ff7-401b-821b-9dd3ce68a7ea" />

---

## Question 6

Using your `restaurants` collection in the `44661` database, write the MongoDB query needed to **find the number of restaurants in Zipcode `10460`**.

*Hint: Look up how to query **embedded documents**.*

### MongoDB Query

```javascript
db.restaurants.countDocuments({"address.zipcode":'10460'})
```

### Screenshot

<img width="500" height="51" alt="Screen Shot 2026-02-01 at 5 17 59 PM" src="https://github.com/user-attachments/assets/b860050f-b4c5-4543-8d5d-6e52ee9a2197" />

---

## Question 7

Using your `restaurants` collection in the `44661` database, write the MongoDB query needed to **display only the names of restaurants in Zipcode `10460`**.

*Hint: Look up how to **project fields** in MongoDB.*

Your output should resemble:

```
{ name: "Wild Asia" }
{ name: "Terrace Cafe" }
{ name: "African Terrace" }
{ name: "Cool Zone" }
{ name: "Beaver Pond" }
...
```

### MongoDB Query

```javascript
db.restaurants.find({"address.zipcode":'10460'},{_id:0,name:1})
```

### Screenshot

![Q7 Screenshot](screenshots/q7_zipcode_names.png)

---

## Question 8

Using your `restaurants` collection in the `44661` database, write the MongoDB query needed to **display only the names of restaurants whose name contains `"IHOP"`**, ignoring case.

Your results should include:
- `"Ihop"`
- `"Ihop Restaurant"`

### MongoDB Query

```javascript
db.restaurants.find({name:{$regex:"IHOP",$options:"i"}},{_id:0,name:1})
```

### Screenshot

![Q8 Screenshot](screenshots/q8_ihop_case_insensitive.png)
