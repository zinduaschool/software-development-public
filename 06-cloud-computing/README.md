# 06. Cloud Computing
Official Curriculum for the Cloud Computing Module at Zindua School.

- **Course Description:** A four-workshop course that takes engineers from choosing infrastructure to designing systems that scale. Learners build progressively — evaluating hosting, database and storage options and what drives their cost, then containerising and deploying a live application with a domain and HTTPS, then automating deployments and matching cloud services to requirements — before designing systems that account for scalability, reliability, observability, cost and AI components. Each workshop pairs self-paced LMS pre-work with a live, lab-focused session.

- **Course Objective:** Equip software engineers to choose appropriate cloud infrastructure, deploy and secure a containerised application, select suitable cloud services, and design systems that account for scalability, reliability, observability and cost, and to justify each choice with requirements and cost data rather than preference.

- **Skills to be learned:** AWS (EC2, Lambda, RDS, S3, DynamoDB, IAM, API Gateway, SNS, SES, CloudWatch), Cloudflare (DNS, proxy, CDN, R2, Workers), Docker, Docker Compose, PostgreSQL, NGINX, Let's Encrypt, Git-based deployment, serverless functions, Redis caching, capacity estimation, observability & SLOs, system design

- **Prerequisites (not taught in the course):** command line navigation and editing; Git and GitHub (clone, branch, commit, push); running a small web API locally with Django, Flask, Express or similar and reading its logs; HTTP fundamentals (requests, responses, status codes, JSON, ports); basic SQL (`CREATE TABLE`, `SELECT`, `INSERT`, `UPDATE` against PostgreSQL or similar)

- **Before Workshop 1:** an AWS account with MFA enabled and a billing alert configured; Cloudflare and GitHub accounts; access to a VPS and a domain name (provider and expected cost confirmed before each cohort); a laptop that can run Docker Desktop or Docker Engine on Linux, with Git, a terminal, Docker and Docker Compose installed; a stable internet connection. Any paid services are communicated to learners before the relevant workshop.

## Curriculum Breakdown

### Workshop 1: Cloud Foundations
> **Objective:** Evaluate infrastructure choices before deploying anything: learners will be able to explain the components of a deployed application and how a cloud environment differs from localhost, choose a hosting approach for a given workload, select a database hosting approach and data model based on requirements and operational responsibility, design a basic file-storage workflow, and name the cost drivers behind each decision.
- Cloud computing fundamentals: what cloud computing is and the problems it solves; the four building blocks — compute, storage, databases, networking; localhost vs. cloud environments; introduction to VPS hosting, managed platforms and serverless
- Choosing where to run an application: what you manage vs. what the provider manages; when to use a VPS, a managed platform or a serverless service; trade-offs across cost, control, complexity and scalability; cost drivers — instance-hours, reserved capacity, invocations, idle resources, data transfer (Further study: GCP Compute Engine, Azure Virtual Machines, Cloudflare Workers, Vercel, Contabo, Oracle Cloud)
- Managed databases and SQL vs. NoSQL: self-hosted vs. managed — backups, availability, patching, maintenance; PostgreSQL on Amazon RDS and Supabase; connecting an application to a managed database; relational vs. NoSQL (PostgreSQL, MongoDB, DynamoDB); cost drivers — instance size, storage, read/write capacity, backups, multi-AZ (Further study: GCP Cloud SQL, Firestore, Azure SQL, Cosmos DB, PlanetScale)
- Cloud object storage: local file storage vs. object storage; buckets, objects, keys and access control; Amazon S3 and Cloudflare R2; uploading and retrieving files from an application; cost drivers — storage volume, request counts, data transfer (Further study: GCP Cloud Storage, Azure Blob Storage)
- **Take-home:** self-paced study of AWS SES (email delivery), Amazon API Gateway (API entry points and management), Amazon SNS (notifications and message distribution), AWS IAM (identities, roles and permissions) and Amazon CloudWatch (logs, metrics and monitoring), followed by a short reflection or quiz on each service and where it fits
- **Projects:** 

### Workshop 2: Docker and Application Deployment
> **Objective:** Take an application from localhost to the public internet: learners will be able to provision and secure a VPS, containerise an application and run it alongside PostgreSQL with Docker Compose, configure a domain and DNS to resolve to the server, set up NGINX with HTTPS, explain how reverse proxies, CDNs and load balancers work together, and produce a live public URL with everything needed to reproduce the deployment.
- Cloud servers and VPS: what a VPS is and how it differs from a managed platform; provisioning with AWS EC2 or an alternative; public IP addresses and remote access; SSH and key-based authentication; basic Linux configuration — users, permissions, firewall rules
- Docker and containerisation: why containers are used; images vs. containers; writing a Dockerfile; building, pulling and running images; environment variables and port mapping; connecting an application to a PostgreSQL container
- Docker Compose: why applications need multiple services; defining backend and database services in `compose.yaml` / `docker-compose.yml`; service-to-service communication; volumes and data persistence across container restarts
- Networking and domains: IP addresses and ports; HTTP vs. HTTPS; domain registration and configuration; DNS records and resolution; pointing a domain or subdomain at a server; basic Cloudflare DNS setup
- Reverse proxies, CDNs and load balancing: NGINX as a reverse proxy; SSL/TLS certificates and HTTPS with Let's Encrypt; what a CDN does and how it affects delivery; the purpose of load balancing; Cloudflare as the main teaching example for DNS, proxying and CDN (Further study: Route 53, CloudFront, Elastic Load Balancing)
- **Projects:** Deploy an application end to end — containerise a working application, prepare the VPS, run the application and a PostgreSQL container with Docker Compose, configure ports and firewall rules, connect a domain and DNS, set up NGINX and HTTPS, and verify public access. Deliverable: a public URL plus the repository and configuration needed to reproduce the deployment

### Workshop 3: Cloud Architecture
> **Objective:** Deploy changes repeatably and choose services on requirements rather than preference: learners will be able to implement a Git-based deployment workflow with verification and rollback, build or examine a serverless function and justify when serverless beats a VPS, and design a cloud architecture for an unfamiliar application and defend it against alternative approaches.
- Git-based deployment workflow: limitations of manual deployment; moving code from a repository into a running container; rebuilding and restarting an application safely; verifying a deployment and handling failed releases or rollbacks
- Serverless computing: what serverless means; serverless functions vs. continuously running services; event-driven execution; AWS Lambda, Cloudflare Workers and Vercel Functions; cost models, execution limits and cold starts (Practical: implement or examine a serverless function for a real use case)
- Cloud architecture: breaking a full-stack application into components and deciding where each runs — frontend hosting, backend and API hosting, database, file storage, caching, DNS and traffic routing, optional serverless functions; weighing cost, complexity and alternative service choices
- **Projects:** Architecture diagram for an unfamiliar full-stack application, with a defence of each service choice against the alternatives

### Workshop 4: System Design
> **Objective:** Design systems that scale, stay observable and keep costs in check: learners will be able to translate a product brief into requirements and a workload estimate, choose a data scaling and caching strategy and explain its consistency trade-offs, decide where load balancing, queues, rate limiting or service boundaries help and where they are unnecessary, justify a single- or multi-region design and the metrics that measure its health, place AI components such as RAG in an architecture with their cost and latency implications, and defend an end-to-end system design document.
- System design fundamentals: what system design is and why it matters; functional vs. non-functional requirements; back-of-the-envelope capacity estimation; vertical vs. horizontal scaling; the CAP theorem
- Scaling data: replication, partitioning and sharding; read replicas and write scaling; Redis caching strategies — cache-aside, write-through, write-back; strong vs. eventual consistency
- Scaling compute and traffic: load balancing; queues and asynchronous processing; rate limiting; monolith vs. microservices, and when none of these is needed
- Designing beyond one server: when multi-region architecture is justified; latency, reliability and cost per request; observability through metrics and tracing; service-level objectives (SLOs); connecting scaling decisions, operating costs and reliability
- AI as a standard system component: embeddings; vector databases; retrieval-augmented generation (RAG); data flow and dependencies in AI-enabled applications; latency, infrastructure cost and operational trade-offs
- System design case study: an AI-enabled system from requirements to architecture, such as a RAG support chatbot or a recommendation feed — requirements gathering, workload estimation, component selection, detailed design decisions; cost, reliability, multi-region and observability trade-offs
- **Take-home:** study consistent hashing and its role in distributing data; complete a second case study (URL shortener or rate limiter); short quiz on the workshop's main concepts
- **Projects:** System design document for a realistic growth scenario, such as supporting 10× the workload of an existing application: requirements and workload assumptions, architecture diagram, data model, scaling strategy, cost considerations, observability plan, and written justification of major trade-offs


## Course Contributors
### Course Owner(s)
- Kwanusu Joseph - Workshop 1: Cloud Foundations
- Chalie - Workshop 2: Docker and Application Deployment
- Sonia - Workshop 3: Cloud Architecture
- Cecilia Wambui — Workshop 4: System Design
