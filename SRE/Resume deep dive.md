## GovTech DevOps Intern
- In GovTech, we have a very interesting team dynamic.
- In my team, we have 20-30 developers while having only 4 devops engineers including myself.
- However, I really enjoyed this challenge, I'm able to lead a lot of projects, work is very fulfilling.

**Projects that I led**

| Description                                     | Situation                                                   | Tools                                                                                                                                            |
| ----------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Uptime multi-region synthetic monitoring system | Check uptime of 5000+ Government services                   | Designed a Master-Worker architecture, push jobs to a SQS queue, workers running concurrent Go routines through multiple channels to ping sites. |
| Dynamic CI environment for integration tests    | Core products need to develop multiple features in parallel | Used Terraform heavily to define IaC, Gitlab CI/CD to automate spin up of environments and run test suite                                        |
| Major network security improvements             | Facing malicious attacks from 100+ known IP sources         | Heavy refactoring of VPC architecture with AWS Network Firewall (L4 firewall) and integrated AWS WAF (L7) with CDNs.                             |
**Main Learning:** 
- AWS Cloud (used heavily throughout the team): System Design, high level architecture
- Terraform IaC
- Gitlab CI/CD
- Golang (especially in concurrency and channels)
- Service Discovery with AWS Cloud Map (service registry)
- Networking (gRPC across microservices)
## Sheares Web Chairperson
- Lead a team developing critical technical products and platforms used actively by 500+ student residents.
- Work closely with JCRC and leadership to support their day-to-day operations.

| Description                              | Situation                                                                                                          | Tools                                                                                                             |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| Refactor DB schema, cloud infrastructure | Many isolated data stores for different products, high cloud cost                                                  | Used liquibase to define DDL, custom golang script to onboard resident data to a centralized PostgreSQL database. |
|                                          | Talk about holiday subsidy system. Business requirements, how to distribute subsidies while keeping in budget etc. | Something I learnt: business process reengineering trumps blindly complicating tech.                              |
What I learnt: the whole process of helping leadership understand their business needs, interesting experience.
## Personal Projects
- Focus more on introducing best practices, processes etc.



