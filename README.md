# AWS Integration
1. Setup cloud environment
  1. Create VPC with a Private Subnet (don't need to create NAT Gateway)
  2. Create an EC2 instance in the subnet
  3. Create IAM User (this will be used by the builder to authenticate in AWS CLI)
    * Recommended: Grant user only the set of permissions required e.g. start/stop instance and scope it to the specific instance
    * Make note of the Access Key and Secret Key
2. Add the Access and Secret Keys to gitbhub by navigating to Settings > Secret
  * Call Access Key `AWS_ACCESS_KEY`
  * Call Secret Key `AWS_SECRET_KEY`
3. Go to your git repository and create a YAML configuration under .github/workflows. An example
```
name: AWS Integration

# Controls when the action will run. Triggers the workflow on push
on: push

jobs:
  aws-test:
    name: Turn EC2 Instance ON
    runs-on: ubuntu-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v2

    - name: Configure AWS credentials from Test account
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_KEY }}
        aws-region: eu-west-1

    - name: Turn instance ON
      run: |
        aws ec2 start-instances --instance-ids <instance ID>
```

Once the YAML file is committed and pushed, github will automatically run the build. This can viewed under Actions. Select the Workflow on the left and select a job on the right. A job can be re-run by clicking the `Rerun Job` button on the right.
For PoC, the configuration only starts the instance.
