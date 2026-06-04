# System Design
System design is the process of planning and structuring the architecture of a software system based on user requirements. It defines how different components of the system will work together to achieve the desired functionality efficiently.
[!ref](https://roadmap.sh/system-design)

## Introduction
### What is System Design
- System design is the process of defining the elements of a system, as well as their interactions and relationships, in order to satisfy a set of specified requirements.
- It involves taking a problem statement, breaking it down into smaller components and designing each component to work together effectively to achieve the overall goal of the system. This process typically includes analyzing the current system (if any) and determining any deficiencies, creating a detailed plan for the new system, and testing the design to ensure that it meets the requirements. It is an iterative process that may involve multiple rounds of design, testing, and refinement.
- In software engineering, system design is a phase in the software development process that focuses on the high-level design of a software system, including the architecture and components
![alt text](image.png)

### How To: System Design?
- **Understand the problem:** Gather information about the problem you are trying to solve and the requirements of the system. Identify the users and their needs, as well as any constraints or limitations of the system.
- **Identify the scope of the system:** Define the boundaries of the system, including what the system will do and what it will not do.
- **Research and analyze existing systems:** Look at similar systems that have been built in the past and identify what worked well and what didn't. Use this information to inform your design decisions.
- **Create a high-level design:** Outline the main components of the system and how they will interact with each other. This can include a rough diagram of the system's architecture, or a flowchart outlining the process the system will follow.
- **Refine the design:** As you work on the details of the design, iterate and refine it until you have a complete and detailed design that meets all the requirements.
- **Document the design:** Create detailed documentation of your design for future reference and maintenance.
- **Continuously monitor and improve the system:** The system design is not a one-time process, it needs to be continuously monitored and improved to meet the changing requirements.
![alt text](image-1.png)


## Importance of System Design
- System design is important for anyone who wants to build a robust, scalable, and efficient software application. 
- Whether you are building a small-scale application or a large one, understanding system design allows you to architect solutions that can handle real-world complexities.

- **Scalability and Reliability:** System design ensures systems can grow and handle increased demand without failure.
- **Efficient Resource Management:** It helps in optimizing resource allocation, ensuring fast and responsive applications.
- **Adaptability:** System design enables the creation of systems that can evolve with changing business needs, reducing long-term costs.
- **Architectural Understanding:** Learning different system architectures (e.g., microservices, monolithic) helps in building applications suited to various needs.


## Performance vs Scalability
A service is scalable if it results in increased performance in a manner proportional to resources added. Generally, increasing performance means serving more units of work, but it can also be to handle larger units of work, such as when datasets grow.

Another way to look at performance vs scalability:
- If you have a performance problem, your system is slow for a single user.
- If you have a scalability problem, your system is fast for a single user but slow under heavy load.

In system design, **Performance** and **Scalability** are often used interchangeably, but they solve two entirely different problems.

Here is the straightforward distinction:

* **Performance** is about doing the *same* amount of work, but doing it *faster*.
* **Scalability** is about doing *more* work, while maintaining the *same* acceptable speed.

### 1. Performance: The "Speed" Metric

Performance measures how efficiently your system processes a single unit of work. If a user clicks a button, how fast does the response come back?

If your application is slow, it suffers from a performance problem.

**Key Metrics:**

* **Latency:** The time it takes to process a single request from start to finish (e.g., 50 milliseconds).
* **Throughput:** The amount of work done in a specific timeframe (e.g., 500 requests per second). While throughput overlaps with scalability, performance focuses on maximizing the throughput of your *existing* resources.

**How to Improve Performance (Optimization):**
You improve performance by optimizing the code, queries, or architecture to do less unnecessary work.

* **Database Tuning:** Adding the correct indexes to a PostgreSQL table so a query scans 10 rows instead of 1,000.
* **Algorithmic Efficiency:** Changing an $O(N^2)$ algorithm to $O(N \log N)$ in your Python code so data processing takes a fraction of the time.
* **Caching:** Storing frequently accessed data (like LLM configurations or static query results) in memory (e.g., Redis) to bypass expensive database hits.
* **Connection Pooling:** Reusing database connections instead of opening and closing a new one for every request.

**Example:**
Imagine you have an AI agent built with LangGraph that retrieves data. If the node execution takes 3 seconds because it does a slow, unindexed database search, adding more servers won't fix it. The request will still take 3 seconds. By adding a database index and implementing a cache, you reduce the execution time to 200 milliseconds. That is a **performance** upgrade.


### 2. Scalability: The "Capacity" Metric

Scalability measures your system's ability to handle increasing amounts of load (traffic, data volume, or concurrent users) without degrading performance.

If your application works perfectly for 100 users but crashes or slows to a crawl when 10,000 users log in, you have a scalability problem.

**Types of Scalability:**

* **Vertical Scaling (Scale-Up):** Adding more power to an existing machine. Upgrading your server from 16GB of RAM and 4 CPU cores to 64GB of RAM and 16 CPU cores. It's the easiest approach but has a hard physical limit and can become very expensive.
* **Horizontal Scaling (Scale-Out):** Adding more machines to the resource pool. Instead of one massive server, you deploy your application across 10 smaller Docker containers behind a load balancer. This approach offers nearly infinite scale.

**How to Improve Scalability (Architecture):**
You improve scalability by distributing the workload.

* **Load Balancing:** Distributing incoming API traffic evenly across multiple server instances.
* **Database Replication:** Creating "read replicas" of your database so multiple servers can handle read-heavy traffic simultaneously.
* **Database Sharding:** Splitting a massive database into smaller, distinct chunks distributed across different machines.
* **Stateless Architecture:** Designing applications so that any server can handle any request, meaning you can spin up or tear down containers on demand without losing user session data.

**Example:**
Your newly optimized LangGraph agent is now incredibly fast (200ms). However, your company launches a massive marketing campaign, and suddenly 5,000 users are hitting the endpoint simultaneously. Your single server's CPU hits 100%, and requests start queuing up or timing out. To fix this, you spin up 5 more identical Docker containers and route traffic through a load balancer. The system handles the 5,000 users effortlessly. That is a **scalability** upgrade.

---

### The Trade-off: Performance vs. Scalability

You can have one without the other, which is why system design requires balancing both.

| Scenario                              | What It Looks Like                                                                                                                                                                       | The Fix                                                            |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **High Performance, Low Scalability** | A highly optimized script running on a single laptop. It returns results in 5ms for one user, but crashes when 10 users hit it at once.                                                  | Scale out horizontally (add load balancers, containerize the app). |
| **Low Performance, High Scalability** | A massive cluster of 100 servers handling a million users seamlessly. However, because the database query is missing an index, *every single user* has to wait 8 seconds for a response. | Optimize the application (add indexes, caching, optimize code).    |

**The Golden Rule:** *Optimize for performance first, then scale to meet demand.* It is much cheaper to make your code run efficiently on one machine than it is to pay for 50 servers to run inefficient code.
![alt text](image-2.png)


## Latency vs Throughput
Latency and throughput are two important measures of a system's performance. 

- **Latency** refers to the amount of time it takes for a system to respond to a request. 
- **Throughput** refers to the number of requests that a system can handle at the same time.

**Latency** and **Throughput** are two of the most fundamental metrics used to evaluate the health and efficiency of a system. While they are closely related, they measure two completely different dimensions of performance: **time** versus **volume**.


### 1. Latency: The Speed of a Single Request

Latency is the total time it takes for a single data packet or request to travel from its source to its destination and back. It is measured in time units, usually milliseconds (ms).

If a user clicks "Submit" on a web form and waits 2 seconds for the confirmation screen, the perceived latency is 2 seconds.

**Types of Latency in System Design:**

* **Network Latency:** The physical time it takes for electrons or light to travel over cables. A user in India querying a server in the US will inherently experience higher network latency (~200ms) than querying a server in Mumbai (~20ms) simply due to the speed of light and physical distance.
* **Disk Latency:** The time it takes for a database to read/write data to storage. Reading from RAM (cache) takes microseconds; reading from a solid-state drive (SSD) takes milliseconds.
* **Compute Latency:** The time the CPU takes to process the logic—for instance, running a complex machine learning inference or calculating a cryptographic hash.

**How to Reduce Latency:**

* **Use Content Delivery Networks (CDNs):** Cache static assets (images, JS, CSS) on edge servers physically closer to the user to eliminate network travel time.
* **In-Memory Caching:** Use tools like Redis or Memcached so your application does not have to wait for slow PostgreSQL disk reads.
* **Optimize Queries & Code:** Add database indexes, remove nested loops, or rewrite inefficient Python code to process data faster.

### 2. Throughput: The Volume of Traffic

Throughput is the maximum rate at which a system can successfully process incoming requests or transfer data over a specific period. It is measured in rates, such as Requests Per Second (RPS), Transactions Per Second (TPS), or Megabytes per second (MBps).

If your web server can successfully handle 5,000 API calls every second without dropping connections, its throughput is 5,000 RPS.

**How to Increase Throughput:**

* **Horizontal Scaling:** Add more servers behind a load balancer. If one Docker container processes 100 RPS, ten containers can process 1,000 RPS.
* **Asynchronous Processing:** Move heavy tasks to message queues (like RabbitMQ or Kafka) and background workers. The main web thread immediately returns a "Success" response (freeing it up for the next request) while the worker processes the job in the background.
* **Database Read Replicas:** Route heavy read traffic across multiple database instances so a single primary database isn't bottlenecking the data flow.


#### The Classic Analogy: The Highway

To visualize the difference, imagine a highway.

* **Latency** is the speed limit. If you drive a car at 100 km/h, it takes you 30 minutes to reach your destination.
* **Throughput** is the number of lanes. If the highway has 2 lanes, 2,000 cars can pass a specific checkpoint every hour.

If you want to improve the system:

* To **improve latency**, you raise the speed limit (e.g., to 150 km/h) so individual cars arrive faster.
* To **improve throughput**, you widen the highway to 4 lanes so more cars can travel simultaneously, even if they are still driving at 100 km/h.


#### The Relationship: Little's Law & System Congestion

Latency and throughput are distinct, but they heavily influence each other, especially under high load. This relationship is mathematically described by **Little's Law**:

$L = \lambda \times W$
*(Concurrency = Throughput $\times$ Latency)*

Where:

* $L$ is the number of requests concurrently in the system.
* $\lambda$ is the throughput (RPS).
* $W$ is the latency (time per request).

**The Congestion Problem:**
If your system has a fixed throughput capacity (e.g., it can only handle 100 concurrent requests), and traffic exceeds that capacity, the incoming requests are forced to wait in a **queue**.

Even if the *compute latency* (the actual time to process the request) is lightning fast, the *perceived latency* for the user skyrockets because their request spent 5 seconds just waiting in line before it was even processed.

Therefore: **High utilization (traffic pushing the limits of throughput) degrades latency.**
![alt text](image-3.png)


## Availability vs Consistency
- **Availability** refers to the ability of a system to provide its services to clients even in the presence of failures. This is often measured in terms of the percentage of time that the system is up and running, also known as its uptime.
- **Consistency** refers to the property that all clients see the same data at the same time. This is important for maintaining the integrity of the data stored in the system.

In distributed systems, it is often a trade-off between availability and consistency. Systems that prioritize high availability may sacrifice consistency, while systems that prioritize consistency may sacrifice availability. Different distributed systems use different approaches to balance the trade-off between availability and consistency, such as using replication or consensus algorithms.


### 1. Consistency: "Everyone sees the same truth"

Consistency means that every read request receives the most recent write. If a user updates their profile picture, and another user views their profile one millisecond later, they must see the *new* picture.

* **How it works:** When data is written to Node A, the system locks and refuses to answer read requests until that new data is successfully copied over to Node B, Node C, etc.
* **The downside:** It introduces latency (waiting for syncs) and risks downtime. If Node B crashes and cannot acknowledge the update, the whole system might refuse to process transactions to protect the integrity of the data.
* **When to use it:** Financial systems, billing platforms, or state-management for complex workflows (like LangGraph agent orchestration) where acting on stale data would cause catastrophic logical errors.

### 2. Availability: "The system is always online"

Availability means that every request receives a non-error response, regardless of the state of the individual nodes. The system guarantees it will answer you, even if the answer is slightly outdated.

* **How it works:** When data is written to Node A, it acknowledges the success immediately. It will eventually sync with Node B in the background. If you read from Node B before the sync happens, Node B just gives you whatever data it currently has.
* **The downside:** You get **Eventual Consistency**. Users might temporarily see stale data.
* **When to use it:** Social media feeds, product reviews, or metrics dashboards. If a YouTube video has 1,000,005 views but you see 1,000,000 for a few minutes, no one is harmed, but if YouTube crashes trying to keep the view count perfectly synced globally, that is a massive problem.

### The CAP Theorem

You cannot talk about Availability and Consistency without mentioning the **CAP Theorem**. It states that a distributed data store can only guarantee two out of the following three traits at the same time:

1. **C**onsistency (Every read gets the most recent write)
2. **A**vailability (Every request gets a response)
3. **P**artition Tolerance (The system continues to operate even if the network between nodes breaks or drops messages)

**Here is the reality check:** Networks *always* fail eventually (cables get cut, routers crash). Therefore, Partition Tolerance (P) is not optional; it is a forced reality of distributed systems.

Because you *must* have **P**, when a network partition happens, you have to choose between **C** and **A**:

* **CP (Consistency + Partition Tolerance):** The network between Node A and Node B breaks. To prevent them from getting out of sync, you shut down Node B. You sacrificed Availability.
* **AP (Availability + Partition Tolerance):** The network breaks. You allow Node A and Node B to keep accepting reads/writes independently. They will get out of sync, but the system stays up. You sacrificed Consistency.

![alt text](image-5.png)


## Consistency Patterns
### 1. Strong Consistency: "Safety First"

After a write completes, **every subsequent read will return that updated value**.

* **How it works:** When you write data to Node A, the system immediately locks. Node A forces Node B and Node C to update their data as well. The write operation does not return a "Success" message to the user until *all* nodes have the new data.
* **The Trade-off:** High latency. The user has to wait for network communication between all nodes to finish before they can move on.
* **When to use it:** Financial transactions, billing systems, or any core transactional data. If you withdraw ₹10,000 from an ATM, the bank's database *must* be strongly consistent before allowing another withdrawal from a different branch.

### 2. Eventual Consistency: "Speed First, Accuracy Later"

After a write completes, reads might return stale data for a short period, but **eventually, all nodes will sync up and return the updated value**.

* **How it works:** When you write data to Node A, Node A instantly returns a "Success" message to the user. Behind the scenes, it asynchronously sends the new data to Node B and Node C.
* **The Trade-off:** Stale reads. If a user quickly reads from Node B before the background sync finishes, they will see the old data.
* **When to use it:** Social media, search engine indexes, or distributed memory stores for AI agents. If you upload a new avatar, it is fine if your friends see the old avatar for an extra 5 seconds. The system remains blazing fast and highly available.

### 3. Weak Consistency: "Best Effort"

After a write completes, **there is no guarantee that subsequent reads will ever see the updated value** unless certain conditions are met.

* **How it works:** Data is written, but the system does not try hard to ensure every node gets it. If the network drops the sync packet, the system just moves on.
* **The Trade-off:** Data loss is acceptable and expected.
* **When to use it:** VoIP calls, live multiplayer gaming, or real-time sensor telemetry. If a video frame drops during a live stream, you don't want the system to pause and wait for it; you just want to see the *next* frame.
![alt text](image-6.png)


## Availability Patterns
### 1. Fail-Over: Protecting the "Compute" Layer

Fail-over applies to the servers that do the actual work—the application logic, the web servers, the API gateways. Because these servers usually don't store permanent data, routing around a failure is mostly about directing traffic.

Imagine a busy restaurant kitchen.

#### i. Active-Passive (The "Understudy" Model): 
With active-passive fail-over, heartbeats are sent between the active and the passive server on standby. If the heartbeat is interrupted, the passive server takes over the active's IP address and resumes service.

* **How it works:** You have a Head Chef (Active) cooking all the meals. You also pay a Sous-Chef (Passive) to just stand there and watch. The Sous-Chef occasionally taps the Head Chef on the shoulder (the "heartbeat"). If the Head Chef collapses, the Sous-Chef immediately takes over.
* **The Catch:** You are paying for a second chef who does absolutely nothing 99% of the time. There is also a brief delay (downtime) while the Sous-Chef figures out where the Head Chef left off.


#### ii. Active-Active (The "Co-Chef" Model):
In active-active, both servers are managing traffic, spreading the load between them.

If the servers are public-facing, the DNS would need to know about the public IPs of both servers. If the servers are internal-facing, application logic would need to know about both servers.

* **How it works:** You have two Head Chefs working side-by-side, splitting the ticket orders 50/50. If one chef collapses, the other chef simply takes on 100% of the orders.
* **The Catch:** There is zero downtime, but if that single remaining chef isn't fast enough to handle the entire restaurant's orders by themselves, they will get overwhelmed and collapse too, bringing down the whole system.



### 2. Replication: Protecting the "Data" Layer

Replication applies to your databases. This is significantly harder than Fail-Over. If a web server dies, you just send the user to another web server. But if a database dies, you risk permanently losing the user's data. You have to keep multiple copies of the data synchronized.

Imagine a system for updating a highly critical ledger book.

#### i. Master-Slave (The "Manager and Tellers" Model):
In this type of replication, one server is designated as the "master" and handles all write operations, while multiple "slave" servers handle read operations. If the master fails, one of the slaves can be promoted to take its place. This type of replication is simpler to set up and maintain compared to Master-Master replication.

* **How it works:** Only the Manager (Master) is allowed to write new entries into the official ledger. However, every time they write something, they hand photocopies to five Tellers (Slaves). If a customer just wants to *check* their balance (a read operation), they ask a Teller. If they want to *deposit* money (a write operation), they must go to the Manager.
* **The Catch:** This is fantastic for systems with massive amounts of reads (like reading a Twitter timeline) and fewer writes. But if the Manager dies, nobody can deposit money until a Teller is officially promoted to Manager.


#### ii. Master-Master (The "Two Managers" Model):
In this type of replication, multiple servers are configured as "masters," and each one can accept read and write operations. This allows for high availability and allows any of the servers to take over if one of them fails. However, this type of replication can lead to conflicts if multiple servers update the same data at the same time, so some conflict resolution mechanism is needed to handle this.

* **How it works:** You have two Managers, both with their own official ledger, both accepting deposits, and constantly yelling across the room to update each other on what they just wrote.
* **The Catch:** This provides incredible availability because either manager can handle anything. But it introduces **Conflicts**. What if Manager A and Manager B both withdraw the last $10 from the same account at the exact same millisecond before they can update each other? Resolving these "split-brain" conflicts requires very complex engineering.
![alt text](image-7.png)


## Background Jobs
Background jobs in system design refer to tasks that are executed in the background, independently of the main execution flow of the system. These tasks are typically initiated by the system itself, rather than by a user or another external agent.

Background jobs can be used for a variety of purposes, such as:
- **Performing maintenance tasks:** such as cleaning up old data, generating reports, or backing up the database.
- **Processing large volumes of data:** such as data import, data export, or data transformation.
- **Sending notifications or messages:** such as sending email notifications or push notifications to users.
- **Performing long-running computations:** such as machine learning or data analysis.

### i. Event Driven
Event-driven invocation uses a trigger to start the background task. Examples of using event-driven triggers include:

- The UI or another job places a message in a queue. The message contains data about an action that has taken place, such as the user placing an order. The background task listens on this queue and detects the arrival of a new message. It reads the message and uses the data in it as the input to the background job. This pattern is known as asynchronous message-based communication.
- The UI or another job saves or updates a value in storage. The background task monitors the storage and detects changes. It reads the data and uses it as the input to the background job.
- The UI or another job makes a request to an endpoint, such as an HTTPS URI, or an API that is exposed as a web service. It passes the data that is required to complete the background task as part of the request. The endpoint or web service invokes the background task, which uses the data as its input.

### ii. Schedule Driven
Schedule-driven invocation uses a timer to start the background task. Examples of using schedule-driven triggers include:

- A timer that is running locally within the application or as part of the application's operating system invokes a background task on a regular basis.
- A timer that is running in a different application, such as Azure Logic Apps, sends a request to an API or web service on a regular basis. The API or web service invokes the background task.
- A separate process or application starts a timer that causes the background task to be invoked once after a specified time delay, or at a specific time.

### iii. Returning Results
Background jobs execute asynchronously in a separate process, or even in a separate location, from the UI or the process that invoked the background task. Ideally, background tasks are "fire and forget" operations, and their execution progress has no impact on the UI or the calling process. This means that the calling process does not wait for completion of the tasks. Therefore, it cannot automatically detect when the task ends.



## Domain Name System (DNS)
The **Domain Name System (DNS)** is famously known as the "phonebook of the internet." Its primary job is to translate human-readable domain names (like `roadmap.sh` or `google.com`) into machine-readable IP addresses (like `142.250.190.46`) so that computers can connect to each other.

However, in system design, DNS is much more than just a lookup table. It is the very **first point of contact** for any user trying to reach your application, and it is a globally distributed, highly available database that can make complex architectural decisions before a user even hits your servers.


### 1. The DNS Resolution Journey

When you type a URL into your browser, finding the IP address isn't a single step. It is a cascading fallback mechanism designed to reduce latency.

1. **Browser & OS Cache:** The browser checks its own memory. If it isn't there, it checks your computer's operating system cache. If you've visited the site recently, the lookup ends here in milliseconds.
2. **Recursive Resolver:** If the IP isn't cached locally, your computer asks a resolver (usually provided by your Internet Service Provider, or a public one like Google's `8.8.8.8`). The resolver checks its massive cache.
3. **Root Name Server:** If the resolver doesn't know, it queries one of the 13 logical Root Servers globally. The Root server says, *"I don't know the IP, but I know who handles `.com` domains."*
4. **TLD (Top-Level Domain) Server:** The resolver then asks the `.com` server. The TLD server says, *"I don't know the IP, but I know the specific Authoritative Server for `roadmap.sh`."*
5. **Authoritative Name Server:** The resolver finally reaches the server where the domain's owner actually configured their DNS records (e.g., AWS Route53, Cloudflare). This server provides the exact IP address, which travels all the way back to your browser.

*System Design Takeaway:* Because this 5-step process takes time (latency), DNS relies heavily on **caching** at every single level with a **TTL (Time to Live)**. If you change your server's IP address, it can take anywhere from 5 minutes to 24 hours for the new IP to propagate globally because everyone is holding onto the cached old IP.
![alt text](image-9.png)
![alt text](image-8.png)

### 2. DNS as a Load Balancer (Advanced Routing)

In modern system design, you don't just point a domain to a single IP address. You use the Authoritative Name Server to intelligently route traffic *before* it even hits your application load balancers.

Here are the advanced DNS routing patterns:

* **Simple Routing:** One domain points to one IP. (Fine for a small blog).
* **Weighted Routing:** You point your domain to two different server IPs. You tell DNS to send 90% of requests to Server A, and 10% to Server B. This is perfect for safely testing a new version of your software (Canary Deployment) with a small group of real users.
* **Latency Routing:** DNS checks where the user is, checks the latency to your various server regions, and returns the IP of the server that will respond the fastest.
* **Geo-Location Routing:** DNS routes traffic based on the user's physical location. A user querying from Bengaluru gets the IP for your `ap-south-1` (Mumbai) cluster, while a user in New York gets the IP for your `us-east-1` (Virginia) cluster. This is crucial for both performance and data compliance laws (like GDPR).
* **Failover Routing:** DNS constantly pings your Primary Server. If the Primary Server stops responding, DNS automatically updates itself to start returning the IP of your Backup (Passive) Server.


## Content Delivery Networks
A **Content Delivery Network (CDN)** is one of the easiest and most cost-effective ways to massively improve both the **performance** (latency) and **scalability** (throughput) of a system.

If the Domain Name System (DNS) is the internet's phonebook, a CDN is the internet's **global supply chain**.

Here is a simple analogy: Imagine you buy a pair of shoes from Amazon. If Amazon only had one warehouse in Seattle, every single customer in the world would have to wait weeks for their shoes. Instead, Amazon builds local warehouses near major cities, stocks them with the most popular shoes, and delivers them the next day.

A CDN does the exact same thing, but for digital assets (Images, Videos, HTML, CSS, JavaScript).

### How a CDN Works

Instead of forcing every user to download your website's logo from your single **Origin Server** in New York, a CDN provider (like Cloudflare, AWS CloudFront, or Akamai) gives you access to a network of thousands of **Edge Servers** distributed globally.

When a user in London visits your site, the DNS routes them to the London Edge Server. The Edge Server hands them the logo in 10 milliseconds. Your Origin Server in New York doesn't even know the interaction happened.

**Why this is crucial for System Design:**

1. **Lowers Latency:** Physics is undefeated. Data traveling from London to New York takes time (~90ms). Data traveling from London to London takes ~5ms.
2. **Protects the Origin:** If your website goes viral and 1 million people visit it, your Origin Server would normally crash. With a CDN, the Edge Servers absorb 99% of that traffic, keeping your Origin Server safe and online.


### Push CDNs vs. Pull CDNs

In the roadmap, you will see CDNs divided into two primary strategies. This refers to how the data actually gets from your Origin Server onto the Edge Servers.

#### 1. Push CDNs

You, the developer, manually upload your files directly to the CDN.

* **How it works:** Whenever you update your website, your deployment pipeline "pushes" the new images and code to the CDN. The CDN then proactively copies those files to all of its global Edge Servers.
* **The Pros:** Content is instantly available everywhere. There is no waiting for the first user to trigger a download.
* **The Cons:** You pay to store *everything* on the CDN, even files that nobody ever looks at.
* **When to use it:** Small websites with a limited amount of static content, or systems where content rarely changes but must be immediately fast when it does.

#### 2. Pull CDNs (Most Common)

The CDN is lazy. It does absolutely nothing until a user asks for a file.

* **How it works:** A user in Tokyo requests `image.png`. The Tokyo Edge Server says, *"I don't have that."* (This is a **Cache Miss**). The Tokyo Edge Server quickly pulls the image from your Origin Server in New York, serves it to the user, and *saves a copy*. The next 10,000 users in Tokyo who ask for `image.png` get the saved copy instantly. (This is a **Cache Hit**).
* **The Pros:** You only use storage space on the CDN for files that people actually want to see. It is highly cost-effective.
* **The Cons:** The very first user in a new region will experience high latency because they have to wait for the Edge Server to pull from the Origin.
* **When to use it:** Large applications with millions of dynamic assets (like YouTube thumbnails, Twitter images, or e-commerce product photos).


## Load Balancer (LB)
If you want to scale horizontally (adding more servers to handle more throughput), you absolutely must have a **Load Balancer (LB)**.

Think of a load balancer as the ultimate traffic cop for your system. When 100,000 users try to access your application, they don't connect to your servers directly. They connect to the load balancer, which then dictates exactly which backend server will process each user's request.

This solves two massive system design problems:

1. **Scalability:** It distributes the workload so no single server gets overwhelmed.
2. **Availability:** It acts as a health monitor. If Server B crashes, the load balancer instantly stops sending traffic to it, routing everyone to Servers A and C instead.

Here is how load balancers are categorized and configured.

---

### 1. Load Balancer vs. Reverse Proxy

These terms are often used interchangeably because modern software (like Nginx, HAProxy, or Traefik) usually does both at the same time. However, their primary purposes are different:

* **Load Balancer:** Its primary job is distributing traffic across a pool of *identical* servers to increase capacity and reliability.
* **Reverse Proxy:** Its primary job is shielding your backend servers from the internet. It sits in front of your servers and handles tasks like SSL termination (decrypting HTTPS so your servers don't have to), caching, and routing traffic to *different* services based on the URL (e.g., sending `/api` requests to a Python backend, and `/blog` requests to a WordPress backend).

### 2. Load Balancing Algorithms

When a request arrives, how does the load balancer decide who gets it? You have to configure an algorithm based on your application's needs.

* **Round Robin:** The simplest method. It distributes requests sequentially: Server 1, then Server 2, then Server 3, then back to Server 1.
* *Best for:* Systems where all servers are exactly the same size and all requests take roughly the same amount of time to process.


* **Least Connections:** The LB checks which server currently has the fewest active requests being processed and sends the new request there.
* *Best for:* Applications where some requests take much longer than others (e.g., one user asks for a simple text file, another asks for a heavy database export). It prevents a server from getting bogged down with too many heavy tasks.


* **IP Hash (Sticky Sessions):** The LB calculates a mathematical hash based on the user's IP address. This guarantees that User A will *always* be routed to Server 1.
* *Best for:* Legacy applications that store user login sessions in the server's local RAM instead of a centralized database like Redis. (Note: In modern system design, we try to avoid this and build "stateless" applications).



### 3. Layer 4 vs. Layer 7 Load Balancing

In system design interviews and cloud architecture, you will frequently be asked at which OSI layer your load balancer operates.

* **Layer 4 (Transport Layer):** * **How it works:** It routes traffic based *only* on the IP address and the TCP/UDP port. It doesn't look at the actual content of the request.
* **Pros:** Blazing fast. Because it isn't decrypting or reading the data, it uses very little CPU and can handle millions of requests per second.
* **Example:** AWS Network Load Balancer (NLB). Perfect for multiplayer gaming or raw database connections.


* **Layer 7 (Application Layer):** * **How it works:** It looks *inside* the HTTP/HTTPS packet. It can read the URL path, the cookies, and the headers.
* **Pros:** Extremely smart routing. It can send `/video` traffic to high-bandwidth servers and `/chat` traffic to high-compute servers.
* **Example:** AWS Application Load Balancer (ALB). Slower than L4, but essential for modern microservice architectures.



---

### Interactive Load Balancer Simulator

To see why choosing the right algorithm matters, try the simulator below.

Set the algorithm to **Round Robin** and watch what happens if one server gets stuck processing a heavy task—the LB will blindly keep sending it traffic, causing a bottleneck. Then, switch to **Least Connections** to see how the LB intelligently routes around the bogged-down server.

You can also simulate a health check failure by "killing" a server mid-traffic.