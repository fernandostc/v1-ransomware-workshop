---
title: "Lab Preparation"
chapter: false
weight: 30
pre: "<b>3. </b>"
---
## Lab Preparation Guide

### Lab Architecture
To simulate a real customer environment, we will use a CloudFormation template that will create the following resources:

- 1 VPC
    - 1 Windows Instance (Target Computer)
    - 1 Linux Instance (Attacker Computer - Mitre Caldera)

With this two components, you will be able to demonstrate how to detect and respond to attacks using the Vision One XDR Agent only. 

The Mitre Caldera is our attacking tool. It simulates a Command and Control Server, that will be used to create malicious payloads and execute malicious instructios on the affected computers.

---
### Enabling the Vision One XDR Agent (Automatic Activation)
The following instructions will guide you on how to enable the Vision One XDR Agent automatic activation.

{{< youtube id="X4Js_Tr61PA" >}}

---
### Creating the base AWS environment using AWS CloudFormation Template
Using your AWS Account, deploy the following Cloud Formation Template and follow the instructions described on the video. 

<a href="https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=V1-Ransomware-Workshop&templateURL=https://v1demoplatform.s3.us-west-1.amazonaws.com/v1-ransomware-demo/v1-ransomware-workshop.yaml" target="_blank"><img src="https://cdn.rawgit.com/buildkite/cloudformation-launch-stack-button-svg/master/launch-stack.svg" /></a>

{{< youtube id="9EnIrsCRvMI" >}}

