# CFN-Template-Kitchen-Sink

## Description

This is a big ass CFN template that I have created when working with customers using AWS services.  The purpose of this template was to allow me to reproduce a customers issues and get me 80 - 90% of the way in one push of a button, no matter what their setup was.  

I'm releasing this publicly in hopes that it can be a sample project that helps others, or maybe just shows some example resources that others can use to build their own templates.

## Overview

This template sets up more resources that you probably actually need, but it's a good catch all that will get you 80 - 90% of the way towards what you need.  After the stack completes, you can tweak the resources in the console or CLI.  

Here is an overview of what the template creates:

- Two VPCs, one called 'Provider VPC' and another called 'Consumer VPC'.  This is in reference to the VPC PrivateLink terms for a VPC Endpoint Service.
  - Ironically, this template does not create any PrivateLink resources, because at the time of creation, these were not supported in CFN
- Each VPC is setup for two AZs, and provides a public and private subnet for each AZ
  - An EC2 instance is created in each subnet, each with UserData that configures the following:
    - Installs Apache and the CloudWatch Logs agent
    - Configures support in Apache for ProxyProtocol
    - Downloads an SSL certificate and key (which you need to add to the template)
    - Configures Apache to listen on 3 ports: 80, 443, 8080
    - Creates an awesome default page
    - starts apache and cw logs agent
- One of each type of Load Balancer (Classic, Application, Network)
    - Configures 3 listeners: 80, 443, 8080
    - registers all public and private instances
- Configures a VPC Peering Connection between the two VPCs

## Visual

![visual-overview][visual-overview]

## How to use

- Upload all files to an S3 bucket
- Create the stack referencing the 'CFN-Template-Kitchen-Sink-root.yaml' file
- In the parameters, reference the bucket name and key that the files are stored in.  It will create multiple nested stacks using these params

## Conclusion

Feel free to fork this project, submit a PR, or simply download and use.  However, I don't believe I will continue improving this template.  It currently is working, but won't be seeing any love moving forward


[visual-overview]: CFN-Template-Kitchen-Sink.png
