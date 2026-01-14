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

| Aspect            | REST (HTTP/1.1 or HTTP/2)     | gRPC (HTTP/2)                    | WebSocket             |
| ----------------- | ----------------------------- | -------------------------------- | --------------------- |
| Data format       | JSON (usually)                | Protobuf (binary)                | Any (text/binary)     |
| Typing            | Weak (JSON, schema optional)  | Strong (proto schema)            | None by default       |
| Performance       | Slower (text, bigger payload) | Very fast (binary, multiplexed)  | Fast, but app-defined |
| Streaming         | Limited (workarounds)         | Built-in streaming               | Built-in full duplex  |
| Tooling           | Huge ecosystem                | Strong codegen                   | Manual handling       |
| Browser support   | Native                        | Needs proxy (gRPC-Web)           | Native                |
| Human readable    | Yes                           | No                               | Depends on format     |
| Versioning        | Manual (URLs, headers)        | Built into proto                 | Manual                |
| Error handling    | HTTP codes + body             | gRPC status codes                | App-defined           |
| Typical use       | Public APIs, CRUD             | Internal services, microservices | Realtime apps         |
| Firewall friendly | Very                          | Mostly                           | Sometimes blocked     |
| Learning curve    | Low                           | Medium                           | Medium                |
