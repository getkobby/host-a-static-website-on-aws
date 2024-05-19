![Alt text](/Host_a_Static_Website_on_AWS.png)
Overview
1. Amazon S3 (Simple Storage Service):
    * Create a bucket:
        * Sign in to the AWS Management Console and open the Amazon S3 console1.
        * Choose Create bucket.
        * Enter a Bucket name (e.g., example.com).
        * Choose the Region where you want to create the bucket (geographically close to you for latency and cost optimization).
        * Accept the default settings and create the bucket.
    * Enable static website hosting:
        * After creating the bucket, enable static website hosting for it.
        * Sign in to the AWS Management Console and open the Amazon S3 console.
        * Select your bucket.
        * Choose the Properties tab, and then choose Static website hosting.
        * Specify an index document (e.g., index.html) and an optional error document.
        * Save the changes.
    * Edit Block Public Access settings:
        * Disabling Block Public Access is required for hosting a static website, but we recommend keeping it enabled for security.
    * Add a bucket policy:
        * Create a bucket policy that makes your bucket content publicly available.
        * Configure the policy to allow s3:GetObject permissions for all users.
    * Configure an index document:
        * Specify the default document to serve when users access your website.
    * Test your website endpoint:
        * Access the website using the Amazon S3 website endpoint.
    * Clean up:
        * Remove any unnecessary resources or configurations.
2. Amazon EC2 (Elastic Compute Cloud):
    * Launch an EC2 instance (e.g., t2.micro) based on your website’s requirements.
    * Install Apache HTTP Server on the EC2 instance.
    * Clone your project’s GitHub repository to the instance.
    * Configure Apache to serve the web content from the cloned repository.
    * Enable Apache to start automatically at system boot.
    * Start the Apache HTTP Server to serve your static website.
3. Domain and DNS:
    * Register a domain name using Route 53.
    * Set up a DNS record to point your domain to the Amazon S3 website endpoint or EC2 instance’s public IP address.
4. Security:
    * Use Security Groups to control inbound and outbound traffic to your EC2 instance.
    * Configure HTTPS using Amazon Certificate Manager for secure communication.
5. Scaling and Availability:
    * Use an Auto Scaling Group to manage EC2 instances for scalability, fault tolerance, and elasticity.
    * Distribute web traffic using an Application Load Balancer across multiple Availability Zones.
  
    * Deployment Script
    
* #!/bin/bash

# Switch to the root user to gain full administrative privileges
sudo su

# Update all installed packages to their latest versions
yum update -y

# Install Apache HTTP Server
yum install -y httpd

# Change the current working directory to the Apache web root
cd /var/www/html

# Install Git
yum install git -y

# Clone the project GitHub repository to the current directory
git clone https://github.com/getkobby/host-a-static-website-on-aws.git

# Copy all files, including hidden ones, from the cloned repository to the Apache web root
cp -R host-a-static-website-on-aws/. /var/www/html/

# Remove the cloned repository directory to clean up unnecessary files
rm -rf host-a-static-website-on-aws

# Enable the Apache HTTP Server to start automatically at system boot
systemctl enable httpd 

# Start the Apache HTTP Server to serve web content
systemctl start httpd
