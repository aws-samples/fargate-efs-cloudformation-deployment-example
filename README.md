# Automating Amazon EFS file systems mounts for AWS Fargate

This CloudFormation template shows how to automate AWS Fargate cluster deployment backed by EFS share, which is connected using Access Points. Demo template shows how to achieve result, described in the [Amazon Elastic Container Service & AWS Fargate, now support Amazon Elastic File System](https://aws.amazon.com/blogs/aws/amazon-ecs-supports-efs/) article.

## Deployment

As current version of Python Lambda runtime environment has outdated version of `botocore` library. We may workaround that by using Lambda layer with updated `boto3` and `botocore` libraries code.

### Lambda layer

Build Lambda layer distribution:

```sh
cd lambda_layers/boto3_common
make
```

Deploy Lambda layer distribution:

```sh
make deploy
```

You may clean build artifacts by running:

```sh
make clean
```

Replace `FargateTaskDefinitionLambdaFunction` layer value in `ecs_efs_template.yaml` with your `LayerVersionArn`.

### Deploy CloudFormation stack

To deploy stack, execute the following command

```sh
aws cloudformation create-stack \
    --stack-name ecs-efs-stack \
    --template-body file://ecs_efs_template.yaml \
    --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM
```

Use the following command to delete the stack:

```sh
aws cloudformation delete-stack --stack-name ecs-efs-stack
```

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

