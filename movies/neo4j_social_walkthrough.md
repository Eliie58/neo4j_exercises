# 🎓 Neo4j Interactive Tutorial: Social Media Graph

Welcome to your Cypher-powered adventure through a rich Neo4j dataset based on a **small social network around movies and TV shows**! 🚀  
You'll work step-by-step to explore, analyze, and master Cypher queries using real-world scenarios.

---

## 🧱 Part 1: Setup & Schema

### 🔧 Create Constraints
```cypher
CREATE CONSTRAINT ON (u:User) ASSERT u.name IS NOT NULL;
CREATE CONSTRAINT ON (m:Movie) ASSERT m.title IS UNIQUE;
CREATE CONSTRAINT ON (s:TV_Show) ASSERT s.title IS UNIQUE;
```
💡 Constraints ensure data integrity and speed up queries.

### 🧠 Task:
Use this to visualize your graph schema:
```cypher
CALL db.schema.visualization();
```

---

## 🧑‍🤝‍🧑 Part 2: Users and Relationships

### ❓ Challenge 1: Find all users who live in Los Angeles.
💡 **Hint**: Use a `MATCH` clause and a `WHERE` condition.

---

### ❓ Challenge 2: Who does "Bradley" follow?
💡 **Hint**:
```cypher
MATCH (:User {name: 'Bradley'})-[:FOLLOWS]->(f:User)
RETURN f.name;
```

---

### ❓ Challenge 3: Who follows "Ripley"?
💡 Think in reverse.

---

### ❓ Challenge 4: List users who follow each other mutually.

---

## 🎥 Part 3: Media and Interactions

### ❓ Challenge 5: List all movies that won at least 1 Oscar.
💡 Use `CONTAINS` or regex.

---

### ❓ Challenge 6: Find all users who commented on "Toy Story 3".

---

### ❓ Challenge 7: Which users rated the movie "Wreck-It Ralph" more than 8?

---

### ❓ Challenge 8: What genres has "Lisa" interacted with (watched, rated, or commented)?

---

## 📊 Part 4: Aggregations, Sorting & Grouping

### ❓ Challenge 9: How many users follow "Matthew"?

---

### ❓ Challenge 10: List users and how many people they follow, sorted by count descending.

---

### ❓ Challenge 11: What is the average rating given to each movie?  
💡 Use `avg()` and `collect()`.

---

### ❓ Challenge 12: Group all the TV shows each user subscribed to in a list.
💡 Use `collect()` and `RETURN`.

---

## 🧭 Part 5: Path Finding

### ❓ Challenge 13: Find the shortest path between "Bradley" and "Amy".
💡 Use:
```cypher
MATCH (a:User {name:'Bradley'}), (b:User {name:'Amy'})
MATCH path = shortestPath((a)-[:FOLLOWS*]-(b))
RETURN path;
```

---

### ❓ Challenge 14: Who are the users within 2 degrees of connection from "Lisa"?  
💡 Use variable length relationships.

---

## 🧠 Part 6: Advanced Challenges

### ❓ Challenge 15: Recommend a movie to "Bradley" that his friends watched but he hasn't.
💡 Use pattern exclusion and filtering.

---

### ❓ Challenge 16: Which city has the most active users (based on comments, ratings, watched)?

---

### ❓ Challenge 17: Which users have blocked someone but are still being followed by them?

---

### ❓ Challenge 18: What’s the most watched movie by Los Angeles users?

---

## 🎉 You Did It!

You now know how to:
- Traverse and query a social graph 🧑‍🤝‍🧑
- Group and aggregate data 📊
- Find paths and recommend content based on connections 🧭

Keep practicing and take it to the next level! 💪
