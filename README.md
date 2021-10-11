# aws_developer_task_4
Create High Availability Architecture with AWS CLI 
The architecture aim-
1. Webserver configured on EC2 Instance
2. Document Root(/var/www/html) made persistent by mounting on EBS Block Device.
3. Static objects used in code such as pictures stored in S3
4. Setting up Content Delivery Network using CloudFront and using the origin domain as S3 bucket.
5. Finally place the Cloud Front URL on the webapp code for security and low latency.


To begging off with first of all we need to launch an AWS instance using AWS CLI, for us to launch an instance we need to specify the subnet where we want to launch our instance, security group id and a key name as mentioned below.

1. aws ec2 run-instances --image-id ami-081bb417559035fe8 --instance-type t2.micro --count 1 --subnet-id subnet-d7caf0bf --security-group-ids sg-09cb39dbe7c04b03a --key-name mykey

Next, we will have to create an EBS volume where we will store the data that will be displayed on the web app and attach it to the instance created earlier.

1. aws ec2 create-volume --availability-zone ap-south-1a --no-encrypted --size 8 --volume-type gp2

2. aws ec2 attach-volume --volume-id vol-019716e389af44903 --instance-id i-03aefd0097ed0ed37  --device /dev/sdh
Now, that we have created a volume we have to create a S3 bucket and upload our image in the bucket created which is done as follows.

1. aws s3 mb s3://yams2000--region ap-south-1
2. aws s3 cp IMAGE1.png s3://yams200

The next step is to use the S3 bucket link as an origin and create a CloudFrount in AWS that is a content Delivery Network. since we want to launch the content stored in S3 we will give the origin name as the s3 bucket name as follows.

1. aws cloudfront create-distribution --origin-domain-name yams200.s3.ap-south-1.amazonaws.com --default-root-object web.html

Now, that our image is stores and linked to the CloudFront we have to now login to our AWS instance and install httpd software and host a website that can display the content coming from CloudFront.

The next step is to mount the Volume created with /var/www/html so that our content becomes permanent even after the instance gets terminated and hence reduce the latency

1. ssh -l ec2-user -i MyKey2.pem <​IP of instance>
2. sudo yum install httpd
3. fdisk -l
4. mount /dev/sdh /var/www/html
5. cd /var/www/html
6. vi web.html

The http code to display the image stored on S3 bucket onto the web app.

Now, our  complete architecture is created and our website is hosted using CloudFrount and can be connected using public IP of our instance that was created earlier.

And finally, all the objectives of the task have been successfully completed.

Thank you .....happy learning!
