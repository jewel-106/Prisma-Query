 This covers querying, creating, updating, and deleting records, as well as advanced features such as filtering, pagination, and transactions.

### 1. **Setting Up Prisma Client**

Before running any queries, ensure you have the Prisma Client set up in your project:

```javascript
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();
```

### 2. **Fetching Records**

#### a. **Get All Records**

Fetch all records from the `userRole` table.

```javascript
const userRoles = await prisma.userRole.findMany();
console.log(userRoles);
```

#### b. **Get a Single Record by ID**

Fetch a specific record using its ID.

```javascript
const userRole = await prisma.userRole.findUnique({
    where: { id: 1 }, // Replace with the actual ID
});
console.log(userRole);
```

#### c. **Get a Single Record by UUID**

Fetch a record using its UUID.

```javascript
const userRole = await prisma.userRole.findUnique({
    where: { uuid: 'your-uuid-here' }, // Replace with the actual UUID
});
console.log(userRole);
```

### 3. **Creating Records**

#### a. **Create a New Record**

Insert a new user role into the database.

```javascript
const newUserRole = await prisma.userRole.create({
    data: {
        roleTitle: 'Admin',
        activeStatus: 1,
        // Include other necessary fields
    },
});
console.log(newUserRole);
```

### 4. **Updating Records**

#### a. **Update a Record**

Modify an existing record using its ID.

```javascript
const updatedUserRole = await prisma.userRole.update({
    where: { id: 1 }, // Replace with the actual ID
    data: {
        roleTitle: 'Super Admin',
        activeStatus: 1,
        // Include other fields to update
    },
});
console.log(updatedUserRole);
```

### 5. **Deleting Records**

#### a. **Delete a Record**

Remove a specific record from the database using its ID.

```javascript
const deletedUserRole = await prisma.userRole.delete({
    where: { id: 1 }, // Replace with the actual ID
});
console.log(deletedUserRole);
```

### 6. **Using Filters**

#### a. **Fetching with Conditions**

Fetch records that meet specific conditions.

```javascript
const userRoles = await prisma.userRole.findMany({
    where: {
        activeStatus: 1, // Fetch only active roles
        roleTitle: { contains: 'Admin' }, // Filter by title
    },
});
console.log(userRoles);
```

### 7. **Combining Conditions**

#### a. **Using AND Conditions**

Fetch records that satisfy multiple conditions.

```javascript
const userRoles = await prisma.userRole.findMany({
    where: {
        AND: [
            { activeStatus: 1 },
            { roleTitle: { contains: 'Admin' } }
        ]
    }
});
console.log(userRoles);
```

#### b. **Using OR Conditions**

Fetch records that satisfy at least one of the conditions.

```javascript
const userRoles = await prisma.userRole.findMany({
    where: {
        OR: [
            { roleTitle: { contains: 'Admin' } },
            { roleTitle: { contains: 'User' } }
        ]
    }
});
console.log(userRoles);
```

### 8. **Pagination**

#### a. **Implementing Pagination**

Fetch a limited number of records with pagination.

```javascript
const userRoles = await prisma.userRole.findMany({
    skip: 0, // Number of records to skip (offset)
    take: 10, // Number of records to fetch
    orderBy: { id: 'desc' }, // Sorting the results
});
console.log(userRoles);
```

### 9. **Sorting**

#### a. **Sorting Results**

Sort results based on a specific field.

```javascript
const userRoles = await prisma.userRole.findMany({
    orderBy: {
        createdAt: 'asc', // Ascending order
    },
});
console.log(userRoles);
```

### 10. **Transactions**

#### a. **Using Transactions**

Execute multiple queries in a single transaction to ensure atomicity.

```javascript
const [userRole1, userRole2] = await prisma.$transaction([
    prisma.userRole.create({ data: { roleTitle: 'Editor' } }),
    prisma.userRole.create({ data: { roleTitle: 'Viewer' } }),
]);
console.log(userRole1, userRole2);
```

### 11. **Aggregations**

#### a. **Count Records**

Count the number of records in a table.

```javascript
const count = await prisma.userRole.count();
console.log(`Total User Roles: ${count}`);
```

#### b. **Aggregating Data**

Get aggregated data (e.g., average, sum) from a field.

```javascript
const avgActiveStatus = await prisma.userRole.aggregate({
    _avg: {
        activeStatus: true,
    },
});
console.log(avgActiveStatus);
```

### 12. **Error Handling**

#### a. **Handling Errors**

Use try-catch blocks to handle potential errors during queries.

```javascript
try {
    const userRole = await prisma.userRole.findUnique({
        where: { id: 1 },
    });
    console.log(userRole);
} catch (error) {
    console.error("Error fetching user role:", error);
}
```

### 13. **Closing the Prisma Client**

Don't forget to close the Prisma Client connection when done.

```javascript
await prisma.$disconnect();
```
