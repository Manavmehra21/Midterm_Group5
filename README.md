# Midterm_Group5
# Online Bookstore Database Design

## Duties Assigned to Team Members
- **Database Design**: Manav Mehra
- **Database Implementation**: Mayank Sharma
- **Query Development**: Emmanuella Owulu Amara
- **Typescript Interface**: Manav, Mayank 

## Database Tables and Attributes

### Customers
- `CustomerID` INT PRIMARY KEY
- `FirstName` VARCHAR(50)
- `LastName` VARCHAR(50)
- `Email` VARCHAR(100) UNIQUE
- `Password` VARCHAR(255)
- `Address` VARCHAR(255)
- `PhoneNumber` VARCHAR(20)

### Authors
- `AuthorID` INT PRIMARY KEY ,
- `FirstName` VARCHAR(50),
- `LastName` VARCHAR(50),
- `Biography` TEXT

### Publishers
- `PublisherID` INT PRIMARY KEY 
- `Name` VARCHAR(100)
- `Address` VARCHAR(255)
- `PhoneNumber` VARCHAR(20) 

### Books
- `BookID` INT PRIMARY KEY
- `Title` VARCHAR(100)
- `ISBN` VARCHAR(20) UNIQUE
- `Genre` VARCHAR(50)
- `Format` VARCHAR(10)
- `Price` DECIMAL(10, 2)
- `PublisherID` INT Foreign Key
- `AuthorID` INT Foreign Key
- `ReleaseDate` DATE
- `Description` TEXT

### Reviews
- `ReviewID` INT, Primary Key
- `BookID` INT, Foreign Key
- `CustomerID` INT, Foreign Key
- `Rating` INT
- `Comment` TEXT
- `ReviewDate` DATE

### Orders
- `OrderID` INT PRIMARY KEY
- `CustomerID` INT Foreign Key
- `OrderDate` DATE
- `TotalAmount` DECIMAL(10, 2),

### OrderDetails
- `OrderDetailID` INT PRIMARY KEY
- `OrderID` INT Foreign key
- `BookID` INT Foreign Key
- `Quantity` INT
- `Price` DECIMAL(10, 2),  

### Sql Queries for creating above mentioned tables in Database

CREATE TABLE Customers ( 
CustomerID INT PRIMARY KEY,
FirstName VARCHAR(50), 
LastName VARCHAR(50),
Email VARCHAR(100) UNIQUE, 
Password VARCHAR(255), 
Address VARCHAR(255), 
PhoneNumber VARCHAR(20)
 );

CREATE TABLE Authors ( 
AuthorID INT PRIMARY KEY , 
FirstName VARCHAR(50), 
LastName VARCHAR(50), 
Biography TEXT 
);

CREATE TABLE Publishers (
PublisherID INT PRIMARY KEY , 
Name VARCHAR(100), 
Address VARCHAR(255), 
PhoneNumber VARCHAR(20) 
);

CREATE TABLE Books (
BookID INT PRIMARY KEY,
Title VARCHAR(100),
ISBN VARCHAR(20) UNIQUE,
Genre VARCHAR(50),
Format VARCHAR(10),  
Price DECIMAL(10, 2),
PublisherID INT,
AuthorID INT,
ReleaseDate DATE,
Description TEXT,
FOREIGN KEY (PublisherID) REFERENCES Publishers(PublisherID),
FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID),
CONSTRAINT chk_format CHECK (Format IN ('Physical', 'eBook', 'Audiobook')) 
 );

CREATE TABLE Reviews ( 
ReviewID INT PRIMARY KEY , 
BookID INT, 
CustomerID INT, 
Rating INT CHECK (Rating >= 1 AND Rating <= 5), 
Comment TEXT, 
ReviewDate DATE, 
FOREIGN KEY (BookID) REFERENCES Books(BookID), 
FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID) )
;

CREATE TABLE Orders ( 
OrderID INT PRIMARY KEY , 
CustomerID INT, 
OrderDate DATE, 
TotalAmount DECIMAL(10, 2), 
FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID) 
);

CREATE TABLE OrderDetails ( 
OrderDetailID INT PRIMARY KEY , 
OrderID INT, 
BookID INT, 
Quantity INT,
Price DECIMAL(10, 2), 
FOREIGN KEY (OrderID) REFERENCES Orders(OrderID), 
FOREIGN KEY (BookID) REFERENCES Books(BookID)
 );

## DDL/DML queries for one of the table(Customers).

### Insert query
INSERT INTO Customers (CustomerID, FirstName, LastName, Email, Password, Address, PhoneNumber) VALUES
(1, 'Manav', 'Doe', 'manav.doe@example.com', 'password123', '123 Maple Street', '555-1234'),
(2, 'Mayank', 'Smith', 'mayank.smith@example.com', 'password456', '456 Oak Avenue', '555-5678'),
(3, 'Amara', 'Johnson', 'amara.johnson@example.com', 'password789', '789 Pine Road', '555-8765'),
(4, 'Bob', 'Brown', 'bob.brown@example.com', 'password101', '101 Elm Boulevard', '555-4321'),
(5, 'Carol', 'Davis', 'carol.davis@example.com', 'password202', '202 Cedar Lane', '555-3456'),
(6, 'David', 'Miller', 'david.miller@example.com', 'password303', '303 Birch Drive', '555-6543'),
(7, 'Eve', 'Wilson', 'eve.wilson@example.com', 'password404', '404 Walnut Circle', '555-7890'),
(8, 'Frank', 'Moore', 'frank.moore@example.com', 'password505', '505 Aspen Way', '555-0987'),
(9, 'Grace', 'Taylor', 'grace.taylor@example.com', 'password606', '606 Spruce Street', '555-2345'),
(10, 'Hank', 'Anderson', 'hank.anderson@example.com', 'password707', '707 Willow Terrace', '555-6789');

### Select all customers
SELECT * FROM Customers;

### Select a specific customer by ID
SELECT * FROM Customers WHERE CustomerID = 1;

 ### Update query for a customer's email by using there customer Id.
UPDATE Customers
SET Email = 'john.doe@newexample.com'
WHERE CustomerID = 1;

### Delete a customer by ID
DELETE FROM Customers
WHERE CustomerID = 10;


## SQL Queries

CASE 1:Power writers (authors) with more than X books in the same genre published within the last X years.

SELECT a.AuthorID, a.FirstName, a.LastName, b.Genre, COUNT(b.BookID) AS BookCount, b.ReleaseDate
FROM Authors a 
JOIN Books b ON a.AuthorID = b.AuthorID 
WHERE b.genre= 'Fantasy'
AND b.ReleaseDate >= NOW() - INTERVAL '19 years'
GROUP BY a.AuthorID, a.FirstName, a.LastName, b.Genre , b.ReleaseDate
HAVING COUNT(b.BookID) > 0;

Case 2 : Loyal Customers who has spent more than X dollars in the last year.

SELECT
    c.CustomerID,
    c.FirstName,
    c.LastName,
    SUM(o.TotalAmount) AS TotalSpent
FROM Customers c
JOIN Orders o ON c.CustomerID = o.CustomerID 
WHERE o.OrderDate >= NOW() - INTERVAL '22 year'
GROUP BY c.CustomerID, c.FirstName, c.LastName
HAVING SUM(o.TotalAmount) > 10
ORDER BY TotalSpent DESC;

Case 3:Well Reviewed books that has a better user rating than average.

SELECT 
    b.BookID,
    b.Title,
    b.Genre,
    AVG(r.Rating) AS AvgBookRating
FROM Books b
JOIN Reviews r ON b.BookID = r.BookID
GROUP BY  b.BookID, b.Title, b.Genre
HAVING  AVG(r.Rating) > (
        SELECT AVG(Rating) FROM Reviews
    );

Case 4 : The most popular genre by sales.

SELECT
    b.Genre,
    SUM(od.Quantity * od.Price) AS TotalSales
FROM Books b
JOIN OrderDetails od ON b.BookID = od.BookID
JOIN Orders o ON od.OrderID = o.OrderID
GROUP BY b.Genre
ORDER BY TotalSales DESC
LIMIT 1;

case5: The 10 most recent posted reviews by Customers.

SELECT 
    r.ReviewID, 
    r.BookID, 
    b.Title AS BookTitle, 
    r.CustomerID, 
    c.FirstName, 
    c.LastName, 
    r.Rating, 
    r.Comment, 
    r.ReviewDate 
FROM Reviews r 
JOIN Customers c ON r.CustomerID = c.CustomerID 
JOIN Books b ON r.BookID = b.BookID 
ORDER BY r.ReviewDate DESC 
LIMIT 5;

## Typescript Interface

import { Client } from 'pg';

// creating Database connection configuration
const client = new Client({
  user: 'postgres',
  host: 'localhost',
  database: 'bookstore',
  password: 'postgres',
  port: 5432,
});

// Connecting to the database
client.connect()
  .then(() => console.log('Connected to the database'))
  .catch(err => console.error('Connection error', err.stack));

// initiating  TypeScript interface for a Book
interface Book {
  BookID: number;
  Title: string;
  ISBN: string;
  Genre: string;
  Format: 'Physical' | 'eBook' | 'Audiobook';
  Price: number;
  PublisherID: number;
  AuthorID: number;
  ReleaseDate: string; // Date in ISO format, e.g., 'YYYY-MM-DD'
  Description: string;
}

// Refer below the Function to create a new book record in the database
const createBook = async (newBook: Omit<Book, 'BookID'>): Promise<Book> => {
  const res = await client.query(
    `INSERT INTO Books (Title, ISBN, Genre, Format, Price, PublisherID, AuthorID, ReleaseDate, Description)
     VALUES ($1, $2, $3, $4, $5, $6, $7, $8, $9)
     RETURNING *`,
    [
      newBook.Title,
      newBook.ISBN,
      newBook.Genre,
      newBook.Format,
      newBook.Price,
      newBook.PublisherID,
      newBook.AuthorID,
      newBook.ReleaseDate,
      newBook.Description,
    ]
  );
  return res.rows[0];
};

// Function to read a book from the database by ID
const readBook = async (bookID: number): Promise<Book | null> => {
  const res = await client.query(`SELECT * FROM Books WHERE BookID = $1`, [bookID]);
  return res.rows[0] || null;
};

// Function to update a book
const updateBook = async (bookID: number, updatedFields: Partial<Omit<Book, 'BookID'>>): Promise<Book | null> => {
  const fields = Object.keys(updatedFields);
  const values = Object.values(updatedFields);
  const setClause = fields.map((field, idx) => `${field} = $${idx + 1}`).join(', ');

  const res = await client.query(
    `UPDATE Books SET ${setClause} WHERE BookID = $${fields.length + 1} RETURNING *`,
    [...values, bookID]
  );
  return res.rows[0] || null;
};

// Function to delete a book from the database by ID
const deleteBook = async (bookID: number): Promise<boolean> => {
  const res = await client.query(`DELETE FROM Books WHERE BookID = $1`, [bookID]);
  return res.rowCount > 0;
};

// Example usage
const exampleUsage = async () => {
  const newBook: Omit<Book, 'BookID'> = {
    Title: 'Example Book',
    ISBN: '123-456-789',
    Genre: 'Fiction',
    Format: 'Physical',
    Price: 19.99,
    PublisherID: 1,
    AuthorID: 1,
    ReleaseDate: '2024-01-01',
    Description: 'An example book description',
  };

  const createdBook = await createBook(newBook);
  console.log('Created Book:', createdBook);

  const fetchedBook = await readBook(createdBook.BookID);
  console.log('Fetched Book:', fetchedBook);

  const updatedBook = await updateBook(createdBook.BookID, { Price: 24.99 });
  console.log('Updated Book:', updatedBook);

  const isDeleted = await deleteBook(createdBook.BookID);
  console.log('Deleted Book:', isDeleted);

  await client.end();
};

exampleUsage().catch(console.error);



