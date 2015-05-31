Team accounts on Flood IO can enjoy the benefits of Amazon's virtual private cloud (VPC) integration so that they can use the same scalable infrastructure inside a virtual network dedicated to their AWS account. It is logically isolated from other virtual networks in the AWS cloud which offers multiple layers of security, including security groups and network access control lists (ACL)

This is great for users wanting to host grid nodes in their own private subnets.

To use this feature simply select the **Enable VPC Integration** checkbox when creating your own hosted grid.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-11-14_13-51-07.jpg)

We will then auto discover available VPC identifiers, subnets, availability zones and security groups available within your hosted region.

## Setting up your VPC

If you haven't already, you will need to create your own VPC prior to creating a Grid with Flood IO.

We recommend using a [VPC with Public and Private Subnets.](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario2.html)

The configuration for this scenario includes a virtual private cloud (VPC) with a public subnet and a private subnet. We will then automatically create grid nodes in the private subnet, with a load balancer in the public subnet.

To get started, simply follow these instructions. Open your VPC dashboard in the [AWS console](https://console.aws.amazon.com/vpc/home) and click **Start VPC Wizard.**

Select the **VPC with Public and Private Subnets** configuration. This will create the two subnets, and grid nodes will be able to reach Flood IO for test control / results via Network Address Translation (NAT) in the public subnet. Please note, this will also create an m1.small instance for NAT. This eliminates the need to expose grid nodes on the public subnet and removes the hassle of needing elastic IPs for each of your grid nodes.

![](https://s3.amazonaws.com/flood-io-support/VPC_Management_Console_2014-11-14_14-03-46.jpg)

In the detailed review, please ensure that the availability zones for the public and private subnets are the same. This will ensure that grid nodes can be reached for test results via an elastic load balancer hosted in the public subnet.

![](https://s3.amazonaws.com/flood-io-support/VPC_Management_Console_2014-11-14_14-04-22.jpg)

Please note that Grid nodes will be created in your private subnet, as such they still need outbound access to Flood IO and related resources. If creating your own customised VPC in AWS please ensure that the Security -> Network ACL allows outbound access for your private subnet otherwise Grid nodes will fail to start.

![](https://s3.amazonaws.com/flood-io-support/VPC_Management_Console_2014-11-14_13-55-15.jpg)

We publish a [high level network diagram here](https://docs.google.com/drawings/d/1CQTt6EZOniH77nfj1mtr1Oi1aMDxvZ39VqyICG-mPdI/pub?w=960&h=720) which details the network connectivity required.

## What about a VPC with Public Subnets only?

AWS provide an [alternative scenario](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario1.html) using just a single public subnet, and an Internet gateway to enable communication over the Internet.

Unfortunately this configuration does not allow the Grid nodes hosted in the public subnet outbound connectivity to the Internet without installation of an additional NAT instance, or manual assignment of Elastic IPs to each node via your VPC console after the Grid has been created.

For this reason, we recommend using the the VPC wizard to set up a VPC with a NAT instance; for more information, see [Scenario 2: VPC with Public and Private Subnets](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario2.html) previously described. Otherwise, you can set up the NAT instance manually using the steps [detailed here](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_NAT_Instance.html).

That's all you need to get started with Amazon VPC and Flood IO integration. Enjoy!