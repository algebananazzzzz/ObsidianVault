## Core Components
1. Service Registry
	- Stores service metadata (service, IP + port)
	- Health check, tags e.g. Kubernetes labels and selectors
2. Discovery
## Discovery Mechanism

| Client-side discovery                              | Server-side discovery                            |
| -------------------------------------------------- | ------------------------------------------------ |
| Client queries registry directly, selects endpoint | Client connects to proxy; proxy queries registry |
| Consul, AWS Cloud Map                              | Envoy, Nacos, Kubeproxy                          |
| Overhead in client logic                           | Single point of failure                          |
