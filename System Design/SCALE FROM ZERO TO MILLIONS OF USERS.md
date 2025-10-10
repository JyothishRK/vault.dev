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
