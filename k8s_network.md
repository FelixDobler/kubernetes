| Criteria                   |                        |     |
| :------------------------- | ---------------------- | --- |
| Retains Source IP          |                        |     |
| Accepts Traffic for k8s IP |                        |     |
| Supports Multi-Node        |                        |     |
| vServer involvement        |                        |     |
| Ingress Failover           | X                      |     |
| Ingress Service Type       | LoadBalancer           |     |
| Normal Service Type        | ClusterIP/LoadBalancer |     |
| IPv4 and IPv6 Support      | with BGP FRR           |     |
| MLB Service Pools          |                        |     |
|                            |                        |     |
|                            |                        |     |
|                            |                        |     |
|                            |                        |     |
|                            |                        |     |
|                            |                        |     |

# Tech Stack

| MetalLB               |     |
| --------------------- | --- |
| BGP FRR on vServer    |     |
| MetalLB on the nodes  |     |
| Wireguard on the Node |     |
|                       |     |
|                       |     |
|                       |     |
|                       |     |
