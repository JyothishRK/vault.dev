# Relational vs Non-Relational Databases

## Relational Databases (SQL / RDBMS)
- Examples: MySQL, Oracle, PostgreSQL  
- Data stored in structured tables (rows & columns)  
- Supports join operations using SQL  

## Non-Relational Databases (NoSQL)
- Examples: CouchDB, Neo4j, Cassandra, DynamoDB  
- Categories: Key-Value, Graph, Column, Document  
- Joins generally not supported  

### When to use Non-Relational Databases (NoSQL)
- Applications needing **very low latency**  
- **Unstructured** or **non-relational** data  
- Simple **serialization/deserialization** (JSON, XML, YAML)  
- Handling **large-scale data storage**  

---

# Scaling

## Vertical Scaling (Scale Up)
- Add more power (CPU, RAM) to a single server  
- Best for **low traffic**; simple to implement  
- **Limitations:**  
  - Hard limit on CPU & memory upgrades  
  - No failover/redundancy → single point of failure  

## Horizontal Scaling (Scale Out)
- Add more servers to resource pool  
- Preferred for **large-scale applications**  
- Provides better **redundancy & reliability** compared to vertical scaling  

---

# Load Balancer
- Distributes incoming traffic evenly across web servers  
- Clients connect to the **public IP** of the load balancer, not directly to servers  
- **Private IPs** used for secure server-to-server communication (not exposed to internet)  

### Advantages
- **Failover:** If one server goes offline, traffic is routed to healthy servers  
- **High availability:** Keeps the website online even if a server fails  
- **Scalability:** Can add more servers as traffic grows, load balancer auto-distributes requests  

---

# Database Replication
- **Definition:** Copying data from a master database to one or more slave databases  
- **Master DB:** Handles **write operations** (insert, update, delete)  
- **Slave DBs:** Receive data from master, handle **read operations** only  
- **Usage:** Supports applications with **high read-to-write ratio**; typically more slaves than masters for scalability and performance  

![[Pasted image 20251001234720.png]]

### Advantages of Database Replication
- **Better Performance:** Reads distributed across slave nodes, writes handled by master → more queries processed in parallel  
- **Reliability:** Data replicated across multiple locations → protected against server failures or disasters  
- **High Availability:** Website remains operational even if one database goes offline, as data is accessible from other servers  

---
# Content Delivery Network (CDN)

A **CDN (Content Delivery Network)** is a geographically distributed network of servers designed to deliver **static content** efficiently.  
These servers cache assets like **images, videos, CSS, and JavaScript files** to reduce latency and improve load times.

---

## How CDN Works
When a user visits a website:
- The **CDN server closest to the user** (based on geographic location) delivers static content.
- The **shorter the distance**, the **faster the response time**.

Example:
- If CDN servers are in *San Francisco*, users in *Los Angeles* will experience faster load times than users in *Europe*.

> Dynamic content caching (based on path, query strings, cookies, headers, etc.) is an advanced concept and not covered here.

---

## Considerations When Using a CDN

### 1. Cost
- CDNs are managed by third-party providers and charge for **data transfers** (inbound & outbound).  
- Avoid caching **rarely used assets** — it adds cost without meaningful benefit.

### 2. Cache Expiry
- Important for **time-sensitive content**.  
- Expiry time must balance between **freshness** and **efficiency**:
  - Too long → outdated content may be served.  
  - Too short → frequent reloads from the origin server.

### 3. CDN Fallback
- Plan for **CDN outages**.  
- Applications should:
  - Detect CDN failures.
  - Fetch resources directly from the **origin server** when needed.

### 4. Invalidating Files
You can remove or refresh cached files **before expiry** by:

- **Using CDN APIs** → Invalidate specific objects.
- **Using Object Versioning** → Change file URLs with a version parameter.

Example:
```text
image.png?v=2
```

---


