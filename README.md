# create the env
mkvirtualenv --python=/usr/bin/python3 zep

# display envs
workon

# enter the env
workon zep

# install the AWS CLI
pip install --upgrade awscli

# verify installation
```(zep) $ aws --version
aws-cli/1.14.64 Python/3.6.3 Linux/4.13.0-37-generic botocore/1.9.17```

# set up credentials
export AWS_CONFIG_FILE="./config"
export AWS_SHARED_CREDENTIALS_FILE="./credentials"

# set up IAM user
make sure you assign `AmazonElasticMapReduceforEC2Role` and to user with programmatic access. Attach policy directly. and `AmazonEC2ContainerServiceforEC2Role` 

# create default roles
aws emr create-default-roles

```(zep) $ aws emr create-default-roles
[
    {
        "Role": {
            "Path": "/",
            "RoleName": "EMR_AutoScaling_DefaultRole",
            "RoleId": "XXXX",
            "Arn": "arn:aws:iam::888:role/EMR_AutoScaling_DefaultRole",
            "CreateDate": "2018-03-27T22:12:31.876Z",
            "AssumeRolePolicyDocument": {
                "Version": "2008-10-17",
                "Statement": [
                    {
                        "Sid": "",
                        "Effect": "Allow",
                        "Principal": {
                            "Service": [
                                "elasticmapreduce.amazonaws.com",
                                "application-autoscaling.amazonaws.com"
                            ]
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            }
        },
        "RolePolicy": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Action": [
                        "cloudwatch:DescribeAlarms",
                        "elasticmapreduce:ListInstanceGroups",
                        "elasticmapreduce:ModifyInstanceGroups"
                    ],
                    "Effect": "Allow",
                    "Resource": "*"
                }
            ]
        }
    }
]
```

# spool up a cluster
```aws emr create-cluster --name "zepCluster" --release-label emr-5.12.0 --applications Name=Spark --instance-type m3.xlarge --instance-count 3 --use-default-roles --configurations file://./clusterConfig.json```

Needs work due to EMR roles not being correct
