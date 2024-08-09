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
```sql

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
```
## DDL/DML queries for one of the table(Customers).

### Insert query
```sql
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
```
### Select all customers
```sql
SELECT * FROM Customers;
```
### Select a specific customer by ID
```sql
SELECT * FROM Customers WHERE CustomerID = 1;
```
### Update query for a customer's email by using there customer Id.
```aql
UPDATE Customers
SET Email = 'john.doe@newexample.com'
WHERE CustomerID = 1;
```
### Delete a customer by ID
```sql
DELETE FROM Customers
WHERE CustomerID = 10;
```


## SQL Queries

CASE 1:Power writers (authors) with more than X books in the same genre published within the last X years.
```sql
SELECT a.AuthorID, a.FirstName, a.LastName, b.Genre, COUNT(b.BookID) AS BookCount, b.ReleaseDate
FROM Authors a 
JOIN Books b ON a.AuthorID = b.AuthorID 
WHERE b.genre= 'Fantasy'
AND b.ReleaseDate >= NOW() - INTERVAL '19 years'
GROUP BY a.AuthorID, a.FirstName, a.LastName, b.Genre , b.ReleaseDate
HAVING COUNT(b.BookID) > 0;
```
Case 2 : Loyal Customers who has spent more than X dollars in the last year.
```sql
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
```
Case 3:Well Reviewed books that has a better user rating than average.
```sql
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
```
Case 4 : The most popular genre by sales.
```sql
SELECT
    b.Genre,
    SUM(od.Quantity * od.Price) AS TotalSales
FROM Books b
JOIN OrderDetails od ON b.BookID = od.BookID
JOIN Orders o ON od.OrderID = o.OrderID
GROUP BY b.Genre
ORDER BY TotalSales DESC
LIMIT 1;
```
case5: The 10 most recent posted reviews by Customers.
```sql
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
```
## Typescript Interface
index.ts file 
```
export type { default } from "./persistenceService";
```
```export default interface PersistenceService {
  create: (name: string) => void;
  // drop: (name: string) => void;
  insert: <T = unknown>(content: T, location: string) => void;
  // update: <T = unknown>(content: T, location: string) => void;
  // delete: <T = unknown>(content: T, location: string) => void;
}


```
!Alt Text
![ER_diagram (1)](https://github.com/user-attachments/assets/689a56ae-0117-4b7d-9642-3a6bedd30dcd)



