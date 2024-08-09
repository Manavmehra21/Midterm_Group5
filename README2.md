# ONLINE BOOKSTORE SYSTEM

## Team Duties

## Duties Assigned to Team Members
- **Database Design**: Manav Mehra
- **Database Implementation**: Mayank Sharma
- **Query Development**: Emmanuella Owulu Amara
- **Typescript Interface**: Manav, Mayank ,Amara

## Tables and Attributes

### Book Table

| Column Name    | Data Type  | Description                               |
|----------------|------------|-------------------------------------------|
| Book_id        | Int        | Unique identifier of the book              |
| Title          | Varchar    | Title of the book                         |
| Genre_id       | Int        | Identifier of the genre                    |
| Author_id      | Int        | Identifier of the author                   |
| Publisher_id   | Int        | Identifier of the publisher                |
| Rating         | Float      | Average user rating of the book            |
| Price          | Decimal    | Price of the book                          |
| Format         | Varchar    | Format of the book                         |
| Published_date | Date       | Date when the book was published           |

Foreign Keys:
- genre_id references Genres(genre_id)
- author_id references Authors(author_id)
- publisher_id references Publishers(publisher_id)

### Authors Table

| Column Name | Data Type | Description                              |
|-------------|-----------|------------------------------------------|
| author_id   | INT       | Unique identifier for each author         |
| name        | VARCHAR   | Name of the author                        |
| biography   | TEXT      | Short biography of the author             |

### Publishers Table

| Column Name  | Data Type | Description                              |
|--------------|-----------|------------------------------------------|
| publisher_id | INT       | Unique identifier for each publisher      |
| name         | VARCHAR   | Name of the publisher                     |
| address      | VARCHAR   | Address of the publisher                  |

### Customers Table

| Column Name | Data Type | Description                              |
|-------------|-----------|------------------------------------------|
| customer_id | INT       | Unique identifier of customer             |
| name        | VARCHAR   | Name of customer                         |
| email       | VARCHAR   | Email of customer                         |
| Total_spent | DECIMAL   | Total spent by the customer               |

### Orders Table

| Column Name  | Data Type | Description                              |
|--------------|-----------|------------------------------------------|
| order_id     | INT       | Unique identifier for each order          |
| customer_id  | INT       | Identify the customer who placed an order |
| order_date   | DATE      | Date when the order was placed            |
| total_amount | DECIMAL   | Total amount of orders                   |

Foreign Key:
- customer_id references Customers(customer_id)

### OrderDetails Table

| Column Name | Data Type | Description                              |
|-------------|-----------|------------------------------------------|
| order_id    | INT       | Identifier for the order                  |
| book_id     | INT       | Identifier for the book                   |
| quantity    | INT       | Quantity of ordered books                |
| price       | DECIMAL   | Price of books                            |

Foreign Keys:
- order_id references Orders(order_id)
- book_id references Books(book_id)

### Reviews Table

| Column Name | Data Type | Description                              |
|-------------|-----------|------------------------------------------|
| review_id   | INT       | Unique identifier for each review         |
| book_id     | INT       | Identifier for the book being reviewed    |
| customer_id | INT       | Identifier for the customer who wrote the review |
| review_text | TEXT      | Text of the review                        |
| rating      | INT       | Rating given by the customer              |
| posted_date | DATE      | Date when the review was posted           |

Foreign Keys:
- book_id references Books(book_id)
- customer_id references Customers(customer_id)

### Genres Table

| Column Name | Data Type | Description                              |
|-------------|-----------|------------------------------------------|
| genre_id    | INT       | Unique identifier for each genre          |
| genre_name  | VARCHAR   | Name of the genre                         |

## SQL Database Creation

```sql
-- Create Authors table
CREATE TABLE Authors (
    author_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    biography TEXT
);

-- Create Publishers table
CREATE TABLE Publishers (
    publisher_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    address VARCHAR(255)
);

-- Create Genres table
CREATE TABLE Genres (
    genre_id INT PRIMARY KEY AUTO_INCREMENT,
    genre_name VARCHAR(100) NOT NULL
);

-- Create Books table
CREATE TABLE Books (
    book_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL,
    genre_id INT,
    author_id INT,
    publisher_id INT,
    rating FLOAT,
    price DECIMAL(10, 2),
    format VARCHAR(50),
    published_date DATE,
    FOREIGN KEY (genre_id) REFERENCES Genres(genre_id),
    FOREIGN KEY (author_id) REFERENCES Authors(author_id),
    FOREIGN KEY (publisher_id) REFERENCES Publishers(publisher_id)
);

-- Create Customers table
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    total_spent DECIMAL(10, 2)
);

-- Create Orders table
CREATE TABLE Orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);

-- Create OrderDetails table
CREATE TABLE OrderDetails (
    order_id INT,
    book_id INT,
    quantity INT,
    price DECIMAL(10, 2),
    PRIMARY KEY (order_id, book_id),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (book_id) REFERENCES Books(book_id)
);

-- Create Reviews table
CREATE TABLE Reviews (
    review_id INT PRIMARY KEY AUTO_INCREMENT,
    book_id INT,
    customer_id INT,
    review_text TEXT,
    rating INT,
    posted_date DATE,
    FOREIGN KEY (book_id) REFERENCES Books(book_id),
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);
```
## DDL/DML for the Authors Table

### DDL (Data Definition Language)

#### Create the Authors Table

```sql
CREATE TABLE Authors (
    author_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    biography TEXT
);

ALTER TABLE Authors
ADD COLUMN nationality VARCHAR(100);

ALTER TABLE Authors
MODIFY COLUMN biography VARCHAR(100);

ALTER TABLE Authors
CHANGE COLUMN biography author_bio TEXT;

-- Delete the Authors table
DROP TABLE Authors;

-- Removes all rows from the Authors table
TRUNCATE TABLE Authors;
```
### DML (Data Manipulation Language)
```sql
-- Inserting a new author
INSERT INTO Authors (name, biography)
VALUES ('J.K. Rowling', 'British author, best known for her Harry Potter series.');

-- Inserting multiple authors at once
INSERT INTO Authors (name, biography)
VALUES
    ('Agatha Christie', 'English writer known for her detective novels, short story collections, plays, and famous detective characters.'),
    ('George R.R. Martin', 'American novelist and short story writer, best known for his series of epic fantasy novels.');

-- Select all authors
SELECT * FROM Authors;

-- Select specific author by author_id
SELECT * FROM Authors WHERE author_id = 1;

-- Update biography of an author
UPDATE Authors
SET biography = 'British author, best known for her Harry Potter series and subsequent works.'
WHERE author_id = 1;

-- Update name of an author
UPDATE Authors
SET name = 'Joanne Rowling'
WHERE author_id = 1;

-- Delete an author by author_id
DELETE FROM Authors WHERE author_id = 4;

```
## SQL Queries for Various Requirements

### Power Writers (Authors) with More than X Books in the Same Genre Published within the Last X Years

```sql
SELECT Authors.author_id, Authors.name, COUNT(Books.book_id) AS num_books
FROM Authors
JOIN Books ON Authors.author_id = Books.author_id
WHERE Books.genre_id = 4
  AND Books.published_date >= CURRENT_DATE - INTERVAL '1 year'
GROUP BY Authors.author_id, Authors.name
HAVING COUNT(Books.book_id) > 3
ORDER BY num_books DESC;
```
## Loyal Customers Who Have Spent More than X Dollars in the Last Year
```sql
SELECT customer_id, name, total_spent
FROM Customers
WHERE total_spent > 19
  AND CURRENT_DATE - INTERVAL '1 year' <= (SELECT MAX(order_date) FROM Orders WHERE Orders.customer_id = Customers.customer_id);
```
## Well Reviewed Books That Have a Better User Rating Than Average
```sql
SELECT Books.book_id, Books.title, Books.rating, AVG(Reviews.rating) AS avg_rating
FROM Books
LEFT JOIN Reviews ON Books.book_id = Reviews.book_id
GROUP BY Books.book_id, Books.title, Books.rating
HAVING Books.rating > AVG(Reviews.rating);
```
## The Most Popular Genre by Sales
```sql
SELECT Genres.genre_id, Genres.genre_name, SUM(OrderDetails.quantity) AS total_sales
FROM Genres
JOIN Books ON Genres.genre_id = Books.genre_id
JOIN OrderDetails ON Books.book_id = OrderDetails.book_id
GROUP BY Genres.genre_id, Genres.genre_name
ORDER BY total_sales DESC
LIMIT 1;
```
## The 10 Most Recent Posted Reviews by Customers
```sql
SELECT Reviews.review_id, Reviews.review_text, Reviews.rating, Reviews.posted_date, Customers.name AS customer_name
FROM Reviews
JOIN Customers ON Reviews.customer_id = Customers.customer_id
ORDER BY Reviews.posted_date DESC
LIMIT 10;
```

# Create a Typescript interface that will allow modification to a table. 
  ``` export type { default } from "./persistenceService"; ```
```  create: (name: string) => void;
  // drop: (name: string) => void;
  insert: <T = unknown>(content: T, location: string) => void;
  // update: <T = unknown>(content: T, location: string) => void;
  // delete: <T = unknown>(content: T, location: string) => void;
}
```
