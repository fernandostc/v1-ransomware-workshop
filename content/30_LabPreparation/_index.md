---
title: "Lab Preparation Guide"
chapter: false
weight: 30
pre: "<b>3. </b>"
---
### Lab Architecture
We will use Cloud Formation Templates to create all the components used in this demo.

Components created by this CFT.
- 1 VPC
    - 1 Windows Instance (Target-1 Computer)
    - 1 Windows Instance (Target-2 Computer)
    - 1 Linux Instance (Attacker Computer - Mitre Caldera)

With these three components, you will be able to demonstrate how to detect and respond to attacks using the Vision One XDR Agent only.

Note: The Mitre Caldera is the attacking tool used in this demo. It simulates a Command and Control Server that will be used to create malicious payloads and execute malicious instructions on the affected computers.

---
### Enabling the Vision One XDR Agent (Automatic Activation)
The following instructions will guide you in enabling the automatic activation of the Vision One XDR Agent.

<b>Instructions:</b>
{{< youtube id="X4Js_Tr61PA" >}}

---
### Creating the base AWS environment using AWS CloudFormation Template
Deploy the following Cloud Formation Template and follow the instructions described in the video. 

<a href="https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=V1-Ransomware-Workshop&templateURL=https://v1demoplatform.s3.us-west-1.amazonaws.com/v1-ransomware-demo/v1-ransomware-workshop.yaml" target="_blank"><img src="https://cdn.rawgit.com/buildkite/cloudformation-launch-stack-button-svg/master/launch-stack.svg" /></a>

<b>Instructions:</b>
{{< youtube id="9EnIrsCRvMI" >}}

