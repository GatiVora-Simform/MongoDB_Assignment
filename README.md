# MongoDB_Assignment

## Queries

### 1.  Batch Create with minimum 100 records in MongoDb (create batch).
This script will generate 150 product records, with 20 duplicates.
```javascript
let batch = [];
const categories = ["Electronics", "Clothes", "Home Decor", "Bakery"];

for (let i = 0; i < 150; i++) {
  batch.push({
    name: `Product ${i + 1}`,
    category: categories[i % categories.length],
    price: (Math.random() * 1000 + 10).toFixed(2),
    stock_quantity: Math.floor(Math.random() * 100),
    created_at: new Date(),
  });
}

const duplicates = 20;

for (let i = 0; i < duplicates; i++) {
  const idx = Math.floor(Math.random() * batch.length);
  const duplicate = { ...batch[idx] };
  batch.push(duplicate);
}

db.products.insertMany(batch);
```

### 2. Batch Update with minimum 100 records  in MongoDB (update batch).
This query performs a batch update on the products collection. It sets the price of all products in the "Electronics" category to 100.
```javascript
db.products.updateMany( 
{ category: "Electronics" }, { $set: { price: 100 } } 
); 
```

### 3. Perform indexing on particular 3 fields in MongoDB.
This query creates indexes on the name, category, and stock_quantity fields in the products collection.
```javascript
db.products.createIndex({"name":1})  
db.products.createIndex({"category":1})  
db.products.createIndex({"stock_quantity":1}) 
```
### 4. Find Duplicates Using Aggregation
This aggregation pipeline finds duplicate products based on the name field. It groups the products by name and returns those with a count greater than 1.
```javascript
db.products.aggregate([
  {
    $group: {
      _id: { name: "$name" },
      count: { $sum: 1 },
    },
  },
  {
    $match: {
      count: { $gt: 1 },
    },
  },
  {
    $project: {
      _id: 0,
      name: "$_id.name",
      category: "$_id.category",
      count: 1,
    },
  },
]);
```
