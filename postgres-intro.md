PostGreSQL Workshop solutions and examples of queries 
===

### Find all the books by an author in SQL with postgres
```sql
SELECT * FROM authors
INNER JOIN book_authors ON book_authors.authors_id = authors.id 
INNER JOIN books ON books.id = book_authors.book_id 
WHERE authors.first_name = 'Ted';
```

### Find all the authors and their books
```sql
SELECT books.name,authors.first_name,authors.surname 
FROM book_authors 
INNER JOIN books ON books.id = book_authors.book_id 
INNER JOIN authors ON authors.id = book_authors.author_id 
ORDER BY authors.surname;
```

### Finding the authors by number of books
```sql
SELECT authors.surname,authors.first_name,
COUNT (book_authors.book_id) 
FROM authors 
INNER JOIN book_authors ON authors.id = book_authors.book_id GROUP BY authors.first_name, authors.surname
HAVING count (book_authors.book_id) > 2;
```

### Find queries matching a pattern
```sql
SELECT * 
FROM authors WHERE authors.surname
ILIKE '%wistle%';
```

`ILIKE` is case insensitive **postgres** specific method, as opposed to `LIKE` another alternative is `*~`

### Finding all the books by a particular author
```sql
SELECT books.name,authors.first_name,authors.surname 
FROM authors INNER JOIN book_authors 
ON book_authors.author_id = authors.id INNER JOIN books
ON books.id = book_authors.book_id WHERE authors.first_name = 'Ted' 
AND authors.surname = 'Burns';
```

### Find number of books published by a publisher
```sql
SELECT publishers.name, COUNT(books.publisher_id) 
FROM books INNER JOIN publishers ON publishers.id = books.publisher_id 
GROUP BY publishers.name 
HAVING COUNT(books.publisher_id) > 0;
```

### Find all books released after 1st Jan 1996, ordered by the number of people who wrote them. 

```sql
SELECT books.name,books.release_date, COUNT (book_authors.author_id)
FROM books INNER JOIN book_authors ON book_authors.book_id = books.id
WHERE books.release_date > '01-Jan-1996' GROUP BY books.name ,books.release_date HAVING COUNT(book_authors.author_id) > 0 ;
```
