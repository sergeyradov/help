When hosting your own Grid you may encounter errors the first time you are setting up a Grid for you account. Since we are starting Grids on your own AWS account, the information behind failures is limited to us. You should see a "failed" status in your Grid dashboard

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-09_09-44-34.jpg)

Your best option for root cause analysis is to open the corresponding Grid which has been created in your AWS console via the "Cloud Formation" tab. Events for that Grid should then reveal root cause

![](https://s3.amazonaws.com/flood-io-support/CloudFormation_Management_Console_2014-10-09_09-46-25.jpg)

### EC2 Classic or Default VPC

In the example above we can see that the Grid failed around creation of the ElasticLoadBalancer, in particular, security groups could not be applied to an ELB which does not sit in a VPC. The fix for this particular issue is to choose the "EC2 Classic" platform option when creating the Grid

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-10-09_09-50-06.jpg)

### IAM Roles

Often Grid creation problems exist around incorrect IAM role settings. Please ensure the IAM roles and policies are setup in accordance with [these instructions](/blog/12-host-your-own-grid-nodes).

If you are still stuck creating your own hosted Grid please contact support for assistance.