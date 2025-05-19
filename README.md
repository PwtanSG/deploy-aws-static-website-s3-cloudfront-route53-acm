# The project
This project demonstrates how AWS services like S3, CloudFront, and Route 53 can be used to host a static website. A simple AWS cloudformation template is used to deploy AWS infrastructure required by the AWS services. 

## App
This is a simple frontend

## third party domain purchase and setup
Your will need to configure the DNS servername at the domain provider's setup to point to the NS of Route53 NS. <br>
For Godaddy domain, login and goto DNS Management tab and change the nameservers to Route 53 hosted zone 4 NS provided.