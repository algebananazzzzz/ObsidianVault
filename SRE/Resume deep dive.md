## Certifications
- AWS Certified Solutions Architect Professional
- Certified Kubernetes Administrator
## GovTech DevOps Intern
- In GovTech, we have a very interesting team dynamic.
- In my team, we have 20-30 developers while having only 4 devops engineers including myself.
- However, I really enjoyed this challenge, I'm able to lead a lot of projects, work is very fulfilling.

**Projects that I led**

| Description                                     | Situation                                                               | Tools                                                                                                                                            |
| ----------------------------------------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Uptime multi-region synthetic monitoring system | Check uptime of 5000+ Government services                               | Designed a Master-Worker architecture, push jobs to a SQS queue, workers running concurrent Go routines through multiple channels to ping sites. |
| Architecting of AWS solutions for key products  | Core products need to develop multiple features in parallel             | Used Terraform heavily to define IaC, Gitlab CI/CD to automate spin up of environments and run test suite                                        |
| Major network security improvements             | Facing malicious attacks from 100+ known IP sources                     | Heavy refactoring of VPC architecture with AWS Network Firewall (L4 firewall) and integrated AWS WAF (L7) with CDNs.                             |
| OpenFaaS on EKS                                 | Developers use Lambda for quick prototyping, but cost overhead is > 70% | Use Prometheus, Linkerd, OpenFaas, Netes, K8s stack on EKS, setup pipeline with ArgoCD (crossplane)                                              |
**Main Learning:** 
- AWS Cloud (used heavily throughout the team): System Design, high level architecture
- Terraform IaC
- Gitlab CI/CD
- Golang (especially in concurrency and channels)
- Service Discovery with AWS Cloud Map (service registry)
- Networking (gRPC across microservices)
## Sheares Web Chairperson
- Lead a team developing technical products used actively by 500+ student residents.
- Work closely with JCRC and leadership to support their day-to-day operations.

| Description                              | Situation                                                         | Tools                                                                                                             |
| ---------------------------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Refactor DB schema, cloud infrastructure | Many isolated data stores for different products, high cloud cost | Used liquibase to define DDL, custom golang script to onboard resident data to a centralized PostgreSQL database. |
| Odyssey Framework                        | Standardize CI/CD pipeline creation and automation                | Golang CLI tool to bootstrap common project use cases from IaC, Github Actions templates                          |
## Personal Projects

| Description    | Situation                                                                                  | Tools                                                                                              |
| -------------- | ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| Home Labbing   | Love retro technology like usenet, understanding the history is important for appreciation | Setup K3s on a VM with Usenet services like radarr, sonarr, lidarr, used Tailscale to setup SD wan |
| Small projects | Use Odyssey Framework to create small projects like LangChain + LangGraph BeerBuddy        | AWS                                                                                                |
