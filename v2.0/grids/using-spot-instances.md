Customers with their own AWS account can choose to [host Grid nodes under their account](https://flood.io/blog/12-host-your-own-grid-nodes). This offers significant savings to customers as Grid nodes are then billed at cost price to the customer. Customers can also track allocation of spending via custom AWS billing tags.

Flood IO now includes the ability to Host Your Own grid nodes using [__Spot Instances__](http://aws.amazon.com/ec2/spot-instances/) from our [team](https://flood.io/pricing) plans, which passes on even more cost savings.

## Spot Instances

Spot Instances allow you to name your own price for Amazon EC2 computing capacity. You simply bid on spare Amazon EC2 instances and run them whenever your bid exceeds the current Spot Price, which varies in real-time based on supply and demand. The Spot Instance pricing model complements the On-Demand and Reserved Instance pricing models, providing potentially the most cost-effective option for obtaining compute capacity.

Spot Instances can significantly lower your computing costs for interruption-tolerant tasks. Spot prices are often significantly less than On-Demand prices for the same EC2 instance types. Now you can nominate a spot price for Host Your Own grid nodes when starting a Grid.

## Nominating a Price

Next time you Host Your Own, simply nominate a Spot Price. We'll automatically bid for and create your Grid using this price if provided. If you don't nominate a price we will just build the Grid using On-Demand instances.

We recommend using a price at or near the On-Demand price. You can [check Spot Instance pricing here](http://aws.amazon.com/ec2/spot-instances/#7). Spot Instances are subject to supply and demand, so you may lose nodes from your Grid whilst tests are running. Since we are built on a shared-nothing architecture, Grids are tolerant of this provided you have more than one node per Grid. Whenever a Grid drops below its nominated number of nodes, it may take time to provision a replacement however this process is automatic. Worst case you will need to restart your test.

We also allow you to nominate the instance type when hosting your own Grid for even greater capacity, although we recommend using m3.xlarge as the baseline.

![](https://flood.io/images/blog/using_spot_instances.gif)

We hope our team users enjoy the savings this option presents.