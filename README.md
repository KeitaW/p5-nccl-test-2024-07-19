## NCCL tests

## Infrastructure setup 

https://console.aws.amazon.com/cloudformation/home#/stacks/quickcreate?templateURL=https://raw.githubusercontent.com/aws-samples/awsome-distributed-training/main/1.architectures/1.vpc_network/1.vpc-multi-az.yaml

```bash
aws --region ap-northeast-1 cloudformation create-stack \
    --stack-name vpc-stack-ml\
    --template-body file://1.vpc-multi-az.yaml \
    --parameters ParameterKey=AvailabilityZones,ParameterValue=ap-northeast-1a\\,ap-northeast-1c\
        ParameterKey=NumberOfAZs,ParameterValue=2 \
        ParameterKey=VPCName,ParameterValue="ML HPC VPC" \
    --capabilities CAPABILITY_IAM
```