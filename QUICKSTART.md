# Quick Start Guide - N-Queen Visualiser Deployment

## 🎯 Choose Your Path

### Path 1: Test Locally (5 minutes)
```bash
# Build and run with Docker
docker-compose up -d

# Open browser
http://localhost:8080

# Stop when done
docker-compose down
```

### Path 2: Deploy to AWS (15 minutes)

#### Step 1: Prerequisites Check
```bash
# Check if tools are installed
docker --version
terraform --version
ansible --version
aws --version

# Configure AWS credentials
aws configure
```

#### Step 2: Generate SSH Key (if needed)
```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ""
```

#### Step 3: Deploy with Terraform
```bash
cd terraform
terraform init
terraform apply -auto-approve

# Save the output IP
export INSTANCE_IP=$(terraform output -raw instance_public_ip)
echo "Instance IP: $INSTANCE_IP"
```

#### Step 4: Update Ansible Inventory
```bash
cd ../ansible

# Replace <INSTANCE_IP> with your actual IP
cat > inventory.ini <<EOF
[webservers]
ec2-instance ansible_host=$INSTANCE_IP ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa

[webservers:vars]
ansible_python_interpreter=/usr/bin/python3
EOF
```

#### Step 5: Deploy Application
```bash
# Wait 30 seconds for instance to be ready
sleep 30

# Deploy with Ansible
ansible-playbook deploy.yml
```

#### Step 6: Access Your App
```bash
echo "Application URL: http://$INSTANCE_IP:8080"
# Open this URL in your browser
```

### Path 3: One-Command Deploy (Automated)
```bash
# Make executable
chmod +x deploy.sh

# Run and select option 2 or 3
./deploy.sh
```

## 🧪 Testing Your Deployment

### Local
```bash
curl http://localhost:8080
```

### AWS
```bash
curl http://<YOUR_EC2_IP>:8080
```

## 🧹 Clean Up

### Stop Local
```bash
docker-compose down
```

### Destroy AWS Resources
```bash
cd terraform
terraform destroy -auto-approve
```

## ⚠️ Common Issues

### Issue 1: AWS Credentials Not Found
```bash
aws configure
# Enter: Access Key ID, Secret Access Key, Region (us-east-1), Output (json)
```

### Issue 2: SSH Connection Refused
```bash
# Wait 60 seconds and try again
# EC2 instance needs time to boot
```

### Issue 3: Port 8080 Not Accessible
```bash
# Check security group in AWS Console
# Ensure port 8080 is open to 0.0.0.0/0
```

### Issue 4: Docker Build Fails
```bash
# Check if Docker daemon is running
docker info

# Restart Docker service
sudo systemctl restart docker  # Linux
# or restart Docker Desktop     # Windows/Mac
```

## 📊 Cost Estimate

- **EC2 t2.micro**: Free tier eligible (750 hours/month)
- **Storage**: ~$1/month for 20GB
- **Data Transfer**: Free tier includes 15GB/month

**Total**: $0-1/month (if within free tier)

## 🎓 What You Just Built

1. ✅ Dockerized web application
2. ✅ AWS VPC with public subnet
3. ✅ EC2 instance with Docker
4. ✅ Automated deployment pipeline
5. ✅ Infrastructure as Code (Terraform)
6. ✅ Configuration Management (Ansible)

## 📞 Need Help?

Check the full [DEPLOYMENT.md](DEPLOYMENT.md) for detailed troubleshooting.

---
**Enjoy your N-Queen Visualiser! 🎉**
