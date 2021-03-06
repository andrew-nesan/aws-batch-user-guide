# How AWS Batch Works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to AWS Batch, you should understand what IAM features are available to use with AWS Batch\. To get a high\-level view of how AWS Batch and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [AWS Batch Identity\-Based Policies](#security_iam_service-with-iam-id-based-policies)
+ [AWS Batch IAM Roles](#security_iam_service-with-iam-roles)

## AWS Batch Identity\-Based Policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. AWS Batch supports specific actions, resources, and condition keys\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

The `Action` element of an IAM identity\-based policy describes the specific action or actions that will be allowed or denied by the policy\. Policy actions usually have the same name as the associated AWS API operation\. The action is used in a policy to grant permissions to perform the associated operation\.

Policy actions in AWS Batch use the following prefix before the action: `batch:`\. For example, to grant someone permission to submitan AWS Batch job with the AWS Batch `SubmitJob` API operation, you include the `batch:SubmitJob` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. AWS Batch defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows\.

```
"Action": [
      "batch:action1",
      "batchaction2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Describe`, include the following action\.

```
"Action": "batch:Describe*"
```



To see a list of AWS Batch actions, see [Actions Defined by AWS Batch](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsbatch.html#awsbatch-actions-as-permissions) in the *IAM User Guide*\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

The `Resource` element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. You specify a resource using an ARN or using the wildcard \(\*\) to indicate that the statement applies to all resources\.



The AWS Batch job\-definition resource has the following ARN\.

```
arn:${Partition}:batch:${Region}:${Account}:job-definition/${JobDefinition}
```

For more information about the format of ARNs, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.

For example, to specify the `JobDefinitionAlpha` job definition in your statement, use the following ARN\.

```
"Resource": "arn:aws:batch:us-east-1:123456789012:job-definition/JobDefinitionAlpha"
```

To specify all instances that belong to a specific account, use the wildcard \(\*\)\.

```
"Resource": "arn:aws:batch:us-east-1:123456789012:job-definition/*"
```

Some AWS Batch actions, such as those for creating resources, cannot be performed on a specific resource\. In those cases, you must use the wildcard \(\*\)\.

```
"Resource": "*"
```

One AWS Batch API action involves multiple resources\. `SubmitJob` submits a job to a job queue so an IAM user must have permissions to use the job definition and the job queue\. To specify multiple resources in a single statement, separate the ARNs with commas\.

```
"Resource": [
      "resource1",
      "resource2"
```

To see a list of AWS Batch resource types and their ARNs, see [Resources Defined by AWS Batch](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsbatch.html#awsbatch-resources-for-iam-policies) in the *IAM User Guide*\. To learn with which actions you can specify the ARN of each resource, see [Actions Defined by AWS Batch](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsbatch.html#awsbatch-actions-as-permissions)\.

### Condition Keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can build conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\.

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM Policy Elements: Variables and Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\.

AWS Batch defines its own set of condition keys and also supports using some global condition keys\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

 The AWS Batch [RegisterJobDefinition](https://docs.aws.amazon.com/batch/latest/APIReference/API_RegisterJobDefinition.html) action supports the `batch:User`, `batch:Privileged`, and `batch:Image` condition keys\. For more information, see [Supported Resource\-Level Permissions for AWS Batch API Actions](batch-supported-iam-actions-resources.md)\.

To see a list of AWS Batch condition keys, see [Condition Keys for AWS Batch](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsbatch.html#awsbatch-policy-keys) in the *IAM User Guide*\. To learn with which actions and resources you can use a condition key, see [Actions Defined by AWS Batch](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsbatch.html#awsbatch-actions-as-permissions)\.

### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of AWS Batch identity\-based policies, see [Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

AWS Batch does not support resource\-based policies\.

AWS Batch does not support tagging resources or controlling access based on tags\.

## AWS Batch IAM Roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

### Using Temporary Credentials with AWS Batch<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\.

AWS Batch supports using temporary credentials\.

### Service\-Linked Roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view but not edit the permissions for service\-linked roles\.

AWS Batch does not support service\-linked roles\.

### Service Roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

AWS Batch supports service roles\.