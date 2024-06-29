# Midterm_Group5
# Online Bookstore Database Design

## Duties Assigned to Team Members
- **Database Design**: Manav Mehra
- **Database Implementation**: Mayank Sharma
- **Query Development**: Emmanuella Owulu Amara
- **API Development**: Manav, Mayank and Amara

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

### DDL/DML queries for one of the table(Customers).

## Insert query
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


