# My Approach to Systems Design

By Mitchell Bryson.

# Gather requirements and scope

The goal of systems design is to turn requirements into a testable hypothesis by answering the question, “How should we build this?”.

### What is it? Who is it for? What does it do?

I start by asking questions to gather information on the requirements and scope. It’s OK to make assumptions in the answers, as that is what we will be testing later.

- What problem are we solving / job are we doing?
- Who is it for and how will they be using it?

Example requirements:

- Customers can buy Product A
- Customers buy subscribe to receive the product monthly
- Product A will be delivered once payment is made
- Customer should be notified of delivery updates

# Entities, services and their relationships

With an idea of the requirements I create a **list of entities, and services** to manage them within system.

| Entities | Services |
| --- | --- |
| Customer | Billing |
| Product A | Delivery |
| Subscription | Notifications |
| Payment |  |
| Delivery Update |  |
| Notification |  |

Then I can create a high level diagram showing the relationship between entities and services.

![Diagram of entities and their relation to services. [https://whimsical.com/system-design-1-entities-and-services-VybdUGHJNcsspgWKgkNXv8](https://whimsical.com/system-design-1-entities-and-services-VybdUGHJNcsspgWKgkNXv8)](My%20Approach%20to%20Systems%20Design%207905716aaf20402d911029f322d19b69/Untitled.png)

Diagram of entities and their relation to services. [https://whimsical.com/system-design-1-entities-and-services-VybdUGHJNcsspgWKgkNXv8](https://whimsical.com/system-design-1-entities-and-services-VybdUGHJNcsspgWKgkNXv8)

# Data, Services and their API

With further questions I can add more detail to each entity and it’s data, as well as service APIs.

Data Questions:

- What information do we need to capture?
- What information do we need to present?
- In what format?
- How accurate does it need to be?
    - Timing, Location
- How secure does it need to be?
    - Authentication, Authorisation, Encryption

Every answer to a question gives more input into the design. 

Now I can document the **initial API for the services**.

![Diagram of services API. [https://whimsical.com/system-design-2-services-api-WQwYA2MytEuVUwEwMZqF4N](https://whimsical.com/system-design-2-services-api-WQwYA2MytEuVUwEwMZqF4N)](My%20Approach%20to%20Systems%20Design%207905716aaf20402d911029f322d19b69/Untitled%201.png)

Diagram of services API. [https://whimsical.com/system-design-2-services-api-WQwYA2MytEuVUwEwMZqF4N](https://whimsical.com/system-design-2-services-api-WQwYA2MytEuVUwEwMZqF4N)

As well as the **entity data and their relations** to each other.

![Diagram of entity data and relationships. [https://whimsical.com/system-design-3-entity-data-LqhwBWZeDTsUZgET1rBCkf](https://whimsical.com/system-design-3-entity-data-LqhwBWZeDTsUZgET1rBCkf)](My%20Approach%20to%20Systems%20Design%207905716aaf20402d911029f322d19b69/Untitled%202.png)

Diagram of entity data and relationships. [https://whimsical.com/system-design-3-entity-data-LqhwBWZeDTsUZgET1rBCkf](https://whimsical.com/system-design-3-entity-data-LqhwBWZeDTsUZgET1rBCkf)

# Infrastructure and Technology choices

More questions give me the information required to plan out the **infrastructure** and make **technology choices**. These questions give me key information related to scalability, security, consistency and accuracy.

User Questions:

- How often are they using it?
    - Spikes in traffic
    - Availability
- Where will they be using it?
    - Internet connection quality
- How will they access it?
    - Devices or interfaces involved
- How many people will be using it?
    - Estimated throughput
    - Concurrent users

Data Questions:

- How much data transfer?
- How available does it need to be?
    - Redundancy, Scaling, Backup
- How consistent does it need to be?
    - Async vs Sync
- How urgent is it?
    - Real-time, Latency
- Does it differ based on jurisdiction of the user?
    - Geolocation
- How often is it accessed?
- Where should it be stored?
    - Local, CDN

### Scaling options

I want the system to perform well for every user. Performance is correlated to amount of resources available for a given number of users, as well as the availability of those resources. 

Key metrics for measuring performance will tell me whether or not I will run into scaling issues:

Latency: time delivering a single message.
Throughput: number of messages or amount of data transmitted within a time frame.
Availability: amount of uptime against downtime.

Vertical scaling refers to the resources of one machine. It’s a typical way to start out because it’s a simple setup, with consistent data and faster communication between services. But the bottleneck is the maximum resources you can put into it. If you’re keeping data on the same machine it can also become a single point of catastrophic failure (make sure you have and use your backup system regularly!).

![Diagram of a vertically scaled web app. [https://whimsical.com/system-design-3-vertically-scaled-full-stack-app-XXM4LfijmZwAhr5qtfh9hY](https://whimsical.com/system-design-3-vertically-scaled-full-stack-app-XXM4LfijmZwAhr5qtfh9hY)](My%20Approach%20to%20Systems%20Design%207905716aaf20402d911029f322d19b69/Untitled%203.png)

Diagram of a vertically scaled web app. [https://whimsical.com/system-design-3-vertically-scaled-full-stack-app-XXM4LfijmZwAhr5qtfh9hY](https://whimsical.com/system-design-3-vertically-scaled-full-stack-app-XXM4LfijmZwAhr5qtfh9hY)

Horizontal scaling refers to a method of distributing all or parts of the system across machines and potentially locations. It requires more complexity, with additional services such as a load balancer and additional networking, as well as data inconsistency. But it is more resilient to failure and is infinitely scalable. 

![Diagram of a horizontally scaled web app. [https://whimsical.com/system-design-4-horizontally-scaled-full-stack-app-TNVNzDWvxnGSM284rT4fZD](https://whimsical.com/system-design-4-horizontally-scaled-full-stack-app-TNVNzDWvxnGSM284rT4fZD)](My%20Approach%20to%20Systems%20Design%207905716aaf20402d911029f322d19b69/Untitled%204.png)

Diagram of a horizontally scaled web app. [https://whimsical.com/system-design-4-horizontally-scaled-full-stack-app-TNVNzDWvxnGSM284rT4fZD](https://whimsical.com/system-design-4-horizontally-scaled-full-stack-app-TNVNzDWvxnGSM284rT4fZD)

**Databases**

Partitioning is a method of scaling a database by splitting it into tables, views and indexes.
Sharding (horizontal partitioning) is splitting a database into multiple databases based on size. 
Sharding is preferably because it gives you increased throughput, storage and higher availability.

NoSQL databases can be used when you need a faster write - good for logging, queues and **caching**. They do not have relations, structure or schema.

### Technology options

![Diagram of example technology options for a system. [https://whimsical.com/system-design-5-technology-choices-GbuoEA8CBpjD2hwduxBsUt](https://whimsical.com/system-design-5-technology-choices-GbuoEA8CBpjD2hwduxBsUt)](My%20Approach%20to%20Systems%20Design%207905716aaf20402d911029f322d19b69/Untitled%205.png)

Diagram of example technology options for a system. [https://whimsical.com/system-design-5-technology-choices-GbuoEA8CBpjD2hwduxBsUt](https://whimsical.com/system-design-5-technology-choices-GbuoEA8CBpjD2hwduxBsUt)

### Integrations

Some entities should be managed by 3rd party services through integrations. I don’t aim to build everything custom, especially if it’s not related to our business IP.

Building integrations rather than our own services saves time and usually improves the UX, as well as scalability, security, consistency and accuracy.

![Diagram of example integrations. [https://whimsical.com/system-design-6-integrations-BkQmD7921V7TSf38oNEGHc](https://whimsical.com/system-design-6-integrations-BkQmD7921V7TSf38oNEGHc)](My%20Approach%20to%20Systems%20Design%207905716aaf20402d911029f322d19b69/Untitled%206.png)

Diagram of example integrations. [https://whimsical.com/system-design-6-integrations-BkQmD7921V7TSf38oNEGHc](https://whimsical.com/system-design-6-integrations-BkQmD7921V7TSf38oNEGHc)

# Problems and solutions

Discuss potential failure scenarios and solutions.

e.g. Traffic spikes, Abuse, Security

Potential usability improvements:

- oAuth providers
- Extra caching for increased performance
- CDN for delivering files to locations closer to users
- Analytics for improving conversion
- Add rate limiting based on IP/requests per second

# Next steps

- What services to develop in priority order.
- Key non-functional requirements for the team.
- Create prototypes to test design and validate assumptions
- Refine based on feedback.

View all diagrams used in this article: https://whimsical.com/system-design-by-mitchell-bryson-7h7sMtvNAAYHqPDV92jkv5

# Systems Design examples

[How to Build: Vehicle Rental](My%20Approach%20to%20Systems%20Design%207905716aaf20402d911029f322d19b69/How%20to%20Build%20Vehicle%20Rental%209d0c5086fc2547058f43fa626e1abd59.md)