# Communication

![](https://camo.githubusercontent.com/1d761d5688d28ce1fb12a0f1c8191bca96eece4c/687474703a2f2f692e696d6775722e636f6d2f354b656f6351732e6a7067)

## HTTP (Hypertext transfer protocol)
- Method for encoding and transporting data between client and server 
- Req/Res protocol
- Allows requests and responses to flow through many intermediate routers and servers that perform LB, Caching, Encryption
- GET reads a resource
- POST creates a resource or triggers a process that handles data
- PUT creates or replaces a resource
- DELETE deletes a resource
- PATCH partially updates resource 
- HTTP is an application layer protocol relying on lower-level protocols such as TCP and UDP

### TCP (transmission control protocol)
- Connection-oriented protocol over IP network
- Data must reach destination in original order and without corruption otherwise data is resent
- TCP is useful for applications that require high reliability but are less time critical
    - web servers, SMTP, SSH, web servers
- Use TCP over UDP when 
    - You need all data to arrive intact
    - You want to automatically make a best estimate use of network throughput

### UDP (User datagram protocol)
- Connectionless
- Data may reach destination out of order or not at all
- TCP has more guarantees making it less efficient
- UDP is less reliable but works well in real-time such as streaming and realtime games
- Use UDP over TCP if you need low latency, and late data is worse than loss of data

## RPC (Remote procedure call)
- Client causes a procedure to execute on a different address space 
- Looks like a method call
- Returns are hand-coded
- Verb centric (getAnOrder)
    - REST is noun centric
- Remote calls are usually slower and less reliable 
- Great for actions (procedures or commands)
- RPC is focused on exposing behaviors
    - Often used for performance reasons with internal communications since you can create custom native calls to fit use cases

### Disadvantage 
- RPC clients are coupled to the service implementation
- New API must be defined for every new operation

## REST (Representational state transfer)
- Architecture style enforcing a client/server model where clients acts on a set of resources managed by the server
- All communication must be stateless and cacheable
- Four qualities of RESTful 
    - Use the same URI
    - Use verbs (GET, POST), headers, body
    - Status codes
    - Should be fully accessible in a browser
- REST is focused on exposing data
    - Often used for public APIs

### Disadvantages
- Uses 5 verbs which sometimes does not fit your use case 
- Fetching complicated resources with nested hierarchies (blog, comments) are not desirable
- REST implementations are a combination of URI path, query params, and body (might not be a good fit if resources are not naturally organized such as returning all updated records from past hour matching certain events)
