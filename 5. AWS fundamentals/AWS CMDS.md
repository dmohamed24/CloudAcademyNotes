### SSH into an ec2 instance 
key value pair `.prem` needs to be located in the same folder that you execute the cmd in
```
ssh -i <key_vale_file_name> ec2-user@<ec2_private_ip>
```

### SSH into an private ec2 instance from a public ec2 instance 
Add the key key pair in the same directory or copy over an existing `.pem` file using this cmd 
```
scp -i <.pem existing file name> <new file name .pem ec2-user@?<existing ec2 public ip address>:~/
``` 


```
aws cloudformation describe-stacks --stack-name my-s3-bucket-stack

aws cloudformation create-stack --stack-name my-s3-bucket-stack --template-body file://s3-bucket.json

aws cloudformation update-stack --stack-name my-s3-bucket-stack --template-body file://s3-bucket.json

aws cloudformation delete-stack --stack-name my-s3-bucket-stack

s3 cp index.html s3://my-s3-bucket-dm  upload html file to s3 bucket


#!bin/bash
# User data for new EC2 instances
# install httpd (Linux 2 version)
yum update -y
yum install -y httpd
systemctl start httpd
system enable httpd
echo "<h1>HELLO WORLD!!!!! ${hostname -f}</h1>" > /var/www/html/index/html
```