
Paytm accepts payments from various sources including Paytm Wallet, Paytm Postpaid, Paytm UPI, netbanking, debit and credit cards.

This note covers:
- [[Overview]]
- [[Challenges]]
- [[Solutions]]
- [[Where They Are Used in Paytm Infrastructure]]
- [[Data Sharding in Detail]]
- [[Example: Paytm Wallet Transactions]]
- [[References]]

---

## [[Overview]]

Paytm processes payments from:
- Paytm Wallet
- Paytm Postpaid
- Paytm UPI
- Netbanking
- Debit/Credit Cards

Challenges addressed by Database Engineering:
- High throughput
- Managing large clusters (IoT devices, merchant accounts)
- Open-source adoption

Solutions implemented:
- [[Consensus Algorithms (Paxos)]]
- [[Data Sharding]]
- [[InnoDB Engine]]
- [[Data Consistencies]]

Outcome:
Enhanced scalability, stability, and reliability in Paytm's transaction processing.

---

## [[Challenges]]

- **High Throughput of Read and Write Operations**: Needed to handle large transaction volumes efficiently.
- **Continuous Synchronization**: Servers must remain in sync for consistency.
- **Handling Large Clusters**: Scaling with IoT devices and multiple transaction types.

See: [[Solutions]]

---

## [[Solutions]]

1. [[Consensus Algorithms (Paxos)]]
2. [[Data Sharding]]
3. [[InnoDB Engine]]
4. [[Data Consistencies]]

---

### [[Consensus Algorithms (Paxos)]]

**Definition**: Agreement among distributed machines to maintain consistent state.

**Why Paxos?**
- Simpler implementation than Raft.
- Single leader approach for efficiency.

**Usage in Paytm**:
- Transactional integrity across multiple data centers.
- Data replication for fault tolerance.

See: [[Where They Are Used in Paytm Infrastructure]]

---

### [[Data Sharding]]

**Purpose**  
Enhance write scalability and performance.

**Process**
- Partition data using sharding key/hash.
- Logical DB executes changes → propagated to physical shards.
- Uses LSM tree for write performance.

See: [[Data Sharding in Detail]]

---

### [[InnoDB Engine]]

**Why Chosen**
- Open-source, robust, stable.
- ACID compliance for transactions.

**Paytm Customization**
- Optimized for high-throughput workloads.
- Tailored for payment-specific stability.

See: [[Where They Are Used in Paytm Infrastructure]]

---

### [[Data Consistencies]]

**Types**
- Strong
- Weak
- Medium

**Paytm Usage**
- Strong for financial accuracy.
- Medium/Weak for non-critical views like history.

See: [[Where They Are Used in Paytm Infrastructure]]

---

## [[Where They Are Used in Paytm Infrastructure]]

**Consensus Algorithms**
- Transactional integrity
- Data replication

**Data Sharding**
- High throughput payment systems
- IoT data management

**InnoDB**
- Transaction databases
- Payment processing

**Data Consistencies**
- Strong for account balances
- Medium/weak for user-facing features

See: [[Data Sharding in Detail]]

---

## [[Data Sharding in Detail]]

**How It Works**
- Horizontal sharding
- Vertical sharding (less common)
- Sharding key selection critical

**Benefits**
- Performance
- Scalability
- Fault tolerance
- Maintenance efficiency

See example: [[Example: Paytm Wallet Transactions]]

---

## [[Example: Paytm Wallet Transactions]]

**Scenario**: Festive season high-load processing using sharding.

**Steps**:
1. Select sharding key (e.g., user_id).
2. Route queries to correct shard.
3. Process in parallel across shards.
4. Balance load to avoid bottlenecks.
5. Maintain fault tolerance.

**Benefits**:
- Scalability
- High throughput
- Reliability

See: [[Data Sharding in Detail]]

---

## [[References]]

- https://paytm.com/blog/engineering/
- https://aws.amazon.com/what-is/database-sharding/
- https://www.redhat.com/architect/pros-and-cons-sharding
- https://medium.com/javarevisited/what-is-database-sharding-scaling-your-data-horizontally-1dc12b33193f
