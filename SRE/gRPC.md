Imagine having 2 microservices running in different machines. How do they communicate?
1. Use queue system like RMQ
2. Send HTTP REST Requests
3. Use a RPC
## What RPC does
1. Called by a function call
2. Serialize request
3. Send over network

## RPC vs REST

1. Higher throughput by using binary instead of JSON, reduce overhead and parsing load
2. Streaming 

| Business Requirement | RPC                            | REST                |
| -------------------- | ------------------------------ | ------------------- |
| High throughput      | Binary protobufs (10x smaller) | JSON large overhead |
| Low latency          |                                |                     |
| Type safe (contract) | Define proto files             |                     |
| Streaming            |                                |                     |
