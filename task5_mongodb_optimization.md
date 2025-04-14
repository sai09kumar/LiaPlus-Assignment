
# Task 5: Database & Storage Optimization (MongoDB)

## ✅ Optimization Techniques

### 1. Indexing
MongoDB supports various types of indexes like **single field**, **compound**, and **text indexes**. Indexing improves read/query performance significantly.

**Example:**
```js
// Before: slow query without index
db.users.find({ email: "user@example.com" })

// Optimization: create index
db.users.createIndex({ email: 1 })
```

---

### 2. Query Optimization
Write efficient queries by filtering and projecting only required fields. This reduces the amount of data MongoDB needs to process and return.

**Example:**
```js
// Before: returns full documents
db.orders.find({ status: "shipped" })

// Optimized: return only needed fields
db.orders.find({ status: "shipped" }, { order_id: 1, shipped_date: 1, _id: 0 })
```

---

### 3. Data Partitioning (Sharding)
Use **sharding** to distribute large collections across multiple servers, improving performance and scalability.

**Example:**
```js
// Enable sharding on a database and collection
sh.enableSharding("ecommerce")
sh.shardCollection("ecommerce.orders", { customer_id: "hashed" })
```

---

## 📈 Query Example: Before vs After Optimization

**Collection:** `products`  
**Use Case:** Frequently filter by `category` and `price`

---

### 🔍 Before Optimization

```js
db.products.find({ category: "electronics", price: { $lt: 500 } })
```

- No index
- Execution plan: `"COLLSCAN"` (collection scan)

---

### ⚙️ Optimization Applied

```js
db.products.createIndex({ category: 1, price: 1 })
```

---

### ⚡ After Optimization

```js
db.products.find({ category: "electronics", price: { $lt: 500 } })
```

- Uses index
- Execution plan: `"IXSCAN"` (index scan)
- Faster response, lower CPU

---

## 🛡️ Conclusion

Optimizing MongoDB with indexes, efficient queries, and sharding ensures high performance, scalability, and reduced latency, especially for large datasets or high-traffic applications.
