# High Level System Design Core Concepts

---

# `Microservice vs Monolith vs SOA`

- It is an architectural development style in which the application is made up of loosely-coupled services handling only small portions of the functionality and data by communicating with each other directly using lightweight protocols like HTTP.
- In monolithic applications there is usually a single codebase containing all the required functionalities of application.

![](https://firebasestorage.googleapis.com/v0/b/boom-b9a18.appspot.com/o/HLD%2F1.png?alt=media&token=a3d83192-44f0-47e9-829f-7929dc04b256)

### Principals of Microservices

- Single responsibility
- Built around business capabilities
- Design for failure

### Microservice vs SOA

- SOA evolved in order to deal with the problems in monolithic architecture and became popular in the early 2000s
- In SOA, the large application is split up into multiple smaller services that are deployed independently but don’t communicate with each other directly.
- ESB is a middleware/server in SOA which allows services exposed to it to communicate even by different protocols or message standards.
- There is no guideline to have an independent database in SOA for each service unlike in Microservices
- The fundamental idea of the evolution of microservices from SOA is to refine availability and loose coupling by introducing additional guidelines.


### Disadvantage of Microservice Architecture

- Microservices are costly in terms of network usage.
- Microservices are less secure relative to monolithic applications due to the inter-services communication over the network.
- Skilled developers are required to work with microservices architecture.

---

# `Scaling`

### Vertical vs Horizontal Scaling

- In vertical scaling, we add more power(CPU, RAM, and DISK) to the existing server or using a bigger machine.
- In vertical scaling servers, scale up or scale down depending on the traffic.
- In horizonal scaling, we scale by adding more servers to the system by renting a number of machines and dividing the load among them.
- In horizontal scaling, servers scale out or scale in depending on the traffic.

### Advantages and Disadvantages of Horizontal & Vertical Scaling.

- There is always a limit on vertical scaling as we can only add power upto an extent.
- There is no limit to horizontal scaling since we can have as many as servers we want and distribute the load among them.
- Also there is high availability in horizontal scaling as there might be less single point of faliures.
- However we need skilled engineers for architecting horizontally scaling systems as complexity is higher than Vertical scaling.

### Scaling EC2 Instances

- We can scale EC2 instances both vertically and horizontally
- Suppose we are using t3.micro and there is a traffic boom,then the Application load balancer will spin up a m5.2XLarge and direct all the traffic to it .This is called vertical scaling.
- Suppose we are using a t3.micro in in EC2 autoscaling group, and there is a traffic boom then AWS will spin up new instances of specified criteria and distribute load among them.

### Scaling AWS Lambda

- AWS Lambda can only scale horizontally.
- When your lambda function is invoked, AWS Lambda allocates an instance of it to process the event having resources specified by developer.
- When the function code finishes running, it can handle another request.
- If the function is invoked again while a request is still being processed, another instance is allocated, which increasing the function's concurrency.
- When the labda instances are free, they scale in.
- However maximum concurrency limit of a particular Lambda function has to be specified by the developer.

### Scaling Container (Kubernetes)

- In kubernetes, container is running inside a pod which itself is running inside EC2 instance.
- The pod keeps a track of resource utilization of itsef.
- When the traffic comes in ,it will scale inside EC2 using Horizontal Pod Autoscaler (HPA).
- As the smallest unit of autoscaling in kubernetes is pod, full pod will be scaled horizontally.
- When EC2 is filled up, we have to scale the EC2 instance too which is called cluster autoscaling.



# `Load Balancer`

- When using scalable microservices, a client needs to be able to route its requests to one of the multiple backend server instances and make sure that no server gets overloaded.
- Also when a server instance faces downtime , LB distributes its requests among other relevant server instances.

### Types of Load Balancers

- `Application Load balancers(Layer 7)` distribute requests based upon data found in application layer protocols such as HTTP(eg. Cookies,Headers etc).
- `Network load balancers(Layer 4)` distribute requests based upon data found in network and transport layer protocols (IP, TCP, FTP, UDP).

### Client-Side Balancing VS Server-Side Load Balancing

- `Client-Side Load balancing`: Client fetches list of registered backend servers instances using service registry/naming server (eg. Netflix Eureka) and then route the request to one of these backend instances using client-side load balancing libraries like Netflix Ribbon.

- `Server-Side Load Balancing`: All backend server instances are registered with a central load balancer(Eg. Netflix Zuul) and are registered with service registry.A client requests this load balancer which then routes the request to one of the server instances.


### Classical Load balancing Algorithms

- **Round Robin Algorithms**: Distributes requests in rotation based on FCFS. 
- **Least Connection**: Distributes to servers with least active connections.
- **Resource Based**: Distributes Requests based on resources available with an instance.
- **Hash Based**: Creates a hash of source/destination IP Address/URL and allocate a server based on that hash.

Note: Most of these algorithms can be weighted too(Priority Based)

# `API-Gateway`

- An API-Gateway is a single point of entry in our system in microservice architecture.
- API Gateway can also perform various tasks like rate-limiting , authentication & authorization, logging requests/responses, etc
- A popular exammple of this is Netflix Zuul which acts as both an API-Gateway and Server-Side Load Balancer.


# `Message Queue Driven Architecture vs Event Driven Architecture`

## Message Queue Driven Architecture

- In a message-queue model, the publisher pushes messages to a queue where 1 or more subscriber can listen to a particular queue.
- In this each message is processed by only one consumer which then deletes it from the queue.
- Due to this it is also called pull model.
- It can be implemented by using RabbitMQ and also availible on AWS as Amazon SNS.

## Event Driven Architecture using Pub/Sub

- An event-driven system consist of typically consists of event emitters (or agents), event consumers (or sinks), and event channels.
- In Publiser/Subscriber model,which is an event-driven architecture,a publisher publishes a message to a topic and all the subscribers of that topic receives 1 copy of that message.
- It can be implemented by using RabbitMQ and also availible on AWS as Amazon SQS.


![](https://firebasestorage.googleapis.com/v0/b/boom-b9a18.appspot.com/o/xyz.png?alt=media&token=828bc2e9-96c7-4ce9-9638-863e02bf0246)

# `Streaming`

- In streaming, a single message is not enough to give the complete picture as we need to analyse whole stream of messages to understant the acual message.
-  We maintain a distributed log file and consumers move backward and forward within that file to re-process messages they've already received on command.
- In simple words, streaming is the passing of event log as they occur.
- For eg- When you visit amazon,all your clicks are put into stream to generate user data as a 1-2 clicks are usually not enough to get user preferences.
- It can be implemented using kafka and is also available on AWS as Amazon Kinesis.


Note: In messaging, we generally can't reprocess a message as it is deleted but we can do that using backup(Eg-batch layer) puting it back into queue from the backup. 

# `Web Sockets`

- WebSocket is a communications protocol for a persistent, bi-directional, full duplex TCP connection from a user's web browser to a server.
- A WebSocket connection is initiated by sending a WebSocket handshake request from a browser's HTTP connection to a server to upgrade the connection.

![](https://assets.website-files.com/5ff66329429d880392f6cba2/617a911c0c264f7bfbe7be5f_websocket%20work.png)
