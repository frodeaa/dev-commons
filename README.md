# dev-commons

This is a set of resource for setting up a light-weight development machine that
runs docker shells for particular development environments.

The only applications installed on the host are ```vim```, ```git```, ```tmux```
and ```docker```. Any other applications or resources needed for development
should be installed in dedicated docker containers

## Prerequisite - provisioning an SSH key pair

To get an SSH key pair that can be used to access the EC2 host created by the
```create-dev-env.yml``` template, we can use the AWS CLI:

    # see https://docs.aws.amazon.com/cli/latest/reference/ec2/create-key-pair.html
    aws ec2 create-key-pair --key-name MyKeyPair > key.out

    jq '.KeyMaterial' --raw-output < key.out > key.pem

The ```.pem``` created contains the private key for the SSH key pair. Make sure to
keep it safe and secure.

## Setup

Use the ```create-dev-env.yml``` CloudFormation template to create the EC2 instance.

    # see https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html
    aws cloudformation create-stack \
      --stack-name dev-machine \
      --parameters ParameterKey=KeyName,ParameterValue=MyKeyPair \
      --template-body file://create-dev-env.yml
