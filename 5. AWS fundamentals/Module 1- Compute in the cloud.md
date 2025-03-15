
### Amazon Elastic Compute Cloud (Amazon EC2)

- **Purpose:** Secure, resizable compute capacity in the cloud.
    
- **Traditional On-Premises Setup:**
    - Requires upfront hardware purchases.
    - Long wait times for delivery and installation.
    - Need for physical data centers and maintenance.
    - High costs even if servers are underutilized.
- **Advantages of EC2:**
    - **Quick Provisioning:** Launch EC2 instances in minutes, no need for hardware setup.
    - **Pay-as-you-go:** Only pay for compute time while instances are running.
    - **Scalability:** Easily start or stop instances based on workload demands, avoiding excess costs.
- **Flexibility:** Run applications in the cloud without long-term server commitments.

#### How EC2 works 

**1. Launch**
- **Select a template:** Choose operating system, application server, and applications.
- **Instance type:** Pick the hardware configuration for your instance.
- **Security settings:** Define network traffic rules for incoming/outgoing connections.

**2. Connect**
- **Multiple connection methods:** Applications and users can connect directly to the instance.
- **User login:** Access the instance like a regular computer desktop.

**3. Use**
- **Run commands:** Install software, add storage, manage files, etc., once connected.

##### Multi-Tenancy
- **Shared host machines:** EC2 uses virtualization to run multiple instances on one physical machine.
- **Hypervisor:** Manages resource sharing between virtual machines (VMs).
- **Isolation:** EC2 instances are secure and separated from each other, ensuring no data or interaction occurs between shared instances.
---
#### EC2 Instance Types 

**1. General Purpose Instances**
- **Balance of compute, memory, and networking.**
- **Use Cases:** Application servers, gaming servers, backend servers, small/medium databases.
- **Ideal for:** Workloads with equal resource demands (compute, memory, network).

**2. Compute Optimized Instances**
- **Optimized for high-performance processors.**
- **Use Cases:** High-performance web servers, compute-intensive applications, batch processing, dedicated gaming servers.
- **Ideal for:** Compute-bound tasks requiring heavy processing power.

**3. Memory Optimized Instances**
- **Designed for fast performance with large datasets in memory.**
- **Use Cases:** High-performance databases, real-time processing of unstructured data.
- **Ideal for:** Workloads requiring large data to be preloaded and processed quickly in memory.

**4. Accelerated Computing Instances**
- **Use hardware accelerators (coprocessors) for efficient processing.**
- **Use Cases:** Graphics processing, game streaming, data pattern matching, floating-point calculations.
- **Ideal for:** Applications needing enhanced graphics or expedited data processing.

**5. Storage Optimized Instances**
- **Optimized for high, sequential read/write access to large datasets.**
- **Use Cases:** Distributed file systems, data warehousing, high-frequency OLTP systems.
- **Ideal for:** Applications needing high IOPS (input/output operations per second) performance for fast data access and processing.
---
#### EC2 Pricing

1. **On-Demand Instances:**
- Pay-as-you-go: No upfront cost or long-term contract.
- Ideal for: Short-term, irregular workloads or unpredictable usage (e.g., development/testing).
- Drawback: Less cost-efficient for workloads lasting a year or longer.

2. **Savings Plans:**
- Commitment-based savings: Commit to a consistent amount of compute usage for 1 or 3 years and provide more flexibility and apply across multiple instance types, making them ideal for users with dynamic or changing workloads.
 - Cost savings: Up to 72% over On-Demand costs.
- Ideal for: Steady-state workloads that are predictable over time.

3. **Reserved Instances:**
- Billing discount: Applicable for 1- or 3-year terms with greater savings for longer commitments On-Demand Instances in your account. Offer greater savings but with less flexibility, making them better for long-term, fixed workloads.
- Ideal for:  Steady-state or predictable workloads (e.g., databases, web apps).
- Types:
    - Standard Reserved: Higher savings, less flexibility.
    - Convertible Reserved: Flexibility to change instance types.
    - Scheduled Reserved: For workloads at specific times.

4. **Spot Instances:**
- Deep discounts: Up to 90% off On-Demand pricing.
- Risk of interruption: AWS can reclaim the instance with 2-minute notice.
- Ideal for: Flexible workloads that can handle interruptions to start and end times (e.g., batch processing, background jobs).

5. **Dedicated Hosts:**
- Dedicated physical servers: Full control over the server, used for meeting compliance requirements.
- Ideal for: Workloads needing dedicated tenancy or specific software licensing.
- Most expensive option compared to other pricing models.

---
#### Scalability and EC2 Auto Scaling

**1. Scalability Overview**
- **Definition:** Start with only the resources you need and scale out/in automatically based on demand.
- **On-Premise Data Center Dilemma:**
    - Buy too little hardware: unable to handle peak loads, affecting customer experience.
    - Buy too much: idle resources and wasted costs during low demand.

**2. Amazon EC2 Auto Scaling**
- **Functionality:** Automatically adds/removes EC2 instances based on application demand.
- **Benefits:**
    - Ensures application availability and responsiveness.
    - Reduces costs by using resources only when needed.

**3. Scaling Approaches**
- **Dynamic Scaling:** Adjusts instance count in real-time, responding to actual demand changes.
- **Predictive Scaling:** Anticipates future demand and schedules scaling based on predictions.

**4. Auto Scaling Group Configuration**
- **Minimum Capacity:** The minimum number of EC2 instances always running (e.g., at least 1 instance).
- **Desired Capacity:** The number of instances you'd prefer to run under normal conditions (e.g., 2 instances).
- **Maximum Capacity:** The upper limit for the number of instances the group can scale to (e.g., 4 instances).

**5. Cost-Effectiveness**
- **Pay-for-use model:** Only pay for the instances used during scaling, reducing idle resource costs.
- **Improved Customer Experience:** Ensures availability during peak times while keeping expenses low.
---
### Elastic Load Balancing (ELB)

**1. Elastic Load Balancing (ELB) Overview**
- **Function:** Automatically distributes incoming application traffic across multiple resources (e.g., Amazon EC2 instances).
- **Acts as Single Point of Contact:** Incoming traffic routes through the load balancer, which spreads requests across multiple instances to balance the workload.

**2. Integration with Auto Scaling**
- **Works with Amazon EC2 Auto Scaling:**
    - As EC2 instances are added or removed (due to scaling), the load balancer adjusts traffic distribution to available instances.
    - Ensures high performance and availability of applications.

**3. Scalability and High Availability**
- **Automatic Scalability:** ELB scales automatically as traffic grows, without the need for user intervention or changes in cost.
- **Regional Service:** ELB operates at the regional level, ensuring high availability by default.

**4. True Decoupled Architecture**
- **Decoupling of Front End and Back End:** The load balancer directs traffic based on which back-end instance has the least outstanding requests. The front end doesn't need to know how many instances are running in the back end.

**5. Example: Coffee Shop Analogy**
- **Low-Demand Period:** A few registers (EC2 instances) are open, matching low customer demand. ELB evenly distributes traffic across them.
- **High-Demand Period:** As customer demand increases, more registers (EC2 instances) open. The load balancer directs customers to the available registers, distributing traffic evenly.

**6. Traffic Handling**
- **Scaling Out:** As new EC2 instances come online, they inform the load balancer, which begins routing traffic to them.
- **Scaling In:** ELB drains existing requests before the instance is terminated, ensuring no disruption to users.

**7. Benefits:**
- **Efficient Resource Usage:** ELB prevents any single EC2 instance from becoming overloaded by distributing requests evenly.
- **Improved Customer Experience:** High availability and smooth scaling improve response times and ensure consistent performance during traffic spikes.
---



### Messaging and Queuing 

**1. Tightly Coupled vs. Loosely Coupled Architectures**
- **Tightly Coupled Architecture:**
    - In this scenario, components (e.g., Application A and Application B) communicate directly with each other.
    - If one component (like the barista in the coffee shop analogy) is unavailable or fails, the other component is affected, causing potential errors or slowdowns.
- **Loosely Coupled Architecture:**
    - Introduces a buffer (like an order board) between components. For instance, Application A sends messages to a queue, and Application B processes them when it's ready.
    - If Application B fails, Application A can continue sending messages to the queue without experiencing disruptions. This isolation prevents cascading failures.

**2. Message Queueing and Buffering**
    - A process in which messages (or requests) are placed into a buffer (or queue), allowing other components to process them asynchronously.
    - Ensures reliable communication between applications even if one of them is temporarily unavailable.

**3. AWS Services for Messaging and Queuing**
- **Amazon Simple Queue Service (SQS):**
    - A fully managed message queuing service that allows you to decouple components of an application.
    - Ensures that messages sent by one component are stored in a queue and processed when the receiving component (e.g., another application) is available.
- **Amazon Simple Notification Service (SNS):**
    - A fully managed notification service that allows for the immediate delivery of messages to multiple subscribers.
    - SNS is often used for pub/sub (publish-subscribe) models where a message is sent to multiple receivers (subscribers) at once.

**4. Monolithic vs. Microservices Architecture**
- **Monolithic Applications:**
    - Applications built as a single, tightly integrated unit where all components (e.g., databases, UI, business logic) are dependent on each other.
    - If one component fails, the entire application may be affected, potentially leading to widespread failures.
- **Microservices Architecture:**
    - An approach where the application is broken down into smaller, independent components (or services) that perform specific tasks.
    - These components are loosely coupled, so if one fails, the rest of the system can continue functioning.

**5. Example: Coffee Shop Analogy**
- **Tightly Coupled System:**
	- The cashier writes an order and hands it directly to the barista.
	- If the barista is busy, the cashier must wait, which causes delays.
- **Loosely Coupled System:**
	- The cashier writes the order and places it on the order board (queue).
	- The barista retrieves the order from the board when ready and prepares the drink. This ensures the cashier can continue taking new orders without waiting for the barista.

**6. Benefits**
- **Resilience:** Failures in one part of the system do not impact the whole system.
- **Scalability:** Components can scale independently.
- **Flexibility:** Easier to modify, update, or replace individual components.

---
### Amazon Simple Queue Service (SQS)

**Amazon SQS** is a fully managed message queuing service that enables you to decouple application components, allowing them to communicate asynchronously by sending, receiving, and processing messages at any scale. It ensures reliable message transmission without requiring the components (such as services, users, or software applications) to be available simultaneously.

**How SQS Works:**
1. **Sending a Message:** One component (e.g., the cashier in the coffee shop analogy) sends a message to the SQS queue.
2. **Message Stored in the Queue:** The message is held in the queue until the receiving component (e.g., the barista) is ready to process it.
3. **Retrieving a Message:** The receiving component pulls messages from the queue, processes them, and then deletes them from the queue once the task is complete.

**Key Components in SQS:**
- **Messages:** These are the pieces of information (payloads) sent between components. Each message contains the data necessary for processing (like the customer's coffee order and their name).
- **Queue:** A buffer where messages are stored until a receiving component retrieves and processes them. The queue ensures that messages are not lost and can be processed whenever the receiving application is available.
##### Advantages of Using Amazon SQS
1. **Decoupling Applications:**
    - SQS allows different components of an application to operate independently, reducing dependencies and allowing for greater flexibility.
    - In the coffee shop analogy, the cashier can keep taking orders and placing them into the queue, even if the barista is busy or unavailable, improving efficiency.
2. **Reliability:**
    - SQS automatically scales and is managed by AWS, ensuring reliable message storage and processing, even at high volumes.
    - The service guarantees that messages are stored until they are processed, providing durability and message retention until delivery.
3. **Asynchronous Processing:**
    - By placing messages in the queue, the sending and receiving applications do not need to operate at the same speed or be available at the same time.
    - This asynchronous communication model is useful for systems where tasks might take varying amounts of time to complete.


### Amazon Simple Notification Service (SNS)

**Amazon SNS** is a fully managed publish/subscribe (pub/sub) messaging service that enables you to send notifications to multiple subscribers simultaneously. It allows publishers to send messages to a topic, and all subscribers to that topic receive the messages in real-time. SNS is particularly useful for distributing time-sensitive information or alerts to multiple systems or users quickly and efficiently.

#### How Amazon SNS Works
1. **Publishing Messages:**
    - Publishers (like the coffee shop cashier) send messages to an SNS topic.
    - The topic acts as a channel for delivering messages to subscribers.
2. **Subscribing to Topics:**
    - Subscribers (customers, in our analogy) can choose to subscribe to one or more topics that interest them.
    - This allows for targeted communication based on the subscribers’ preferences.
3. **Distributing Notifications:**
    - When a message is published to a topic, SNS delivers it to all subscribed endpoints simultaneously.
    - Endpoints can be AWS Lambda functions, HTTP/S endpoints, mobile devices via push notifications, SMS, or email.

#### Key Features of Amazon SNS
1. **Flexibility in Subscriptions:**
    - Subscribers can choose the topics they want to receive notifications for, allowing for personalized communication.
2. **Multi-Protocol Delivery:**
    - SNS can deliver messages via multiple protocols, including:
        - Email
        - SMS
        - Mobile Push Notifications
        - HTTP/S endpoints
        - AWS Lambda functions
3. **Fan-out Messaging:**
    - SNS allows you to send a single message to multiple subscribers at once, ensuring that all interested parties receive updates in real-time.
    - This is useful for scenarios like sending alerts, notifications, or updates to different systems and users simultaneously.
4. **Integration with Other AWS Services:**
    - SNS can easily integrate with other AWS services, allowing for complex workflows. For example, it can trigger AWS Lambda functions or notify users based on specific events in your applications.
#### Example: Coffee Shop Notification System

**Single Newsletter Approach (Single Topic):**
- Initially, the coffee shop has a single newsletter covering multiple topics (coupons, coffee trivia, new products).
- All customers receive all updates, regardless of their interests.

**Multiple Newsletter Approach (Multiple Topics):**
- After receiving feedback from customers, the coffee shop creates three separate newsletters for each specific topic:
    - **Coupons**
    - **Coffee Trivia**
    - **New Products**
- Customers can now subscribe to the topics that interest them:
    - Customer 1 subscribes to **Coupons** only.
    - Customer 2 subscribes to **Coffee Trivia** only.
    - Customer 3 subscribes to both **Coffee Trivia** and **New Products**.

#### Use Cases for Amazon SNS

- **Real-time Notifications:** Sending alerts or updates to users about important events, such as order statuses or system health checks.
- **Decoupled Microservices Communication:** Allowing microservices to communicate efficiently without tight coupling, facilitating better scalability and resilience.
- **Mobile App Notifications:** Delivering push notifications to mobile devices for engagement and updates.
- **Multi-Channel Messaging:** Supporting various notification methods for users based on their preferences.


---
### SQS vs. SNS (Simple Notification Service)

- **SQS (Queue-Based Messaging):**
    - Best suited for decoupling components and allowing asynchronous communication between them.
    - The queue holds messages until they are processed, making SQS ideal for handling workloads where messages need to be processed one by one, like in our coffee shop example.
- **SNS (Topic-Based Messaging):**
    - Ideal for sending messages to multiple subscribers at once (pub/sub model).
    - SNS works best when you need to broadcast messages to many systems or users at the same time.

In summary, **Amazon SQS** is an excellent solution for decoupling components and ensuring reliable communication across distributed systems, while **Amazon SNS** is more appropriate for broadcasting messages to multiple recipients simultaneously.

---

### Serverless Computing

**Serverless computing** allows you to build and run applications without managing the infrastructure. While servers are still involved, developers don’t have to handle server provisioning, scaling, patching, or maintenance. The cloud provider takes care of these tasks, allowing you to focus solely on writing code and building features.

#### Differences from Traditional EC2

In a traditional **Amazon EC2** setup:

1. You need to provision virtual servers (instances).
2. Upload your code to the servers.
3. Continuously manage and scale these instances, apply patches, and ensure high availability.

With **serverless computing**, none of these tasks are required. AWS handles the underlying infrastructure, which includes provisioning, scaling, and maintaining servers. This gives developers the freedom to concentrate on their application logic rather than operational concerns.



#### Key Benefits of Serverless Computing

1. **No Server Management:**
    - There’s no need to manage EC2 instances or worry about patching, scaling, or ensuring availability. The cloud provider, like AWS, handles these operations.
2. **Automatic Scaling:**
    - Serverless applications scale automatically based on demand, ensuring optimal resource usage.
3. **Cost Efficiency:**
    - You only pay for the compute time you actually use. If your code isn’t running, you aren’t charged.
4. **Faster Innovation:**
    - By abstracting infrastructure management, developers can focus more on developing new features and improving applications.

### AWS Lambda 

**AWS Lambda** is a fully managed serverless compute service that runs your code in response to triggers or events. It handles the heavy lifting of server management and scaling. Here's how Lambda operates:

#### How AWS Lambda Works

1. **Upload Your Code:**
    - You create a **Lambda function** by uploading your code in languages supported by AWS Lambda (like Node.js, Python, Java, etc.).
2. **Set Triggers:**
    - Your function can be triggered by events from various sources like AWS services (S3, DynamoDB), HTTP endpoints (API Gateway), or even custom events from applications.
3. **Execution Upon Trigger:**
    - When a trigger is detected, Lambda automatically runs your code in a managed, serverless environment.
4. **Pay Per Execution:**
    - You are billed only for the compute time when the code is running. Charges are based on the number of requests and the time it takes to execute.

#### Example Use Case: Image Resizing

One of the simplest and most common Lambda use cases is **automatically resizing images**. Here's how it works:

1. **Trigger:**
    - An event is triggered when a user uploads an image to an S3 bucket.
2. **Lambda Execution:**
    - Lambda runs the code that processes and resizes the image.
3. **Output:**
    - Once processed, the resized image is saved back to the S3 bucket. You only pay for the time Lambda takes to resize the image.


#### Key Features of AWS Lambda

1. **Automatic Scaling:**
    - Lambda scales based on the number of events that trigger the function. It can handle thousands of concurrent requests without the need for manual intervention.
2. **High Availability:**
    - Lambda functions are automatically available across multiple availability zones, ensuring uptime and redundancy.
3. **Event-Driven:**
    - Lambda is designed to respond to specific events or triggers, making it an ideal solution for event-driven architectures (e.g., responding to API requests, processing IoT events, or automating tasks).
4. **Short-lived Execution:**
    - Each Lambda function is designed to run code for a maximum of **15 minutes**, making it suitable for fast, lightweight tasks like web requests or background processes.

### Serverless Use Cases

- **Web Application Backends:** Handle web requests, API responses, or user authentication without managing server infrastructure.
- **Data Processing Pipelines:** Process real-time streams of data (e.g., log analysis, event processing).
- **Automation:** Automate tasks like file uploads, database backups, or scheduled tasks.
- **Microservices:** Build and deploy individual services that are part of a larger microservices architecture.

---

### Containers on AWS

Containers are a standardized way to package an application and its dependencies into a single unit that can run consistently across different environments. In AWS, containers are most commonly deployed using **Amazon Elastic Container Service (ECS)** or **Amazon Elastic Kubernetes Service (EKS)**.

#### What are Containers?

A **container** is a lightweight, standalone package that includes everything needed to run an application: code, runtime, system tools, libraries, and settings. They are similar to virtual machines but more efficient, as they share the host system’s kernel instead of requiring a full operating system for each instance.

Containers are created using **Docker**, a popular platform for containerization. Docker containers allow developers to create consistent environments for applications, ensuring that software runs the same way on any system.

#### Example Use Cases

1. **One Host with Multiple Containers:**
    - Suppose a developer builds an application on their local machine, but the environment differs from the production environment. Containers ensure that the app’s environment remains the same across all stages of development, from local machines to production servers. This consistency reduces bugs caused by differing configurations between environments.
2. **Managing Many Containers:**
    - As applications scale, you may need to run hundreds or even thousands of containers across multiple hosts. Managing the lifecycle of these containers—starting, stopping, and monitoring—can become overwhelming without automation.



#### Container Orchestration on AWS

**Container orchestration** automates the deployment, scaling, and management of containerized applications. AWS offers two major container orchestration services: **Amazon ECS** and **Amazon EKS**.

### Amazon Elastic Container Service (ECS)

Amazon ECS is a fully managed container orchestration service that helps you deploy, manage, and scale containerized applications on AWS without managing your own orchestration infrastructure. ECS integrates seamlessly with other AWS services, such as **Amazon EC2** for compute resources or **AWS Fargate** for serverless container deployment.

- **Use Case:**
    - ECS is a good choice if you’re looking for simplicity and ease of use within the AWS ecosystem. It handles many of the underlying complexities of managing clusters and containers, allowing you to focus on your applications.
- **Key Features:**
    - **Fully managed**: AWS takes care of the orchestration infrastructure.
    - **Seamless integration**: Tight integration with other AWS services like EC2 and Fargate.
    - **Scalability**: Easily scale containerized applications across multiple instances.

### Amazon Elastic Kubernetes Service (EKS)

Amazon EKS is a fully managed service that allows you to run **Kubernetes** on AWS without needing to install and maintain your own Kubernetes control plane. Kubernetes is an open-source container orchestration tool that provides even greater flexibility and portability for running containers at scale.

- **Use Case:**
    
    - EKS is ideal if you're already familiar with Kubernetes or need more granular control over how your containers are managed. It's useful for applications that need multi-cloud or hybrid-cloud deployments since Kubernetes is widely supported outside of AWS.
- **Key Features:**
    
    - **Kubernetes compatibility**: EKS uses upstream Kubernetes, so there’s no need to change your existing configurations.
    - **Multi-cloud and hybrid cloud**: Kubernetes is not AWS-specific, allowing you to deploy containers across other cloud providers or on-premises.
    - **Community and ecosystem**: Kubernetes has a large, active community with a rich ecosystem of tools.



### Container Orchestration Comparison: ECS vs. EKS

|Feature|Amazon ECS|Amazon EKS|
|---|---|---|
|**Ease of Use**|Easier to use within the AWS ecosystem.|More complex but provides more flexibility.|
|**Control**|Less granular control over orchestration.|Fine-grained control with Kubernetes.|
|**Multi-cloud Support**|AWS-specific service.|Supports hybrid and multi-cloud environments.|
|**Best For**|Simplicity, AWS-specific workloads.|Advanced users, Kubernetes-native workflows.|

#### Running Containers on AWS

You can run containers in two main ways:

1. **Amazon EC2:** You manually manage the EC2 instances that run your containers. This offers more control but requires manual scaling and maintenance.
    
2. **AWS Fargate:** A serverless option where AWS manages the infrastructure. You just define your containerized applications, and Fargate handles provisioning, scaling, and management behind the scenes.


