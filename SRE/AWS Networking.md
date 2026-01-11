## VPC Peering vs Transit Gateway

| Dimension              | **VPC Peering**                                   | **Transit Gateway (TGW)**                                             |
| ---------------------- | ------------------------------------------------- | --------------------------------------------------------------------- |
| Model                  | Point-to-point L3 link                            | Central L3 router (hub-and-spoke)                                     |
| Transitive routing     | Not supported                                     | Supported                                                             |
| CIDR overlap           | Not allowed                                       | Not allowed                                                           |
| Cross account / region | Allowed                                           | Allowed<br>Requires TGW Peering (2 TGWs)                              |
| Route management       | Manually hardcode CIDRs into route tables per VPC | Attachments advertise their associated VPC CIDR into TGW route tables |
## Private Link vs VPCE
| Dimension | **PrivateLink / Interface VPCE**                  | **Gateway Endpoint**                                     |
| --------- | ------------------------------------------------- | -------------------------------------------------------- |
| Mechanism | App → VPCE ENI → PrivateLink (L4) → NLB → Service | Add route to VPC route table (L3) prefix list → endpoint |
