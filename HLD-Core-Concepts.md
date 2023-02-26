# High Level System Design Core Concepts

---

## Microservice vs Monolith vs SOA

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

## Scaling

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

