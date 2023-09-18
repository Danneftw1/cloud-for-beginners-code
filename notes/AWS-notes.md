# AWS
## IAM Overview (IAM)
### AWS Identity and Access Management (IAM)

![Alt text](image-1.png)

AWS Identity and Access Management (IAM) is a service that helps you control access to AWS resources. It allows you to create and manage users, groups, and permissions securely.

### IAM Authentication Methods

![Alt text](image.png)

IAM Authentication Methods refer to the ways users and services prove their identity to access AWS resources. These methods include username/password, access keys for API calls, and roles assumed by services or federated users.

---
## Amazon VPC

![Alt text](image-2.png)
![Alt text](image-3.png)

Amazon VPC (Virtual Private Cloud) is a service provided by AWS (Amazon Web Services) that allows you to create an isolated, private network in the cloud. With a VPC, you can define your own network space with IP address ranges, subnets, and security settings. This enables you to run resources like EC2 instances or databases securely and with fine-grained control over network configuration and accessibility.

**Why use a VPC?**: It provides a secure, isolated enviroment for your cloud resources, offering better control over network configuration, improved security, and easy scalability. It resembles a traditional network you'd operate in your own data center.

A subnet, or subnetwork, is a smaller, segmented part of a larger network. It allows you to divide a network into smaller, more manageable pieces for purposes like security, traffic management, or resource allocation. Each subnet has its own unique IP address range, which is a subset of the larger network's IP address range. In the context of an Amazon VPC, you can create multiple subnets to isolate different types of resources or to configure specific access rules.

NAT-gateways is the part that costs money when creating and using VPCs. If you don't know if you're going to use it or don't need it, you can get rid of it by doing this: 

VPC -> NAT gateways -> Actions -> Delete NAT Gateway

When you delete the NAT gateway it's still going to leave behind the elastic IP-adress. You only pay for elastic IPs when you're not using them. In this case we need to delete it by following these steps: 

VPC -> Elastic IPs -> Actions -> Release Elastic IP Addresses

It shouldn't cost us anything since the NAT Gateway was only active for a few minutes and the Elastic IP was only detached for a couple of seconds (from tutorial). 

Why should you use a NAT-gateway? Without a NAT Gateway, resources in private subnets won't be able to access the internet directly. They can still communicate within the VPC and with other AWS services that are configures to allow such traffic. Internet is not a requirement depending on our use case, operating without a NAT Gateway is entirely feasible.

![Alt text](image-4.png)

AWS Public Services are accessible over the internet and include services like S3 and EC2. Private Services are only accessible within your VPC, such as VPC endpoints or private RDS instances.

---

## Stateful vs Stateless Firewalls

![Alt text](image-6.png)

Stateful and stateless firewalls are two types of firewalls that filter network traffic based on different criteria:

* **Stateful Firewalls:** These keep track of the state of active connections and make decision based on the context of the traffic, such as TCP handshake completion or established sessions. This allows them to apply rules based on the entire communcation session, not just individual packets. Stateful firewalls are more secure and flexible but can require more processing power.

* **Stateless Firewalls:** These filter traffic based on the source and destination without considering the state of the connection. Each packet is inspected individually and either accepted or rejected based on pre-set rules. Stateless firewalls are faster and require less memory because the don't keep track of connection states, but they are generally considered less secure compared to stateful firewalls.

The choice between stateful and stateless firewalling depends on your specific needs for performance, security and complexity

---

## Security Groups & Network ACLs


![Alt text](image-7.png)


Security Groups and Network Access Control Lists (ACLs) are both used for configuring network security in Amazon VPC, but they operate at different levels and offer different types of control.

* **Security Groups:** These act as virtual firewalls for instances like EC2, controlling inbound and outbound traffic at the instance level. Security groups are stateful, meaning if you allow an incoming request from an IP, the response is automatically allowed, regardless of outbound rules.

* **Network ACLs:** These are similiar to firewalls that control traffic going in and out of a subnet within your VPC. Unlike Security Groups, Network ACLs are stateless, so you need to specify both inbound and outbound rules explicitly. They offer rule numbering and allow you to create rules that deny traffic, giving you more granular ("grainy") control

Both can be used together for layered security, with Security Groups acting as the first line of defense at the instance level and Network ACLs providing broader subnet-level protection.

![Alt text](image-8.png)


## Stateful vs Stateless Applications

![Alt text](image-5.png)

Stateful and stateless applications differ in how they manage and utilize data across multiple interactions.

* **Stateful Applications:** These applications maintain state between different interactions or transactions. Information from one session, like user preferences or data, is stored and can be referenced in future sessions. Databases and most interactive applications like shopping carts are examples of stateful applications.

* **Stateless Applications:** These applications do not save any client state between interactions. Each transaction is processed without reference to past or future transactions. Stateless applications are easier to scale horizontally since each request is independent. HTTP and RESTful web services are often stateless. 

The choice between stateful and stateless design depends on your application's needs for scalability, complexity and data persistence.

**What is the difference between horizontal and vertical scaling?**

* **Horizontal:** Adds more machines to your existing setup to distribute the load. **EXAMPLE:** If you have a web app running on a single server, horizontal scaling would involve adding more servers and distributing incoming traffic among them. This is often easier to implement and is more flexible, allowing you to scale out or in based on demand.

* **Vertical:** This involves upgrading the resources on an existing machine, such as adding more RAM, CPU or storage. You're essentially making your individual server more powerful. Vertical scaling often requires downtime for hardware upgrades and has an upper limit defined by the capabilites of the individual servers.

Horizontal scaling is generally more adaptable and can offer better fault tolerance, while vertical scaling is often simpler but has its limitations.

---

# EC2 (Elastic Cloud)

EC2 Provides resizable compute resources in the cloud, making it easier to deploy applications and handle varying workloads. You can launch virtual machines, known as instances, with different configurations of CPU, memory, storage, and  networking capabilities (this is within a VPC). EC2 offers flexibility with options for on-demand pricing, reserved instances, and spot instances, enabling cost optimization based on your needs. It intergrates with many AWS services like Amazon RDS for databases and Amazon S3 for storage, providing a comprehensive enviroment for running applications in the cloud.

![Alt text](image-10.png)

**What are Public, Private, and Elastic IP addresses within EC2?**

* **Public IP:** Temporary, internet-reachable address assigned when an EC2 instance is launched.
* **Private IP:** Permanent address for internal VPC communaction, not accessible from the internet.
* **Elastic IP:** Static, public IP you can allocate and keep until you release it. Internet-reachable and can be reassigned to different instances.

Each type serves different use-cases: Public for temporary internet access, Private for internal VPC communication, and Elastic for more control and persistence.

![Alt text](image-11.png)

### Public Subnets:
A subnet with a route to the internet, typically via an internet Gateway. Instance in a public subnet can have public IP address and can be aaccessed directly from the internet.

![Alt text](image-12.png)

### Private Subnets:

A subnet without direct route to the internet. Instances can communicate within the VPC but can't be accessed directly from the internet. Internet access, if needed, is usually via a NAT Gateway.

![Alt text](image-13.png)

### Launching an EC2 instance
See How to guides, Section 6 "How to launch an EC2 instance"

---

# Access Keys and IAM Roles with EC2

Using access keys with EC2 is easier, however you risk exposing information to outsiders

![Alt text](image-14.png)

Using roles is instead more secure, however, the initial setup is abit more complex.

![Alt text](image-15.png)








# How to guides: 

## Section 5:
* How to create IAM User and Group? **28**
* How to create a custom VPC? **30**
* How to create a Security Group? **32**
* How to configure AWS CLI (Command Line Interface)? **33** 

## Section 6: 
* How to launch an EC2 Instance? **36**
* How to connect to EC2? **37**
* How to practise with Access Keys and IAM Roles? **39**
* How to create a website with User Data? **40**

# Terms and Definitions:

* **S3(Simple Storage Service):** It's a scalable, durable cloud storage service from AWS. It's used for storing and retrieving data like files, backups, or as a backend for applications. Offers features like access control and data versioning.