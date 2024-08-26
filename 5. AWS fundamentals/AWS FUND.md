- regions ✅
- availability zones ✅
- local zones ✅
- edge locations✅
- vpc ✅
- private subnet ✅
- public subnet ✅
- internet gateway ✅
- subnet route table  ✅
- nat gateway ✅
- security groups ✅
- network access control list ✅


### AWS global structure
#### Regions
- A region is a physical location in the world and is each region is a separate geographical location for example eu-west-2 = Europe (London)
- Each region has 2 or more availability zones which means you can deploy resources across availability zones to provide redundancy 
- Each region is designed to be isolated from other regions, this is to achieve the greatest possible fault tolerance and stability

#### Availability zones 
- Each aws region has multiple isolated locations called availability zones that are composed of one or more data centers
- AZ provide access to all aws services and high availability within a region 

#### Local zones
- Local zones allow you to extend resources like compute and storage in multiple locations closer to the user for low latency access 
- Local zones are an extensions of region meaning they live outside a region and note with in it like an availability zone

#### Edge locations 
- AWS Edge Locations are strategically positioned points in the AWS network optimized for low-latency content delivery, ensuring that data reaches users swiftly. In contrast, AWS regions are larger geographic areas with multiple Availability Zones
- They are part of Amazon's content delivery network (CDN) and are designed to reduce the distance data needs to travel to reach the user

### VPC

#### VPC
A virtual private cloud is a virtual network that's created with in a public cloud to provide a secure and isolated environment for an organizations resources

- Isolation: VPC's create a private network that's separate from other cloud networks which adds a layer of security 
- Control: VPCs allow the user to define a range of ip address, create subnets and set up network  gateways 

#### Subnet 
A subnet is a range of IP addresses in your VPC. A subnet must reside in a single Availability Zone. 

#### Subnet route table 
A subnet route table is a set of rules, or routes, that determine where network traffic from a subnet is directed in a VPC

#### Public subnet
A public subnet is a subnet in a VPC that has a direct route to the internet gateway. This allows resources in the public subnet like EC2 to be directly accessible from the internet.  

- A server in a public subnet is a assigned a public ip address which the allows the subnet to react the internet and the internet reach the subnet 

#### Private Subnet
A private subnet is a subnet in a VPC that has no direct route to the internet gateway., This means that resources in a private subnet can not be directly access by the internet but they can be accessed through a NAT device 

#### Internet gateway (IGW)
IGW is a component that allows network traffic to flow between a VPC and the public internet 
- Function: Acts as a bridge between 2 networks, allowing inbound and out bound connections from resources within the vpc 
- Benefits: An IGW allows resources in a public subnet  to access the internet such as a EC2 instance 
#### Network address translation (NAT)
NAT gateway is  a service that allows instances in a private network to connect to resources outside of the VPC while also preventing external services over the internet to establish a connecting with those instances

#### Security groups 
A security group is a virtual firewall that manages incoming and outgoing traffic from aws resources like EC2 instances. 
- security groups work by filtering out traffic at the TCP and ip layers, based on the source and destination ip addresses and specific ports. 

#### Network access control list (NACL)
A NACL is a security layer thjat controls traffic in and out of subnets. Similar to a firewall and acts as a barrier to ensure only authorized traffic enter and leave
