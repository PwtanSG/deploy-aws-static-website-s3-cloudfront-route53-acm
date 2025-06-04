# The project
This project demonstrates how AWS services like S3, CloudFront and Route 53 can be used to host a static website with a SSL cert and a custom domain (Godaddy purchased domain). A simple AWS cloudformation template is used to deploy AWS infrastructure required by the AWS services. 

- CloudFront caches content in edge locations globally, reducing latency for users.
- S3 : S3 bucket is use to host the application code/files. Access can be restricted to CloudFront, preventing direct public access to S3 bucket.
- Route 53: manage Custom Domain and access of site content via custom domain instead of using an S3 bucket or cloudfront URL.
- Certification Manager : HTTPS support using CloudFront to provide SSL/TLS certificates via ACM, securing website traffic.

## App
This is a simple frontend. index.html

## Third party domain purchase and setup
Your will need to configure the DNS servername at the domain provider's setup to point to the NS of Route53 NS. <br>
For Godaddy domain, login and goto DNS Management tab and change the nameservers to Route 53 hosted zone 4 NS provided.

## cloudformation templates to create aws infrastructures
### 1. infrastructure\CF-s3-cloudfront-route53-acm-template.yaml
- execute using AWS CLI create stack
- S3 : Create private S3 buckets to host static site files, permission bucket policy to allow only cloudfront arn to access.
- Cloudfront : create cloudfront distributions to point to S3 buckets using OAC(origin)
- Route 53 : HostedZone, A Records point to cloudfront 
- ACM : create SSL cert

## Configurations required by ACM to approve and issue cert 
Important : Once cert creation is activated and Route53 hostedzone is created, 
### 1. Add AWS Route 53 HostedZone NS to domain provider DNS record
- Goto AWS Route 53 HostedZone NS (4 NS provided)
- Add the 4 NS provided to the domain provider(e.g godaddy) DNS records.

### 2.AWS management console configurations
- Create CNames records in Route53 hostedzone - in AWS console ACM click create CNames records in Route 53 HostedZone Records inorder to complete ACM cert creation DNS validation to complete. Check that CNames added in HostedZone recordsets.
Note: Cloudfront : Once cert is issued, Check Cloudfront distribution - Edit settings configure CName and SSL cert.
Alternate domain name (CNAME) e.g domain.com &/or www.domain.com and Custom SSL certificate selected cert issued

<br> ACM approval may takes a while ~30mins

## Deploy static files to S3 bucket
- copy html file to S3 buckets : 
- aws s3 cp index.html s3://bucket-name
- aws s3 cp build s3://bucket-name/ --recursive