# eks

## Prerequisites

- aws CLI
- aws-profile
- eksctl
- helm CLI

To assume AdminRole you also must setup MFA access.

Setup your ~/.aws/config with the following:

```
[jose-dev]
source_profile = jose
role_arn       = arn:aws:iam::849514089141:role/AdminRole
mfa_serial     = <YOUR OWN MFA ARN>
region         = us-west-2
duration_seconds = 3600
```
aws-profile set And select: assume andes_admin Verify your credentials are working:

aws sts get-caller-identity --no-cli-pager (This command will request to enter your mfa code) Then you should see something like this:

```
{
    "UserId": "XXXXXXXXXXXXX:botocore-session-12223333",
    "Account": "849514089141",
    "Arn": "arn:aws:sts::849514089141:assumed-role/AdminRole/botocore-session-1222333"
}
```
## Launch EKS

Create an eksctl deployment file (eksworkshop.yaml) use in creating your cluster using the following syntax:

```
cat << EOF > eks-example.yaml
---
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: eksworkshop-eksctl
  region: ${AWS_REGION}
  version: "1.23"

availabilityZones: ["${AZS[0]}", "${AZS[1]}", "${AZS[2]}"]

managedNodeGroups:
- name: eks-example
  desiredCapacity: 3
  instanceType: t3.small
  ssh:
    enableSsm: true

secretsEncryption:
  keyARN: ${MASTER_ARN}
EOF
```

