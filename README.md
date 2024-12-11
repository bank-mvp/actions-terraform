# Deploy Terraform with Workspace Support

This GitHub Action initializes and applies Terraform configurations with support for workspace selection. It is designed to facilitate the deployment of infrastructure as code (IaC) using Terraform within an AWS environment.

## Inputs

### `aws_access_key_id`

**Required** AWS Access Key ID.

### `aws_secret_access_key`

**Required** AWS Secret Access Key.

### `aws_region`

**Required** AWS Region where the resources will be deployed.

### `terraform_directory`

**Required** The directory where your Terraform configurations are located.

### `terraform_workspace`

**Required** The Terraform workspace to use for managing state.

### `deploy`

Whether to apply the changes (deploy resources) or not. Defaults to `false`. If set to `true`, Terraform will apply the changes specified in the configuration.

## How it Works

The action performs the following steps:

1. **Checkout Code**: Checks out the repository code, allowing the action to access the Terraform configurations.
2. **Setup Terraform**: Sets up Terraform CLI with the specified version.
3. **Configure AWS Credentials**: Configures AWS credentials using the provided access key, secret access key, and region.
4. **Initialize Terraform and Set Workspace**: Initializes Terraform within the specified directory and selects the specified workspace. If the workspace does not exist, it will be created.
5. **Plan Terraform**: Generates an execution plan for Terraform. This step is skipped if `deploy` is set to `true`.
6. **Apply Terraform**: Applies the Terraform configurations to create or update resources. This step is executed only if `deploy` is set to `true`.

## Usage

To use this action, create a `.github/workflows` directory in your repository if it doesn't already exist. Then, create a workflow file (e.g., `.github/workflows/terraform_deploy.yml`) and include the following configuration:

```yml
name: Deploy Terraform with Workspace Support

on:
  push:
    branches:
      - main

env:
  AWS_REGION: 'eu-central-1'
  TERRAFORM_DIRECTORY: '/infrastructure'

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - name: 'Deploy Terraform with Workspace Support'
        uses: github.com/alessonviana/action-terraform-workspace@v0.0.1
        with:
          aws_access_key_id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws_secret_access_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws_region: ${{ env.AWS_REGION }}
          terraform_directory: ${{ env.TERRAFORM_DIRECTORY }}
          terraform_workspace: 'dev || prd'
          deploy: 'true'
