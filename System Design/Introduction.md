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

🧮 A FRAMEWORK FOR SYSTEM DESIGN INTERVIEWS
 ^tOSSTEEM

## Embedded Files
5068417d03bd99416554a6c2b28465ba3ab6c970: [[Pasted Image 20260409172835_982.png]]

%%
## Drawing
```compressed-json
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

USOmBOABAik6eEAOKgqCACCoQAA=
```
%%