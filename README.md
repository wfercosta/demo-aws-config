# Criação da infraestrutura básica

## Exportado varáveis de ambiente

```
export DEMO_BUCKET_CONFIG=demo-aws-config-items-recorded
export DEMO_SUBSCRIPTION_EMAIL=<email>
```

## Provisionando configuração de VPC e Subnets

```
aws cloudformation create-stack \
 --stack-name demo-config-res-network \
 --template-body file://01_config_res_network.yaml
```

## Provisionando e configurando o AWS Config

```
aws cloudformation create-stack \
 --stack-name demo-config-res-config \
 --template-body file://02_config_res_aws_config.yaml \
 --parameters ParameterKey="NotificationEmail",ParameterValue="${DEMO_SUBSCRIPTION_EMAIL}" \
 --capabilities CAPABILITY_NAMED_IAM
```

## Lab - Bucket S3 Rules & Timeline

```
aws cloudformation create-stack \
 --stack-name demo-config-res-example-s3 \
 --template-body file://03-0_s3_encryption_disabled.yaml
```

```
aws cloudformation update-stack \
 --stack-name demo-config-res-example-s3 \
 --template-body file://03-1_s3_encryption_enabled.yaml
```

```
aws cloudformation delete-stack \
 --stack-name demo-config-res-example-s3
```

## Lab - EC2 Innstance com volumes EBS encrypted e non encrypted

```
aws cloudformation create-stack \
 --stack-name demo-config-res-example-ec2-ebs \
 --template-body file://04_ec2_ebs_encryption.yaml
```

## Lab - Postmortem - ECS Cluster - Service Failing

**Repositorio imagem:** https://github.com/gkoenig/go-simplehttp

```
aws cloudformation create-stack \
 --stack-name demo-config-postmortem \
 --template-body file://05_postmortem_ecs.yaml \
 --capabilities CAPABILITY_NAMED_IAM
```

# Cleanup

```
aws cloudformation delete-stack \
 --stack-name demo-config-postmortem

aws cloudformation delete-stack \
 --stack-name demo-config-res-example-ec2-ebs

aws cloudformation delete-stack \
 --stack-name demo-config-res-example-s3

aws cloudformation delete-stack \
 --stack-name demo-config-res-config

aws cloudformation delete-stack \
 --stack-name demo-config-res-network
```

```
aws s3 rm s3://${DEMO_BUCKET_CONFIG} --recursive

aws s3 rb s3://${DEMO_BUCKET_CONFIG} --force
```
