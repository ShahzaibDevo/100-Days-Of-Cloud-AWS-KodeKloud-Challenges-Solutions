
# Day 30: Enable Internet Access for Private EC2 using NAT Instance

## Project Overview
This project demonstrates how to enable internet access for a **private EC2 instance** using a **NAT Instance** in AWS. The setup ensures secure architecture where the private instance has outbound internet access but remains inaccessible from the public internet.

---

## Objective
- Create a secure VPC architecture
- Enable internet access for private EC2 using NAT Instance
- Understand routing, NAT, and VPC networking concepts

---

## Architecture Design
- Public Subnet → NAT Instance + Internet Gateway  
- Private Subnet → EC2 Instance (no public IP)  
- NAT Instance → Handles outbound internet traffic  

---

## Step-by-Step Implementation

### 🔹 1. Create VPC
- Use existing VPC or create a new one
- Ensure CIDR block is properly configured

---

### 🔹 2. Create Public Subnet
- Name: `datacenter-pub-subnet`
- Enable auto-assign public IPv4

---

### 🔹 3. Create and Attach Internet Gateway (IGW)
- Create IGW
- Attach to VPC
- Update public route table:

```

0.0.0.0/0 → Internet Gateway

````

---

### 🔹 4. Launch NAT Instance (Amazon Linux 2023)
- Launch EC2 in public subnet
- Assign public IP
- Security Group:
  - SSH (22)
  - All outbound traffic allowed

---

### 🔹 5. Install iptables on NAT Instance
```bash
sudo dnf install iptables -y
````

---

### 🔹 6. Configure NAT (MASQUERADE Rule)

```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

---

### 🔹 7. Enable IP Forwarding

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Make it permanent:

```bash
sudo nano /etc/sysctl.conf
net.ipv4.ip_forward = 1
```

---

### 🔹 8. Disable Source/Destination Check

* Go to EC2 console
* Select NAT Instance
* Actions → Networking → Change Source/Destination Check → Disable

 This step is mandatory for NAT functionality.

---

### 🔹 9. Configure Private Subnet Route Table

Update route table:

| Destination | Target          |
| ----------- | --------------- |
| 0.0.0.0/0   | NAT Instance ID |

---

### 🔹 10. Launch Private EC2 Instance

* Place in private subnet
* Do NOT assign public IP

---

## 🧪 Testing Internet Access

Login to private EC2 and run:

```bash
ping google.com
curl https://amazon.com
```

If responses are received → setup is successful 

---

## 🧠 Key Concepts Learned

* NAT Instance vs Internet Gateway usage
* Private subnet architecture in AWS
* Route table configuration
* IP forwarding & packet routing
* Source/Destination check importance

---

## Real-World Use Case

This architecture is widely used in:

* Secure backend systems
* Production VPC designs
* Microservices architectures
* Enterprise cloud environments

---

## 🏁 Conclusion

Successfully implemented a secure AWS network where private EC2 instances can access the internet through a NAT Instance without being exposed publicly.

---



