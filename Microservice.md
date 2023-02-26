# Microservice

- It is an architectural development style in which the application is made up of loosely-coupled services handling only small portions of the functionality and data by communicating with each other directly using lightweight protocols like HTTP.

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


### Disaadvantage of Microservice Architecture

- Microservices are costly in terms of network usage.
- Microservices are less secure relative to monolithic applications due to the inter-services communication over the network.
- Skilled developers are required to work with microservices architecture.
