# Bootstrap App Action

Github action for provisioning resources on AWS for an application

## Inputs

| Name                 | Description                                           | Required |
|----------------------|-------------------------------------------------------|----------|
| `name`               | Configuration name, used for backend name.            | `true`   |
| `aws_s3_bucket_name` | Name of S3 bucket where Terraform state is persisted. | `true`   |
| `aws_region`         | Target AWS Region.                                    | `true`   |
| `context`            | Terraform working directory.                          | `false`  |

## Example usage

To use this action in your workflow, add the following steps to your `.github/workflows` file:

```yaml
name: Provision resources

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::${{ secrets.aws_account_id }}:role/${{ secrets.aws_role_name }}
          aws-region: ${{ inputs.aws_region }}
      - name: Provision resources
        uses: rdeak/bootstrap-app-action@v1
        with:
          name: my-app
          aws_s3_bucket_name: my-terraform-bucket
          aws_region: us-west-2
          context: .deploy
```

## License

This project is licensed under the terms of the MIT license.