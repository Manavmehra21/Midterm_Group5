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

