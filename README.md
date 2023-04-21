# AWS VPC Lattice deployment with a CloudFormation Template

## Table of Contents
1. [About this Repo](#About)
2. [Additional Examples](#AddEx)
3. [License](#License)

## About this Repo <a name="About"></a>
This repository contains an example of how use Vpc Lattice to Strangle your legacy application deployed in EC2. 
A lot of times in startup scenarios, we simply spin up an ec2 and deploy our application there. Until there's a clear scaling problem that needs to be addressed (and congratulations, that means your application is being used!) often times is a non trivial task to refactor your architecture and break your monolith in smaller chunks while reduce risk of downtime.

VPC Lattice is a great new service that not only allows a lot of the network complexitites to be abstrated away from your application integrations, but it also offers weighted routing, which can be used to strangle traffic from your monolith, while having the possibility to easily revert back traffic in case of issues start to be observed. 

This set of 3 cloudformation template are broken down by a ilustrative representation of the legacy system (cfn-legacy-product), new service running on Lambda (cfn-new-product) and a template which spin ups the most important resources to bare minimum, functional VPC lattice setup.

## Contraints

1) This example is built under the premise that a vpc (the traditional one) is already setup.
2) For demonstration purposes, the legacy-product template was generated so you could run these templates in your own account and see how it works. In a real world situation, the legacy environment has been already setup and you just have to feed in the details in the parameter section.
3) The VPC this has been developed had 172.x.x.x CIDR range, please modify the parameter according to your own VPC configuration.

### Official Resources
- [Cloud Formation references](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_VpcLattice.html)
- [Strangler original Martin Fowler's article](https://martinfowler.com/bliki/StranglerFigApplication.html)



# License <a name="License"></a>

This library is licensed under the Apache 2.0 License.

# FAQ

Q:I am not being able to hit the service domain from my test environment. Why?
If you are hitting the domain from your VPC, most likely you have to allow inbound traffic from the resource you are doing it. If it is an EC2 box, add a new inbound rule refering its security group in the security group tied to the VPC association within the VPC lattice service network.