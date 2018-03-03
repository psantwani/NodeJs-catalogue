Microservices

Small independently releasable autonomous services that work together modelled around a business domain.

Service-oriented architecture.

Micro-services bring more options. Options can be good/bad. In monolithic, we'd need to take one/two decisions per year - tech stack, persistence, design etc.

Thinking of each micro service from scratch can lead to overall inconsistency.

Heroku's 12 guiding factors when building a software-
1. Codebase - One codebase tracked in revision control, many deploys.
2. Dependencies - Explicitly declare and isolate dependencies. 
3. Config - Store config in environment
4. Backing services - Treat backing services as attached resources
5. Build, release, run - Strictly separate build and run stages
6. Process - Execute the app as one or more stateless processes
7. Concurrency - Scale out via the process model
8. Port binding - Export services via port binding
9. Disposability - Maximise robustness with fast startup and graceful shutdown
10. Dev/prod parity - Keep dev, staging, and production as similar as possible
11. Logs - Treat logs as event streams
12. Admin processes - Run admin/management tasks as one-off processes

Another set of guiding principles.
1. Strategic Goals
- Enable scalable business
- Support entry into new markets
- Support innovation in existing markets

2. Architectural principals
- Reduce intertia (Make choices that favour rapid feedback and change with reduced dependencies across teams)
- Eliminate accidental complexity
- Consistent interfaces and data flows
- No silver bullets

3. Design and delivery practices
- Standard REST/HTTP
- Encapsulate legacy
- Eliminate integration databases
- Consolidate and cleanse data
- Published integration model
- Small independent services
- Continuous deployment
- Minimal customisations of COTS/SAAS


Principal of micro services
- Modelled around business domain (more stable APIs) - Book : Implementing domain driven design by Vaughn vernon. 
- Embracing culture of automation (because we have more number of deployable units now) - Relentless in infrastructure automation, automated testing, continuous delivery, to use micro services at scale.
- Hide implementation details (to allow one service to grow independently of others)
- Decentralise All the things (decisions and architecture)
- Deploy independently
- Consumer first
- Isolate failure
- Highly Observable (how they behave together)

