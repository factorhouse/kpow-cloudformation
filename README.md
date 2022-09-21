# Run Kpow for Apache Kafka in Amazon ECS

This quick start provides help getting Kpow up and running in Amazon ECS in minutes.

Kpow is the perfect companion to [Amazon MSK](https://aws.amazon.com/msk/) and is easily configured to run in Amazon ECS.

See our [documentation](https://docs.kpow.io/) for full installation options and licensing information.

See the [AWS Marketplace Guide](https://docs.kpow.io/installation/aws-marketplace) for information on the IAM permissions required by Kpow.

## Deploying to ECS or EKS.

Kpow is a great fit for ECS or EKS because it a **single docker container** with **zero dependencies**.

Kpow has a suggested allocation of **2GB heap** making it ideal for provisioning as a FARGATE task.

Kpow connects to your Kafka cluster with exactly the same configuration as a Kafka Producer or Consumer.

This quick start provides CloudFormation scripts to run Kpow in ECS, for EKS support [Kpow Helm Charts](https://github.com/operatr-io/kpow-helm-charts).

## Licensing Options

The Kpow container is available in two places:

* [Dockerhub](https://hub.docker.com/r/operatr/kpow) requires a license, [start a free 30-day trial](https://kpow.io/try) today.
* [AWS Marketplace](https://aws.amazon.com/marketplace/seller-profile?id=ab356f1d-3394-4523-b5d4-b339e3cca9e0&ref=dtl_B084BTWJHD) is automatically licensed, simply click subscribe and pay via your AWS bill.

## CloudFormation

This repository contains configuration for a Kpow Cloudformation Stack.

* **kpow-dockerhub.yaml** includes license parameters
* **kpow-aws-marketplace.yaml** includes the AWSMarketplaceMetering/registerUsage IAM policy

This configuration is provided as a quick-start demonstration of simple provisioning and configuration options, and is for example purposes only. You may extend it however you need to fit your own purposes.

This configuration defines the following resources:

* **AWS::ECS::TaskDefinition** to run your Kpow container.
* **AWS::ECS::Service** containing the Task, provisioned within a Subnet designated by you.
* **AWS::EC2::SecurityGroup** with permissive egress to ECR/Kafka and ingress on the UI port.
* **AWS::IAM::Role** providing IAM actions to ECS, Logs, and Marketplace:RegisterUsage (if applicable).

In order for Kpow to run correctly you may need to ***alter your Kafka Cluster Security Group to allow ingress on the port of the bootstrap URL*** (eg. 9092, 9094 for MSK w/ SSL connections, etc).

## Authentication & Encryption

Kpow supports all standard Kafka client authentication options, configured via environment variables.

The Cloudformation scripts provided here support PLAIN and SASL connections.

## Known Issues

If stack creation fails with a 'CannotPullContainer' error please either:

* Select the 'Auto-Assign-Public-IP' deployment option; or,
* Ensure the subenet you choose to deploy Kpow into has access to a NAT.

This is required to pull your Kpow subscription container image, see [here for more information](https://github.com/aws/amazon-ecs-agent/issues/1128).

