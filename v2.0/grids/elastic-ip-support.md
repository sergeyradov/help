We often get asked, is _how can we predict the IP address allocation of flood nodes?_

For On Demand nodes, _we can't_. They are essentially ephemeral machines and it is not practical for us to allocate static addresses to such short lived resources.

The most common driver for requesting static IPs are the firewalls sitting in front of production and/or development/test environments, effectively blocking flood nodes from hitting them with load.

Mitigations to this range from loose, through to tight security options.

Flood IO [already supports VPC integration](https://flood.io/blog/7-amazon-vpc-integration) for _tight_ control of your network, including deployment of flood nodes in your private subnet.

We now offer an __alternative option through the use of Elastic IPs__ for our team plans.

If you Host Your Own flood nodes, you can pre-allocate Elastic IPs within your AWS account. Elastic IP addresses are static IP addresses designed for dynamic cloud computing. An Elastic IP address is associated with your account, not a particular instance, and you control that address until you choose to explicitly release it. This is a great option for punching holes in your firewall.

Mapping those pre-allocated addresses is as simple as selecting them from the relevant drop down menu when creating your next hosted grid.

![](https://flood.io/images/blog/elastic_ip_association.gif)

Flood nodes will then associate themselves with the IP addresses you have made available. __Leaving you free to stop and start your hosted grids as you wish.__

Other __less secure__ options you might consider are protecting your site with some form of basic authentication, or if you're feeling really _loose_, open up your firewall to the public IP range [published by AWS](https://forums.aws.amazon.com/ann.jspa?annID=1701)