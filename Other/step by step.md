## 🏗️ 1. **VPC and Networking**

### ✅ What you did:

- Created a **VPC** with a **public subnet**
    
- Attached an **Internet Gateway (IGW)** to the VPC
    
- Created a **route table** that sends traffic `0.0.0.0/0` to the IGW
    
- Associated that route table with the **public subnet**
    

### ✅ Result:

- **Any instance** in this subnet can access the internet **and be accessed from the internet**, assuming the instance has a public IP and correct security group.
    

---

## 🔐 2. **Security Groups**

### 🔹 EC2 Security Group (`ec2_sg`)

- Ingress rules:
    
    - Allows traffic on:
        
        - **Port 22** for SSH (from VPC CIDR – should be `0.0.0.0/0` or restricted IP)
            
        - **Port 3000** for your app (originally from VPC CIDR, but should be from ALB SG)
            
- Egress: All traffic to anywhere (`0.0.0.0/0`)
    

> ✅ EC2 is protected and only receives traffic allowed explicitly (port 22 or 3000 from ALB SG).

---

### 🔹 ALB Security Group (`alb_sg`)

- Ingress:
    
    - Port 80 from `0.0.0.0/0` (anyone on the internet)
        
- Egress: (default all)
    

> ✅ ALB accepts HTTP traffic from the internet, and can forward it to targets (EC2s).

---

## 🌐 3. **ALB, Target Group & Listener**

### 🔹 Application Load Balancer (`aws_lb`)

- Internet-facing
    
- Lives in public subnet
    
- Has security group (`alb_sg`)
    

### 🔹 Target Group

- Type: `instance`
    
- Protocol: `HTTP`
    
- Port: **80** (⚠️ this should be **3000** if your app is running on port 3000 inside the EC2)
    

> 💡 If your app listens on port 3000, make the target group match that port.

hcl

CopyEdit

`port     = 3000 protocol = "HTTP"`

### 🔹 Listener

- Listens on port 80 (public)
    
- Forwards traffic to target group
    

---

## ⚙️ 4. **Launch Template & Auto Scaling Group**

### 🔹 Launch Template

- Specifies:
    
    - EC2 AMI
        
    - Instance type
        
    - User data (installs Apache, serves HTML page)
        
    - Security group
        
    - Key name (for SSH access)
        

### 🔹 Auto Scaling Group (ASG)

- Uses the Launch Template
    
- Spreads instances across public subnet(s)
    
- Registers instances with the ALB's **target group**
    
- Maintains:
    
    - `min_size = 1`
        
    - `desired_capacity = 2`
        
    - `max_size = 3`
        

### 🔹 Auto Scaling Policy

- A simple policy to increase capacity by 1 (can be triggered by CloudWatch metrics, etc.)
    

---

## 📡 When You Hit the ALB's Public IP (Port 80)

Let's assume:

- Your **app runs in Docker** on port **3000**
    
- The **EC2 instance exposes port 3000**
    
- Your **ALB Target Group is configured for port 3000**
    
- Security groups are set up correctly
    

---

### 🔁 Step-by-Step Request Flow:

#### 🔸 Step 1: DNS/Public IP Request

- A user accesses the **ALB public IP or DNS name** (e.g., `http://<alb-dns>`)
    

#### 🔸 Step 2: ALB Security Group

- ALB allows the inbound HTTP request on port 80
    

#### 🔸 Step 3: ALB Listener

- Listener on port 80 matches the request
    
- It forwards the request to the **Target Group**
    

#### 🔸 Step 4: Target Group → EC2

- Target Group is configured to forward traffic to port **3000** on EC2 instances
    
- ALB picks a healthy EC2 instance from the group
    

#### 🔸 Step 5: EC2 Security Group

- EC2 SG allows inbound on port 3000 **from ALB SG**
    

#### 🔸 Step 6: EC2 Receives Request

- Docker container on the EC2 listens on port 3000
    
- App processes the request and sends a response
    

#### 🔸 Step 7: Response → ALB → Client

- Response goes back to ALB
    
- ALB returns it to the original client
    

---

## ✅ Final Checklist for This to Work:

| Component      | Requirement                                           |
| -------------- | ----------------------------------------------------- |
| ALB            | `port 80` listener and SG allowing 0.0.0.0/0          |
| Target Group   | `port = 3000`, protocol HTTP                          |
| EC2 SG         | Allows port `3000` **from ALB SG**                    |
| EC2 Docker App | Listens on port `3000`                                |
| Public Subnet  | Has route to IGW and `map_public_ip_on_launch = true` |
| AMI/User Data  | Runs Docker container or app server                   |