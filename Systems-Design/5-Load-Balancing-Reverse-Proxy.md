# Load Balancer & Reverse Proxy
## Load Balancer
- Distributes incoming client requests to computing resources such as servers and databases
- Load balancer returns the response to the appropriate client
- Effective at
    - Preventing requests from going to unhealthy servers
    - Preventing overloading resources
    - Helping eliminate single points of failure
    - SSL termination (decrypt requests and encrypt responses so servers don't have to perform)
    - Session persistence (issue cookies and route a specific client to same instance if web apps do not keep track of sessions)
- Can be implemented with hardware or with software
- To protect against failures, common to set up multiple load balancers, either in active-passive or passive-passive mode
- Can route traffic based on metrics like random, least loaded, session, layer 4, layer 7
- Layer 4 distributes based on transport layer data (ip, source)
- Layer 7 looks at application layer (headers, message, cookies)
### Horizontal Scaling
- load balancers help with horizontal scaling -> higher availability and more cost efficient
- disadvantage is that scaling out introduces complexity and involves cloning servers
### LB Disadvantages
- LB can become a performance bottleneck if it doesn't not have enough resources or is configured wrong
- single LB is a single point of failure, configuring multiple increases complexity 

## Reverse Proxy
- Requests from client are forwarded to a server that can fulfill it before the reverse proxy returns the response to a client
- client does not know what server is being used
- increased security, scalability, and flexibility, caching, static content

### Disadvantage
- single RP removes single point of failure, multiple RP's introduce complexity  

## Load Balancer vs Reverse Proxy
- Load balancer is useful when you have multiple servers
- Reverse proxies can be useful even with just one server
