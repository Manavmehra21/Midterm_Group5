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

###Sql Queries for creating above mentioned tables in Database

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
Format VARCHAR(10),  -- Changed ENUM to VARCHAR
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

