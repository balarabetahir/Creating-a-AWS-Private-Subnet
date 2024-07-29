# Creating-a-AWS-Private-Subnet
🚷 Create a private subnet.  🚧 Create a private route table.  🚔 Create a private network ACL.

Ever wondered how to keep your AWS resources safe from the public internet? Here's a solution: create a private subnet that keeps your resources isolated and secure. 🔒 In this article, we'll delve into the steps to create a private subnet within an Amazon VPC (Virtual Private Cloud) and explore how this setup adheres to security best practices, including the principle of least privilege.

Understanding the Basics
What is a Private Subnet?
A private subnet is a segment of a VPC that is not directly accessible from the public internet. Resources within a private subnet can communicate with each other and other subnets in the VPC, but cannot be reached from outside unless specific configurations are made (e.g., using a NAT gateway or VPN).

Why Use a Private Subnet?
Enhanced Security: By isolating resources from the public internet, you significantly reduce the attack surface.
Controlled Access: Only specific services and users can access resources in the private subnet, adhering to the principle of least privilege.
Compliance: Helps meet regulatory requirements by keeping sensitive data and applications isolated.
Step-by-Step Guide to Creating a Private Subnet
Here's how I did it:

1. Create a Private Subnet in Amazon VPC
First, navigate to the VPC Dashboard in your AWS Management Console and follow these steps:

Create a VPC: If you haven't already, create a VPC. Provide a CIDR block, for example, 10.0.0.0/16.
Create a Subnet:
Go to the Subnets section.
Click Create Subnet.
Choose your VPC, provide a name, and specify a CIDR block, such as 10.0.1.0/24. Ensure this CIDR block does not overlap with any existing subnets.
2. Configure Route Table
By default, subnets use the main route table of the VPC, which may include routes to the internet gateway. To ensure your private subnet does not have internet access:

Create a Custom Route Table:
Go to the Route Tables section.
Click Create Route Table and associate it with your VPC.
Edit Routes:
Select your new route table.
Click on Routes and ensure there are no routes to an internet gateway.
Associate Route Table with Subnet:
Go to the Subnet Associations tab.
Click Edit Subnet Associations and select your private subnet.
3. Network ACL Configuration
Network Access Control Lists (ACLs) act as a firewall for controlling traffic in and out of subnets. To lock down your private subnet:

Create a Network ACL:
Go to the Network ACLs section.
Click Create Network ACL and associate it with your VPC.
Modify Network ACL Rules:
By default, a new ACL denies all traffic. If this suits your security requirements, you can leave it as is. Otherwise, customize inbound and outbound rules to fit your specific needs.
For example, to deny all traffic:
Inbound Rules: Rule Number 100, Type All Traffic, Source 0.0.0.0/0, Deny.
Outbound Rules: Rule Number 100, Type All Traffic, Destination 0.0.0.0/0, Deny.
Associate Network ACL with Subnet:
Go to the Subnet Associations tab.
Click Edit Subnet Associations and select your private subnet.
4. Security Group Configuration
Security Groups (SGs) act as virtual firewalls for your instances. Unlike network ACLs, security groups are stateful, meaning if you allow an incoming request, the response is automatically allowed.

Create Security Groups:
Go to the Security Groups section.
Click Create Security Group and associate it with your VPC.
Set Rules According to Least Privilege:
For instances within the private subnet, only allow necessary traffic. For example:
Allow SSH access only from a specific IP range within your VPC.
Allow database traffic only from specific application servers.
Example rules:
Inbound Rule: Type SSH, Protocol TCP, Port 22, Source 10.0.2.0/24 (if 10.0.2.0/24 is your application subnet).
Outbound Rule: As needed by your applications, but generally allow minimal necessary outbound traffic.

Principle of Least Privilege
The principle of least privilege (PoLP) is a key security concept that entails giving users and resources the minimum level of access necessary to perform their functions. Applying PoLP in the context of a private subnet involves:
Restricted Network Access: By default, deny all traffic and explicitly allow only necessary connections.
Granular Security Group Rules: Tailor security group rules to allow minimal required access.
Role-Based Access Control: Use IAM roles and policies to limit who can modify network configurations and access resources within the private subnet.

Benefits of a Private Subnet with PoLP
Reduced Attack Surface: Isolating resources from the public internet limits exposure to potential threats.
Enhanced Compliance: Facilitates adherence to industry regulations by controlling access to sensitive data.
Operational Efficiency: Prevents unauthorized access and potential misconfigurations by adhering to PoLP.
Improved Security Posture: By default, deny-all configuration ensures only explicitly allowed traffic can reach your resources.

Conclusion
Creating a private subnet in AWS and configuring it according to the principle of least privilege is a robust strategy to enhance the security of your cloud infrastructure. By isolating sensitive resources from the public internet and meticulously controlling access, you can safeguard your applications and data against a wide array of threats. Follow the steps outlined above to set up a private subnet, configure route tables, network ACLs, and security groups, and you will significantly strengthen your AWS security posture.
