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