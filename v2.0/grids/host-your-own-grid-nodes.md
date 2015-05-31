Customers with their own AWS account can choose to host Grid nodes under their account. This offers significant savings to customers as Grid nodes are then billed at cost price to the customer. Customers can also track allocation of spending via custom AWS billing tags. Host Your Own grid nodes is available to our [team](https://flood.io/pricing) plans.

## Creating an AWS Account

Amazon Web Services (AWS) delivers a set of services that together form a reliable, scalable, and inexpensive computing platform “in the cloud”.

To access any web service AWS offers, you must first create an AWS account at [aws.amazon.com](http://aws.amazon.com). An AWS account is simply an Amazon.com account that is enabled to use AWS products; you can use an existing Amazon.com account login and password when creating the AWS account.

## Creating an AWS IAM User

You will need to create an IAM user specifically for Flood IO to allow us to create AWS resources on your behalf. Create an AWS IAM User in your [console](https://console.aws.amazon.com/iam/home#users)

### Step 1

Create New Users and enter a user name, we recommend something like `flood_io`. Ensure you generate an access key for each User.

![](https://s3.amazonaws.com/flood-io-support/IAM_Management_Console_2015-04-15_14-31-26.jpg)

### Step 2

Show User Security Credentials and take note of the Access Key ID and Secret Access Key. You will need to update your Flood IO Account settings with these details.

![](https://s3.amazonaws.com/flood-io-support/IAM_Management_Console_2015-04-15_14-32-32.jpg)

### Step 3

Create a new policy in your AWS IAM Policies [console](https://console.aws.amazon.com/iam/home#policies)

![](https://s3.amazonaws.com/flood-io-support/IAM_Management_Console_2015-04-15_14-51-12.jpg)

In the Review Policy, customise the policy document with the following:

Policy name: `FloodIO`

Policy document:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "FloodIO",
      "Action": [
        "ec2:*",
        "cloudformation:*",
        "elasticloadbalancing:*",
        "autoscaling:*",
        "iam:AddRoleToInstanceProfile",
        "iam:PassRole"
      ],
      "Effect": "Allow",
      "Resource": [
        "*"
      ]
    }
  ]
}
```

![](https://s3.amazonaws.com/flood-io-support/IAM_Management_Console_2015-04-15_14-51-12.jpg)

### Step 4

Attach the `FloodIO` policy to your `flood_io` user in your AWS IAM Users [console](https://console.aws.amazon.com/iam/home#users)

![](https://s3.amazonaws.com/flood-io-support/IAM_Management_Console_2015-04-15_14-53-28.jpg)

![](https://s3.amazonaws.com/flood-io-support/IAM_Management_Console_2015-04-15_14-54-38.jpg)

The custom user policy `FloodIO` should now be attached.

![](https://s3.amazonaws.com/flood-io-support/IAM_Management_Console_2015-04-15_14-55-39.jpg)

## Creating an AWS IAM Role

You will also need to create an AWS IAM Role in your [console](https://console.aws.amazon.com/iam/home#roles). This role is associated with the IAM instance profile of each grid node you launched and is useful for controlling what permissions grid nodes have for AWS resources.

### Step 1

Create a new IAM role with the Role Name `flood-node`

![](https://s3.amazonaws.com/flood-io-support/IAM_Management_Console_2015-04-15_14-56-29.jpg)

### Step 2

Select the `Amazon EC2` for Role Type

![](https://s3.amazonaws.com/flood-io-support/IAM_Management_Console_2015-04-15_14-57-10.jpg)

### Step 3

Attach the previously created policy called `FloodIO`

![](https://s3.amazonaws.com/flood-io-support/IAM_Management_Console_2015-04-15_14-58-18.jpg)

### Step 4

Review and Create the new Role

![](https://s3.amazonaws.com/flood-io-support/IAM_Management_Console_2015-04-15_14-59-18.jpg)

### Step 5

Review and Create the new Role

![](https://s3.amazonaws.com/flood-io-support/iam_5.png_2014-11-28_11-19-34.jpg)

## Host Your Own

Make sure you update your Flood IO account settings with the AWS Account details of the user you just created.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-11-28_11-21-30.jpg)

Now whenever you start the next Grid, you will be able to select "Host Your Own" as the infrastructure option which will launch the nodes under your own AWS account.

![](https://s3.amazonaws.com/flood-io-support/Flood_IO_2014-11-28_11-20-41.jpg)