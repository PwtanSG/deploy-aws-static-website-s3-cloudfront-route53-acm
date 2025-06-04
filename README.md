# The project
This project demonstrates how AWS services like S3, CloudFront and Route 53 can be used to host a static website with a SSL cert and a custom domain (Godaddy purchased domain). A simple AWS cloudformation template is used to deploy AWS infrastructure required by the AWS services. 

- CloudFront caches content in edge locations globally, reducing latency for users.
- S3: S3 bucket access can be restricted to CloudFront, preventing direct public access to S3 bucket.
- Route 53: manage Custom Domain allows access of site content via custom domain instead of using an S3 bucket or cloudfront URL.
- Certification Manager : HTTPS support using CloudFront to provide SSL/TLS certificates via ACM, securing website traffic.

## App
This is a simple frontend. index.html

## third party domain purchase and setup
Your will need to configure the DNS servername at the domain provider's setup to point to the NS of Route53 NS. <br>
For Godaddy domain, login and goto DNS Management tab and change the nameservers to Route 53 hosted zone 4 NS provided.

## infrasturtures using cloudformation templates 
### 1. infrastructure\CF-s3-cloudfront-route53-template.yaml
- execute using AWS CLI create stack
- S3 : Create 2 private S3 buckets to host static site files
- Cloudfront : create cloudfront distributions to point to S3 buckets
- Route 53 : HostedZone, A Records point to cloudfront 
### 2. infrastructure\CF-acm-cert-template.yaml
- execute using AWS CLI create stack
- ACM : create SSL cert
<br> Note : ACM cert creation is on a separate as the cert require the DNS validation is dependent on CName to create record in HostedZone before cert can be issued.

## AWS management console configurations (manually)
- ACM : AWS console ACM click create CNames records in Route 53 HostedZone Records inorder to complete ACM cert creation DNS validation to complete.
- Cloudfront : Once cert is issued, Goto AWS console Cloudfront distribution - Edit settings configure CName and SSL cert.
configure Alternate domain name (CNAME) e.g domain.com /www.domain.com and Custom SSL certificate select cert issued

## Add AWS Route 53 HostedZone NS to domain provider DNS record
- Goto AWS Route 53 HostedZone NS (4 NS provided)
- Add the 4 NS provided to the domain provider(e.g godaddy) DNS records.

# Deploy static files to S3 bucket
- copy html file to S3 buckets : 
- aws s3 cp index.html s3://<www-bucket-name>