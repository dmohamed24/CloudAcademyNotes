
create a load balancer 

link load balancer to vpc

create auto scaling group in pulic subtent of the defualt vpc

link the asg to the load balancer  

## CMDS
terraform state list
terraform plan --target=aws_security_group.ec2_sg -destroy
terraform destroy --target=aws_security_group.ec2_sg


# VPC
### 1. **`aws_vpc`**

Defines the Virtual Private Cloud.

- Set CIDR block (e.g., `10.0.0.0/16`)
    
- Enable DNS support (recommended)
### 2. **`aws_subnet`**

Creates a subnet within the VPC.

- Associate with the VPC
    
- Provide CIDR block (e.g., `10.0.1.0/24`)
    
- Set `map_public_ip_on_launch = true` (to assign public IPs to instances in this subnet)
### 3. **`aws_internet_gateway`**

Allows traffic between your VPC and the internet.

- Attach to the VPC
### 4. **`aws_route_table`**

Defines routing rules for your VPC.

- Associate with the VPC
    
- Create a route to send traffic to the internet via the internet gateway
### 5. **`aws_route`**

Adds a route to the route table to send all outbound traffic (`0.0.0.0/0`) through the internet gateway.

- Depends on `aws_internet_gateway`
### 6. **`aws_route_table_association`**

Links the route table to the public subnet.
### 🧩 Optional (Best Practice):

- **`aws_vpc_ipv4_cidr_block_association`** if using additional CIDR blocks
    
- **Tags** for all resources (e.g., `Name`, `Environment`)



## ASG

### 1. **`aws_launch_template`** _(or `aws_launch_configuration`, but `launch_template` is preferred)_

Defines how to launch EC2 instances:

- AMI ID
    
- Instance type
    
- Key pair (optional)
    
- Security groups
    
- User data (optional)
    

> 🔑 Note: You will reference this in the ASG.

---

### 2. **`aws_autoscaling_group`**

Defines the auto scaling group:

- Set `max_size = 3`, `desired_capacity`, `min_size`
    
- Reference the launch template above
    
- Define VPC subnet(s) (from Step 1)
    
- Add a health check type (e.g., `EC2` or `ELB` if using a load balancer)
    
- Set `target_group_arns` if attaching to a load balancer (step 3)
    

---

### 3. **`aws_security_group`**

Security group for EC2 instances:

- Allow SSH (port 22) from your IP (for testing)
    
- Allow HTTP/HTTPS (if web server)
    
- Attach to `aws_launch_template`
    

---

### 4. **(Optional) `aws_autoscaling_policy`**

Defines scale-in and scale-out policies based on metrics like CPU utilization. You can skip this for static scaling with min=1, max=3.


## ALB

### 1. **`aws_lb`**

Creates the **Application Load Balancer (ALB)**:

- Type: `application`
    
- Scheme: `internet-facing`
    
- Subnet IDs: Use **public subnets** (from Step 1)
    
- Security group: Allow HTTP/HTTPS
    

---

### 2. **`aws_lb_target_group`**

Defines a target group for the ALB:

- Protocol: `HTTP`
    
- Port: 80 (or your app port)
    
- VPC ID: VPC from Step 1
    
- Target type: `instance` or `ip` (use `instance` if attaching EC2s via ASG)
    

---

### 3. **`aws_lb_listener`**

Defines how the ALB listens for traffic:

- Protocol: `HTTP` (or HTTPS if using SSL certs)
    
- Port: 80
    
- Default action: Forward to the target group
    

---

### 4. **`aws_autoscaling_attachment`** _(or use `target_group_arns` inside ASG)_

Associates the Auto Scaling Group with the target group:

- Required to register EC2s from ASG with the ALB
    

> ✅ This can also be done inline using `target_group_arns` in the `aws_autoscaling_group`.

---

### 5. **`aws_security_group`** (for ALB)

ALB needs its own security group:

- Inbound: Allow HTTP (port 80) or HTTPS (port 443)
    
- Outbound: Allow all or specifically to instances
    

---

### 🧩 Optional:

- **`aws_lb_listener_rule`**: Add routing conditions (e.g., path-based routing)
    
- **SSL/TLS setup**: Use ACM for HTTPS support (`aws_acm_certificate`, `aws_lb_listener` with `HTTPS`)