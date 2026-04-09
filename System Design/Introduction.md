---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Relational vs Non-Relational Databases

Relational Databases (SQL / RDBMS)
        • Examples: MySQL, Oracle, PostgreSQL
        • Data stored in structured tables (rows & columns)
        • Supports join operations using SQL

Non-Relational Databases (NoSQL)
        • Examples: CouchDB, Neo4j, Cassandra, DynamoDB
        • Categories: Key-Value, Graph, Column, Document
        • Joins generally not supported

When to Use NoSQL
        • Applications needing very low latency
        • Unstructured or non-relational data
        • Simple serialization/deserialization (JSON, XML, YAML)
Large-scale data storage ^enkwTZGp

Scaling

Vertical Scaling (Scale Up)
        • Add more power (CPU, RAM) to a single server
        • Best for low traffic; simple to implement
        • Limitations:
                ○ Maximum upgrade limits (CPU, memory)
                ○ No failover → single point of failure


Horizontal Scaling (Scale Out)
        • Add more servers to a resource pool
        • Suitable for large-scale applications
        • Provides redundancy and reliability
 ^cBxMMXXs

Load Balancer
        • Distributes incoming traffic across multiple web servers
        • Clients connect to the public IP of the load balancer
        • Private IPs used for internal secure communication

Advantages
        • Failover: Automatically reroutes traffic from unhealthy servers
        • High Availability: Keeps service running even if one server fails
        • Scalability: Easily add servers; load balancer handles distribution


Database Replication
        • Replicates data from a master database to one or more slave databases
        • Master DB: Handles writes (insert, update, delete)
        • Slave DBs: Handle reads only
        • Useful for applications with a high read-to-write ratio

Advantages
        • Performance: Read load spread across slaves
        • Reliability: Data stored across multiple machines
        • High Availability: Application stays up even if one DB fails
 ^i82SHlBA

Content Delivery Network (CDN)
A CDN is a geographically distributed network of servers that deliver static content quickly by caching assets like images, videos, CSS, and JavaScript.

How a CDN Works
        • User requests → routed to the nearest CDN server
        • Closer distance = faster response
(Example: Faster for users in Los Angeles if the CDN server is in San Francisco)

Considerations When Using a CDN

1. Cost
        • Charged based on data transfers
        • Avoid caching rarely accessed files

2. Cache Expiry
        • Controls how long content stays cached
        • Too long = outdated content
        • Too short = frequent origin fetches

3. CDN Fallback
        • Plan for CDN failures
        • Application should load assets from the origin server if CDN is unreachable

4. Invalidating Files
To refresh cached files before expiry:
        • Use CDN APIs for cache invalidation
        • Use object versioning
 ^gsTuf47s

Scaling from Zeros to Millions ^9RHbuoxz

Key Points:
-----

        • Strong signals:
                ○ Collaboration
                ○ Work under pressure
                ○ Resolve ambiguity constructively
        • Red flags:
                ○ Over engineering
                ○ Narrow mindedness
                ○ Stubbornness
 ^BBGvKCjy

4 Step process:
-----

        1. Understand the problem and establish the design scope.
        2. Propose high-level design and get buy-in
        3. Design deep dive:
        4. Wrap up
 ^Mg0Vamzn

• Understand the problem and establish the design scope.
        ○ Clarify requirements and assumptions
                § Understand the exact requirements
        ○ Clarifications:
                § Use cases
                § Features
                § Scale
                        □ In-app metrics
                        □ Traffic volume - DAU
                § Tech clarifications
        ○ Important to understand requirements and clarify ambiguities.
 ^EKE0KSxJ

• Propose high-level design and get buy-in
        ○ Initial blueprint for the design
                § Ask feedback and collaborate.
        ○ Draw box diagrams with key components.
        ○ Do back of the envelope calculations to evaluate the scale constraints.
        ○ Go through few use cases to check feasibility.



 ^k3sNIEtI

• Design deep dive:
        ○ Identify and prioritize components in the architecture.
                ○ System performance characteristics:
                        § Focus on bottlenecks and resource estimations.
                ○ Details of some design components
                        § URL Shortener:
                                □ Hash function
                        § Chat system
                                □ Reduction of latency
                                □ Support offline/online status.
        ○ Don't get carried away on minute details
 ^AEHew5I9

        • Wrap up:
                ○ Identify bottlenecks and discuss potential improvements
                ○ Recap of the design
                ○ Error cases:
                        § Server failure
                        § Network loss
                ○ Monitoring metrics and error logs
                ○ Handling next scale of users
                        § Changes to be made in the system to handle higher volume of customers.
                
 ^9hwxZTUP

        • Dos
                ○ Always ask for clarification. Do not assume your assumption is correct.
                ○ Understand the requirements of the problem.
                ○ There is neither the right answer nor the best answer. A solution designed to solve the
                ○ problems of a young startup is different from that of an established company with millions
                ○ of users. Make sure you understand the requirements.
                ○ Let the interviewer know what you are thinking. Communicate with your interview.
                ○ Suggest multiple approaches if possible.
                ○ Once you agree with your interviewer on the blueprint, go into details on each
                ○ component. Design the most critical components first.
                ○ Bounce ideas off the interviewer. A good interviewer works with you as a teammate.
                ○ Never give up.

        • Don’ts
                ○ Don't be unprepared for typical interview questions.
                ○ Don’t jump into a solution without clarifying the requirements and assumptions.
                ○ Don’t go into too much detail on a single component in the beginning. Give the high-
                ○ level design first then drills down.
                ○ If you get stuck, don't hesitate to ask for hints.
                ○ Again, communicate. Don't think in silence.
                ○ Don’t think your interview is done once you give the design. You are not done until your
                ○ interviewer says you are done. Ask for feedback early and often.
 ^DYoBMaDD

## Embedded Files
5068417d03bd99416554a6c2b28465ba3ab6c970: [[Pasted Image 20260409172835_982.png]]

%%
## Drawing
```compressed-json
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

gAgqEAA=
```
%%