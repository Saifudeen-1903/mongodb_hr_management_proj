### A MongoDB-based backend project for HR Management System. 

This project demonstrates how to use collections, schema validation, and CRUD operations for managing employees, departments, and attendance records.


### Project Overview:

This project can help in managing three components:

* **Employees** – Staff working in the organization  
* **Departments** – Organizational units like HR, IT, Finance  
* **Attendance** – Employee daily attendance tracking  

MongoDB is used with schema validation to ensure consistent and reliable HR data.



### Database Setup:

```js
use hrDB
```
Creates or switches to the hrDB database.

### Collections and Schema Design
### 1. Employees Collection
   
### Create Collection

```js
db.createCollection("employees", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["empId", "name", "salary"],
      properties: {
        empId: { bsonType: "string" },
        name: { bsonType: "string" },
        salary: { bsonType: "int" },
        isActive: { bsonType: "bool" }
      }
    }
  }
})
```
### Example Insert
```js
db.employees.insertOne({
  empId: "EMP101",
  name: "Ravi Kumar",
  salary: 50000,
  isActive: true
})
```
Invalid Insert (Fails)
```js
db.employees.insertOne({
  empId: "EMP102",
  salary: 30000
})
```
### 2. Departments Collection
   
Create Collection with Advanced Validation
```js
db.createCollection("departments", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["deptId", "deptName", "budget"],
      properties: {
        deptId: {
          bsonType: "string",
          pattern: "^DPT[0-9]{3}$"
        },
        deptName: {
          enum: ["HR", "IT", "Finance", "Sales"]
        },
        budget: {
          bsonType: "number",
          minimum: 10000,
          maximum: 1000000
        }
      }
    }
  },
  validationLevel: "strict",
  validationAction: "error"
})
```

### Valid Insert
```js
db.departments.insertOne({
  deptId: "DPT101",
  deptName: "IT",
  budget: 500000
})
```

### Invalid Insert (Fails)
```js
db.departments.insertOne({
  deptId: "101",
  deptName: "Admin",
  budget: 5000
})
```
### 3. Attendance Collection
   
### Create Collection
```js
db.createCollection("attendance")
```
### Add Validation using collMod
```js
db.runCommand({
  collMod: "attendance",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["empId", "date", "status"],
      properties: {
        empId: {
          bsonType: "string",
          pattern: "^EMP[0-9]{3}$"
        },
        date: {
          bsonType: "string"
        },
        status: {
          enum: ["Present", "Absent", "Leave"]
        }
      }
    }
  },
  validationLevel: "moderate"
})
```
### CRUD Operations
### Create
```js
db.employees.insertOne({
  empId: "EMP201",
  name: "Anita",
  department: "HR",
  salary: 45000,
  isActive: true
})
```
### Read
```js
db.employees.find()
db.employees.find({ salary: { $gt: 40000 } })
```
### Update
```js
db.employees.updateOne(
  { empId: "EMP201" },
  { $set: { salary: 48000 } }
)
```
### Delete
```js
db.employees.deleteOne({ empId: "EMP201" })
```
### Utility Commands
### Show Collections
```js
shows collection
```
### Drop Collection
```js
db.employees.drop()
```
### Get Collection Info
```js
db.getCollectionInfos({ name: "departments" })
```

### Conclusion

This project helps in understanding how MongoDB can be used to build a structured HR Management System. By applying schema validation and CRUD operations, the system ensures data integrity, consistency, and efficient management of employee-related data.
