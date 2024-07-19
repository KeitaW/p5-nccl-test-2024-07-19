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


```bash
eksctl create cluster -f eks-p5-odcr-vpc.yaml
```


```bash
EFA_INSTALLER_VERSION=1.33.0
AWS_OFI_NCCL_VERSION=v1.9.2-aws
NCCL_VERSION=v2.22.3-1
NCCL_TESTS_VERSION=v2.13.9
docker build  -f nccl-tests.Dockerfile \
       --build-arg="EFA_INSTALLER_VERSION=${EFA_INSTALLER_VERSION}" \
       --build-arg="AWS_OFI_NCCL_VERSION=${AWS_OFI_NCCL_VERSION}" \
       --build-arg="NCCL_VERSION=${NCCL_VERSION}" \
       --build-arg="NCCL_TESTS_VERSION=${NCCL_TESTS_VERSION}" \
       -t 159553542841.dkr.ecr.ap-northeast-1.amazonaws.com/nccl-test:${EFA_INSTALLER_VERSION}-${AWS_OFI_NCCL_VERSION}-${NCCL_VERSION}-${NCCL_TESTS_VERSION} \
       .
```


```bash
aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 159553542841.dkr.ecr.ap-northeast-1.amazonaws.com
docker push 159553542841.dkr.ecr.ap-northeast-1.amazonaws.com/nccl-test:${EFA_INSTALLER_VERSION}-${AWS_OFI_NCCL_VERSION}-${NCCL_VERSION}-${NCCL_TESTS_VERSION}
```


```bash
kubectl apply --server-side -f https://raw.githubusercontent.com/kubeflow/mpi-operator/v0.5.0/deploy/v2beta1/mpi-operator.yaml
```