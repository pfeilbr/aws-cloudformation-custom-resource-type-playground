# aws-cloudformation-custom-resource-type-playground

examples for creating CloudFormation extensions and specifically [CloudFormation Custom Resource Type](https://docs.aws.amazon.com/cloudformation-cli/latest/userguide/what-is-cloudformation-cli.html)

> An extension is an artifact, registered in the CloudFormation Registry, which augments the functionality of CloudFormation in a native manner

> You can use the CloudFormation CLI to register extensions—both those you create yourself, as well as ones shared with you—with the CloudFormation registry. This enables you to use CloudFormation capabilities to create, provision, and manage these custom types in a safe and repeatable manner, just as you would any AWS resource

There are the following four types of CloudFormation extension mechanisms:

* [CloudFormation Custom Resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources.html)
* [CloudFormation Module](https://aws.amazon.com/blogs/mt/introducing-aws-cloudformation-modules/)
* [CloudFormation Custom Resource Types](https://docs.aws.amazon.com/cloudformation-cli/latest/userguide/resource-types.html)
* [CloudFormation Macros](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-macros.html)

see [`python-custom-resource-type-example`](python-custom-resource-type-example)

## Notes

* [Custom resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources.html) can be backed by lambda or [SNS topic](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources-sns.html#walkthrough-custom-resources-sns-adding-nonaws-resource)

## Resources

* [User Guide for Extension Development](https://docs.aws.amazon.com/cloudformation-cli/latest/userguide/what-is-cloudformation-cli.html)
* [Use Python to manage third-party resources in AWS CloudFormation | Amazon Web Services](https://aws.amazon.com/blogs/infrastructure-and-automation/using-python-to-create-aws-cloudformation-resource-providers/)
* [Learn Best Practices for Implementing AWS Lambda-backed Custom Resources with AWS CloudFormation](https://aws.amazon.com/premiumsupport/knowledge-center/best-practices-custom-cf-lambda/)
* [Writing an AWS CloudFormation Resource Provider in Python: Step by Step - Cloudar](https://www.cloudar.be/awsblog/writing-an-aws-cloudformation-resource-provider-in-python-step-by-step/)
* [CloudFormation Resource Providers - A Chicken and Egg Problem](https://garbe.io/blog/2020/02/24/cloudformation-resource-provider/)
* [Resolve the &quot;Resource timed out waiting for creation of physical resource&quot; error in AWS CloudFormation](https://aws.amazon.com/premiumsupport/knowledge-center/cloudformation-physical-resource-error/)