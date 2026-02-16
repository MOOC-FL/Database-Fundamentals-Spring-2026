| Property Category | Property Name | Description | Example |
| :--- | :--- | :--- | :--- |
| **Core Properties** | **Name** | Unique identifier for the column within the table. | `CustomerID`, `FirstName`, `Email` |
| | **Data Type** | Defines the kind of data the column can hold. | `INT`, `VARCHAR(50)`, `DATE`, `DECIMAL` |
| | **Nullability** | Determines if the column can be left empty (`NULL`) or must contain data (`NOT NULL`). | `NOT NULL`, `NULL` |
| **Data Types** | **Numeric** | Stores numbers. | `INT`, `BIGINT`, `DECIMAL(10,2)`, `FLOAT` |
| | **String (Text)** | Stores text characters. | `VARCHAR(255)`, `CHAR(10)`, `TEXT` |
| | **Date/Time** | Stores dates, times, or both. | `DATE`, `DATETIME`, `TIMESTAMP`, `TIME` |
| | **Boolean** | Stores true/false logic. | `BOOL`, `BIT` |
| | **Binary** | Stores binary data like images or files. | `BLOB` |
| **Key Properties & Constraints** | **PRIMARY KEY** | Uniquely identifies each row. Cannot be `NULL`. Only one per table. | `EmployeeID INT PRIMARY KEY` |
| | **FOREIGN KEY** | Creates a link to the Primary Key of another table. | `DeptID INT FOREIGN KEY REFERENCES Departments(DeptID)` |
| | **UNIQUE** | Ensures all values in a column are different. | `Email VARCHAR(255) UNIQUE` |
| | **DEFAULT** | Provides a fallback value if no value is specified during insert. | `HireDate DATE DEFAULT (CURRENT_DATE)` |
| | **CHECK** | Validates data against a specific condition. | `Salary DECIMAL(10,2) CHECK (Salary > 0)` |
| **Performance** | **INDEX** | Speeds up data retrieval queries on a column. | `CREATE INDEX idx_lastname ON Employees(LastName);` |
