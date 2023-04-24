# AWS VPC Lattice deployment with a CloudFormation Template

## Table of Contents
1. [About this Repo](#About)
2. [Additional Examples](#AddEx)
3. [License](#License)

## About this Repo <a name="About"></a>
This repository contains an example of how use Vpc Lattice to Strangle your legacy application deployed in EC2. 
A lot of times throughout startup journey, there is a clear requirement for speed and experimentation. Once the feasibility, usage and MVP are proven, engineering teams will pivot to scaling goals. often times is a non trivial task to refactor your architecture and break your monolith in smaller chunks while reducing risk of downtime.

VPC Lattice is a great new service that not only allows a lot of the network complexitites to be abstrated away from your application integrations, but it also offers weighted routing, which can be used to strangle traffic from your monolith, while having the possibility to easily revert back traffic in case of issues start to be observed. 

This set of 3 cloudformation templates are broken down by a ilustrative representation of the legacy system (cfn-legacy-product), new service running on Lambda (cfn-new-product) and a template which spin ups the most important resources to bare minimum, functional VPC lattice setup.

![Architecture](./lattice-strangler.png "Architecture")

## Contraints

1) This example is built under the premise that a vpc (the traditional one) is already setup.
2) For demonstration purposes, the legacy-product template was generated so you could run these templates in your own account and see how it works. In a real world situation, the legacy environment has been already setup and you just have to feed in the details in the parameter section.
3) The VPC this has been developed had 172.x.x.x CIDR range, please modify the parameter according to your own VPC configuration.
4) This architecture will not work with stateful applications. If your application requires stateful requests, consider using [ALB] (https://docs.aws.amazon.com/elasticloadbalancing/latest/application/sticky-sessions.html). 

### Official Resources
- [Cloud Formation references](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_VpcLattice.html)
- [Strangler original Martin Fowler's article](https://martinfowler.com/bliki/StranglerFigApplication.html)

## How to deploy the CFN Templates

aws cloudformation deploy --template-file cfn-new-product.yaml --stack-name new-product-stack --capabilities CAPABILITY_NAMED_IAM

aws cloudformation deploy --template-file cfn-legacy-product.yaml --stack-name product-legacy --parameter-overrides "$(cat param-legacy-product.json)"

aws cloudformation deploy --template-file cfn-lattice-basic.yaml --stack-name vpc-lattice-stack --parameter-overrides "$(cat param-lattice.json)"   

# License <a name="License"></a>

This library is licensed under the Apache 2.0 License.

# FAQ

Q:I am not being able to hit the service domain from my test environment. Why?

If you are hitting the domain from your VPC, most likely you have to allow inbound traffic from the resource you are doing it. If it is an EC2 box, add a new inbound rule refering its security group in the security group tied to the VPC association within the VPC lattice service network.
