# System Design

A process of defining the architecture, components, modules, interfaces, and data for a system to satisfy specified requirements.

Things to consider in designing the system:
1. Functional and non functional requirements
2. Capacity estimation
3. Database to use
4. Vertical scaling, Horizontal scaling, [Shard](https://en.wikipedia.org/wiki/Shard_(database_architecture) "Shard (database architecture)")
5. [Load Balancing](https://en.wikipedia.org/wiki/Load_balancing_(computing) "Load balancing (computing)")
6. Primary-secondary [Replication](https://en.wikipedia.org/wiki/Replication_(computing) "Replication (computing)")
7. Cache and CDN
8. Stateless and Stateful servers
9. Datacenter [georouting](https://en.wikipedia.org/wiki/Geographic_routing "Geographic routing")
10. Message Queue, Publish-Subscribe Architecture
11. Performance Metrics Monitoring and Logging
12. Build, test, configure deploy automation
13. Finding single point of failure
14. [API](https://en.wikipedia.org/wiki/API "API") Rate Limiting
15. Service Level Agreement

## Sub-Processes

Short version of project stages architect-view

- RFI - Request for Information (project requirements are not fully defined)
- RFP - Request For Proposal (project requirements are more or less clear)
	- Discussion, questions
	- High-level design
	- Agreement
- Discovery - preparation, research, prioritisation
	- Artefacts
		- Teams
		- Goals
		- Timings
		- Tools (platforms, frameworks, programming langs, db, etc.)
		- func. requirements
		- non-func. requirements
- Construction - Planing, Development, Testing
	- Architect goals
		- check that func. requirements in compliance
		- check that non-func. requirements in compliance
- Deployment - release
- Analytic/Metrics/Support - checking results

## Solution/Application/Service Architecture

Result (artefact) of System Design Process

## Roles

There are several aspects thats needs architecture design. There could be additional roles to lead this process.

Sorted hierarchically (probably) 

### Enterprise Architect

Arch for whole enterprise system.

### Solution Architect

A role that gets some Business Goal and runs system design process, creates a blueprints of the future architecture to achieve this goal. Typically it will became architecture of a application, internal service, system, etc, depends on original goal.

example

goal: 
- make B2C communication faster and cheaper
solution: 
- implement ai-chatbot as microservice on current system
- buy and setup mail sender system, integrate it with current DB, analytics system.

Also Goal could be help to integrate Solution (B2B Product) to another service.
 
### Data Architect

Works with DB (scheme, logic), connections between services/DBs.

### System Architect

Deploy pipelines, CI/CD infra, etc

### Application Architect

Creates/Maintains architecture of particular app/service/microservice.

## Frameworks

### C4

Context-Container-Component-Code\
C4 model is a framework for visualizing the architecture of software systems. 

The model consists of four main types of diagrams, each representing a different level of detail:

1. **Context Diagram (Level 1)**:
    - **Purpose**: Provides a high-level view of the system within its environment.
    - **Focus**: Shows how the system interacts with external actors such as users, other systems, and external organizations.
    - **Elements**: Includes the system under consideration, external systems, and actors (e.g., users or other systems that interact with the system).
    - **Example**: A diagram showing the relationships between a web application, its users, and third-party services it interacts with.
2. **Container Diagram (Level 2)**:
    - **Purpose**: Zooms in on the system to show the high-level technology choices and how responsibilities are distributed across various containers (applications or services).
    - **Focus**: Illustrates the major containers (e.g., web servers, databases, microservices) within the system and their interactions.
    - **Elements**: Containers such as web applications, mobile apps, databases, and other runtime environments, along with their interactions.
    - **Example**: A diagram depicting a web application, a mobile app, a database, and how they communicate with each other.
3. **Component Diagram (Level 3)**:
    - **Purpose**: Provides a detailed view of the internal structure of a specific container.
    - **Focus**: Breaks down a container into its constituent components, such as classes, modules, services, and their interactions.
    - **Elements**: Components within a container and their relationships, such as dependencies or data flow.
    - **Example**: A diagram showing the internal structure of a web application, including services, controllers, repositories, and how they interact.
4. **Code Diagram (Level 4)**:
    - **Purpose**: Offers the most detailed view, focusing on the implementation details of individual components.
    - **Focus**: Usually consists of UML class diagrams or similar representations that show the structure and relationships of classes or code elements.
    - **Elements**: Classes, methods, properties, and their relationships within a component.
    - **Example**: A UML class diagram showing the classes in a particular module, their attributes, methods, and how they inherit or interact with each other.

## Project requirements

### Functional

as example, depends on project:
1. User Authentication and Authorization
2. Data Management
3. Business Process Workflows
4. Reporting and Analytics
5. Integration with Third-Party Systems
6. User Interface Design
7. Transaction Management
8. Communication Features
9. Compliance and Auditing
10. Document Management
11. Localization Support
12. Error Handling
13. Search and Filter Capabilities
14. Scheduling and Notifications
15. User Preferences and Settings

### Non-Functional

Prioritisation depends on project

list:
1. Performance
   - Response Time
   - Throughput
   - Load Handling
2. Scalability  
   - Vertical Scalability (Increasing resources)
   - Horizontal Scalability (Adding more nodes)
3. Security  
   - Data Encryption
   - Data Privacy
   - Auditing and Logging
4. Reliability  
   - Availability (Uptime)
   - Mean Time Between Failures (MTBF)
   - Mean Time to Repair (MTTR)
5. Usability  
   - User Interface Design
   - Accessibility
   - Learnability
6. Maintainability  
   - Code Quality
   - Modularity
   - Documentation
   - Ease of Upgrades
7. Portability  
   - Platform Independence
   - Hardware Independence
   - Compatibility with Different Operating Systems
8. Interoperability  
   - Integration with Other Systems
   - API Standards Compliance
9. Compliance  
   - Regulatory Compliance
   - Industry Standards Compliance
10. Backup and Recovery  
    - Data Backup Frequency
    - Disaster Recovery Time
11. Localization  
    - Multi-language Support
    - Regional Settings Adaptation
12. Capacity  
    - Storage Capacity
    - Maximum Concurrent Users
13. Flexibility  
    - Configurability
    - Extensibility
14. Efficiency  
    - Resource Utilization (CPU, Memory)
    - Energy Consumption
15. Supportability  
    - Diagnostic Tools
    - Troubleshooting Capabilities

## Methodology

Set of best practices and architectural guidelines for developing modern, scalable, and maintainable software applications

links:
- https://www.12factor.net/

### The Twelve Factors

1. Codebase:\
   One codebase tracked in revision control, many deploys
2. Dependencies: \
   Explicitly declare and isolate dependencies 
3. Config: \
   Store config in the environment
4. Backing services: \
   Treat backing services as attached resources
5. Build, release, run: \
   Strictly separate build and run stages
6. Processes: \
   Execute the app as one or more stateless processes
7. Port binding: \
   Export services via port binding
8. Concurrency: \
   Scale out via the process model
9. Disposability: \
   Maximize robustness with fast startup and graceful shutdown
10. Dev/prod parity: \
    Keep development, staging, and production as similar as possible
11. Logs: \
    Treat logs as event streams
12. Admin processes: \
    Run admin/management tasks as one-off processes
