# How to Build: Vehicle Rental

By Mitchell Bryson

This is an example of [my approach to systems design](../My%20Approach%20to%20Systems%20Design%207905716aaf20402d911029f322d19b69.md) applied to a real-world example.

# Requirements and scope

A person should be able to rent different available vehicles such as cars (both electric and combustion), scooters and motorbikes.

Real world examples of similar businesses are Turo, ShareNow or Lime.

Requirements for this specific example:

- Scooter and Motorbike customers may rent a helmet for an extra fee.
- Motorbike rental requires the purchase of additional insurance that is only valid for 24 hours.
- Customers will be charged based on their usage time but the service should also support a subscription model that would allow them to use a set amount of minutes per month for each of the different vehicles as well.

---

Further requirements based on assumptions:

- The customer is someone who wants to use a vehicle on demand for a short time that is within walking distance of their current location
- They are potentially not familiar with the vehicle they are renting and may cause issues
- They may want to rent multiple types of vehicle depending on their situation
- All Vehicles have a GPS transponder and remote locking
- Accessories are available with some of the vehicles (locked to it)
- Maintenance needs to be carried out on vehicle and accessories at regular intervals
- When a rental ends the vehicle can be left where it is
- Rental usage time begins when the reservation is confirmed as assigned to a customer
- Rental usage ends when the vehicle (and accessory, if present) is locked by the customer
- License verification and payments / subscriptions can be done by a 3rd party

# Entities, services and their relationships

| Entities | Services |
| --- | --- |
| Customer
• License | Vehicle license verification |
| Vehicle
• Car
    ◦ Electric Car
    ◦ Combustion Car
• Scooter
• Motorbike | Billing
• Pay as you go (price per unit of usage time)
• Subscription (price per date interval for set amount of usage time)
• Charge when a rental has ended
• Or… charge when a subscription interval happens |
| Accessory
• Helmet
    ◦ Motorbike Helmet
    ◦ Scooter Helment | Vehicle and Accessory Management
• Location
• Accessory presence
• Unlock / Lock remotely
• Schedule maintenance
• Issues |
| Insurance
• Motorbike Insurance (24 hours only) | Vehicle Availability Search
• Vehicles without reservation
    ◦ Within a distance from latitude/longitude coordinates
    ◦ Without issues on itself or accessory
    ◦ With distance from coordinates
    ◦ By vehicle type |
| Payment method
Charge
Subscription | Reservation management
• Assign a vehicle and/or accessory to a customer
• Track usage time of a vehicle and accessory by a customer
• Start insurance for Motorbike rental
    ◦ Check reservation assignment every 24 hours and renew the insurance if required
• Issue reporting
• Unlock vehicle and / or accessory
• End rental - lock vehicle and / or accessory |

![Diagram showing entities, services and their relationship. [https://whimsical.com/system-design-1-entities-and-services-8kxvxeshhMb2icJqbBq8LU](https://whimsical.com/system-design-1-entities-and-services-8kxvxeshhMb2icJqbBq8LU)](How%20to%20Build%20Vehicle%20Rental%209d0c5086fc2547058f43fa626e1abd59/Untitled.png)

Diagram showing entities, services and their relationship. [https://whimsical.com/system-design-1-entities-and-services-8kxvxeshhMb2icJqbBq8LU](https://whimsical.com/system-design-1-entities-and-services-8kxvxeshhMb2icJqbBq8LU)

# Data, Services and their API

![Diagram of services API. [https://whimsical.com/system-design-2-services-api-copy-9JUfTUHs2NQRdTwR23LwLc](https://whimsical.com/system-design-2-services-api-copy-9JUfTUHs2NQRdTwR23LwLc)](How%20to%20Build%20Vehicle%20Rental%209d0c5086fc2547058f43fa626e1abd59/Untitled%201.png)

Diagram of services API. [https://whimsical.com/system-design-2-services-api-copy-9JUfTUHs2NQRdTwR23LwLc](https://whimsical.com/system-design-2-services-api-copy-9JUfTUHs2NQRdTwR23LwLc)

![Diagram of entity data and relationships. [https://whimsical.com/system-design-3-entity-data-Fq1FBGVNarVKC9G3d6ivqy](https://whimsical.com/system-design-3-entity-data-Fq1FBGVNarVKC9G3d6ivqy)](How%20to%20Build%20Vehicle%20Rental%209d0c5086fc2547058f43fa626e1abd59/Untitled%202.png)

Diagram of entity data and relationships. [https://whimsical.com/system-design-3-entity-data-Fq1FBGVNarVKC9G3d6ivqy](https://whimsical.com/system-design-3-entity-data-Fq1FBGVNarVKC9G3d6ivqy)

# Infrastructure and Technology choices

![Diagram of technology choices and infrastructure. [https://whimsical.com/system-design-4-technology-choices-L5AiDUnR8hxmHqVktKLd1U](https://whimsical.com/system-design-4-technology-choices-L5AiDUnR8hxmHqVktKLd1U)](How%20to%20Build%20Vehicle%20Rental%209d0c5086fc2547058f43fa626e1abd59/Untitled%203.png)

Diagram of technology choices and infrastructure. [https://whimsical.com/system-design-4-technology-choices-L5AiDUnR8hxmHqVktKLd1U](https://whimsical.com/system-design-4-technology-choices-L5AiDUnR8hxmHqVktKLd1U)

## Reasoning and trade-offs

Native mobile app:

Customers will be out and about when they need a vehicle so it makes sense here to go mobile first. I don’t expect there to be any performance issues with a hybrid mobile app but this would need to be tested, especially around the mapping and geolocation features. A hybrid app will allow us to launch sooner and on more platforms. Using something like Flutter would give us the opportunity to build to most native platforms.

API Gateway:

An API gateway would act as our single point of entry to our API(s). It can act as a load balancer, as well as provide authentication and authorisation. Most modern gateways support multiple protocols such as GraphQL, REST and gRCP. Amazon offer an API gateway but another popular solution is Kong.

EC2:

A popular platform for hosting in the cloud. Another option could be DigitalOcean as they provide an app platform that simplifies a lot of devOps initial then can be expanded horizontally to host multiple servers in different locations.

App - Node.js:

Node has good real-time capabilities through streams. Development would be less complicated due to the frontend sharing the same language and ecosystem as the backend. I’d suggest using Typescript for extra type safety, this tends to reduce the complexity in boilerplate unit tests.

PostgreSQL:

Standard relational database. Also supports a vast array of plugins including PostGIS for dealing with geospatial data and calculations within a query. I like the security aspects of Postgres, particularly the ability to do row-level authorisation.

Memcache:

Standard schema-less database used for caching. Other options could be Redis, to simplify the infrastructure a bit. Memcache does one job really well. Fast writes are key.

Redis Queue:

My go to queuing database. Similar to Memcache but has more features and is better designed for scaling large queuing systems. It includes support for clustering and has high availability tooling.

AWS Fargate:

For managing and running simple background jobs that have one function, such as async calls to 3rd party services. We can use the same container orchestration we might use for the application. We would need to setup auto scaling for removing jobs. Another option is just to run more replica application services in a new process on the same machine or another machine.

VLS App:

I would recommend running this in a separate process or machine, as the functional requirements differ slightly from the consumer API. This service needs to be able to handle lots of concurrent small writes (GPS data) and does not require much of a schema. The main application can pull this data as and when needed from it’s data store.

MongoDB:

I’d use a schema-less database for this service as it allows for faster writes and high availability but doesn’t need to keep consistency in it’s data. Another option to simplify the infrastructure would be to use another instance of Redis.

# Integrations

![Diagram of example integrations. [https://whimsical.com/system-design-5-integrations-2wNzpFFztPBoBj5dimNzjp](https://whimsical.com/system-design-5-integrations-2wNzpFFztPBoBj5dimNzjp)](How%20to%20Build%20Vehicle%20Rental%209d0c5086fc2547058f43fa626e1abd59/Untitled%204.png)

Diagram of example integrations. [https://whimsical.com/system-design-5-integrations-2wNzpFFztPBoBj5dimNzjp](https://whimsical.com/system-design-5-integrations-2wNzpFFztPBoBj5dimNzjp)

I’d recommend outsourcing the Mapping and Geolocation services to 3rd parties. OSM is a good choice, and IPInfo allows us to geolocate users based on IP address. I’m also assuming we need to find an underwriter for the insurance - one that has a good API and support for developers.

There are a few 3rd party license verifiers that could be used, not none that give me 100% confidence - this requires more research.

# Potential problems and solutions

- 2 reservations at the same time - this can happen when 2 customer try to reserve at the same time.
    - Confirm reservation is active after assignment. If another reservation exists before this one and is active, cancel the reservation. Otherwise, confirm it.
- Issues with vehicles or accessories
    - Customer rating system
        - Increase based on successful rentals with no issues at next rental or maintenance
        - Decrease based on issues present after rental reported at next rental or maintenance
        - Ban customers if it drops below a certain threshold
- High demand in an area results in no vehicles available
    - Dynamic pricing
    - Delivery or pick up of vehicles and accessories to customers

View all diagrams used in this article: https://whimsical.com/system-design-vehicle-rental-by-mitchell-bryson-9DN1hK9v99GVHKJEkVZZML

# My Approach to Systems Design

[My Approach to Systems Design](../My%20Approach%20to%20Systems%20Design%207905716aaf20402d911029f322d19b69.md)
