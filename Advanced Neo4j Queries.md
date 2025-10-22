# Advanced Neo4j Queries

---

## 1. Dataset Setup

Let’s first create a dataset of users, movies, and their interactions.

```cypher
CREATE
  (alice:Person {name: 'Alice', age: 30}),
  (bob:Person {name: 'Bob', age: 35}),
  (carol:Person {name: 'Carol', age: 28}),
  (dave:Person {name: 'Dave', age: 40}),

  (m1:Movie {title: 'Inception', year: 2010}),
  (m2:Movie {title: 'The Matrix', year: 1999}),
  (m3:Movie {title: 'Interstellar', year: 2014}),

  (alice)-[:FRIENDS_WITH]->(bob),
  (bob)-[:FRIENDS_WITH]->(carol),
  (carol)-[:FRIENDS_WITH]->(dave),

  (alice)-[:WATCHED {rating: 5}]->(m1),
  (bob)-[:WATCHED {rating: 4}]->(m1),
  (bob)-[:WATCHED {rating: 5}]->(m2),
  (carol)-[:WATCHED {rating: 3}]->(m2),
  (dave)-[:WATCHED {rating: 4}]->(m3);
```

---

## 2. Variable-Length Paths (`*`)

You can use `*` to traverse paths of **unknown or variable length**.

Example:

```cypher
MATCH path = (a:Person {name: 'Alice'})-[:FRIENDS_WITH*1..3]->(friend)
RETURN friend.name, length(path) AS hops;
```

This finds everyone reachable within 3 friendship hops from Alice.

### Exercise 1

Find all people connected to Bob within **2 hops**, including their names and how many steps away they are.

---

## 3. Aggregation and Grouping

You can use aggregation functions like `count()`, `avg()`, `min()`, `max()`, `collect()`.

Example:

```cypher
MATCH (p:Person)-[r:WATCHED]->(m:Movie)
RETURN m.title, avg(r.rating) AS avgRating, count(p) AS viewers;
```

This computes the average rating and viewer count per movie.

### Exercise 2

Show each person and the **average rating** they’ve given across all movies.

---

## 4. Using `WITH` for Multi-Stage Queries

`WITH` allows you to **chain multiple stages** of processing. It works like a temporary pipeline between steps.

Example:

```cypher
MATCH (p:Person)-[r:WATCHED]->(m:Movie)
WITH p, avg(r.rating) AS avgRating
WHERE avgRating >= 4
RETURN p.name, avgRating;
```

Here:

* First stage computes each person’s average rating.
* Second stage filters only those with an average of 4 or more.

### Exercise 3

Find all movies that have **more than one watcher**, and return their titles and total watcher count.

---

## 5. Combining Aggregation and Relationships

You can combine grouping, filtering, and relationship expansion.

Example:

```cypher
MATCH (p:Person)-[r:WATCHED]->(m:Movie)<-[:WATCHED]-(other:Person)
WHERE p.name = 'Alice'
RETURN other.name AS similarViewer, collect(DISTINCT m.title) AS commonMovies;
```

This finds all people who watched the same movies as Alice.

### Exercise 4

Find pairs of people who have both watched at least one common movie, and list those shared movie titles.

---

## 6. Multi-Step Query Example

Let’s combine all techniques in a more complex example:

```cypher
MATCH (p:Person)-[r:WATCHED]->(m:Movie)
WITH m, avg(r.rating) AS avgRating, collect(p.name) AS reviewers
WHERE avgRating >= 4
RETURN m.title, avgRating, reviewers
ORDER BY avgRating DESC;
```

This query:

1. Groups by movie and calculates average rating.
2. Filters to only high-rated movies.
3. Returns titles, average ratings, and reviewers sorted by rating.

### Exercise 5

Find all movies watched by friends of friends of Alice, showing for each movie the number of **unique people** who watched it.

---

## 7. Key Takeaways

✅ Use `*` for variable-length path traversal
✅ Use `WITH` to build multi-stage logic
✅ Combine aggregation with filters for powerful analysis
✅ Always alias computed values (`AS`) for clarity

---

**Challenge Task**:
Find the top 2 movies (by average rating) among all people within 2 hops of Bob’s friends.

Use a combination of `*`, `WITH`, and aggregation functions to solve it!
