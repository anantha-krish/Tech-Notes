Install HTTP webserver on EC2

Install HTTP webserver on EC2

```
sudo su   -> Enter super user mode
yum update -y  -> Do a system update, -y flags accepts all installation
yum install httpd  -> install an apache server
systemctl start httpd  -> start apache server
systemctl enable httpd -> allow us to communicate with http server
echo "Hello World" > /var/www/html/index.html
 
//cmd to get details instance inside EC2
curl http://169.254.169.254/latest/meta-data ->//Important asked in exams the url
curl http://169.254.169.254/latest/meta-data/ami-id
curl http://169.254.169.254/latest/meta-data/hostname
curl http://169.254.169.254/latest/meta-data/instance-id
curl http://169.254.169.254/latest/meta-data/instance-type
 
curl http://169.254.169.254/latest/dynamic
curl http://169.254.169.254/latest/dynamic/instance-identity
curl http://169.254.169.254/latest/dynamic/instance-identity/document
 
```

## Get dynamic details of instance..like account id,region etc
```
[root@ip-172-31-33-79 ec2-user]# curl http://169.254.169.254/latest/dynamic/instance-identity/document
{
  "accountId" : "273700281035",
  "architecture" : "x86_64",
  "availabilityZone" : "ap-south-1a",
  "billingProducts" : null,
  "devpayProductCodes" : null,
  "marketplaceProductCodes" : null,
  "imageId" : "ami-09a7bbd08886aafdf",
  "instanceId" : "i-0eb53e4c47df6e83b",
  "instanceType" : "t2.micro",
  "kernelId" : null,
  "pendingTime" : "2020-09-13T07:10:55Z",
  "privateIp" : "172.31.33.79",
  "ramdiskId" : null,
  "region" : "ap-south-1",
  "version" : "2017-09-30"
}[root@ip-172-31-33-79 ec2-user]# 
```

## Modify the default Apache test page

pwd-> /home/ec2-user
The Apache server uses by default the location /var/www/html  to get html file similar to xampp
```
echo "Hello World" > /var/www/html/index.html
echo "Getting started with AWS $(hostname)" > /var/www/html/index.html
curl -s http://169.254.169.254/latest/dynamic/instance-identity/document > /var/www/html/index.html
```

By default if you ping Public IP from your machine, it gets timed out.
henc you need to add ICMP IPV4 in INbound rules of security grp.
