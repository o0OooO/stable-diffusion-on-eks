# Overview

Before deploying the solution, we recommend that you review the architecture diagram and regional support information in this guide, and then follow the instructions below to configure and deploy the solution to your account.

## Prerequisites
Check all considerations in the [Deployment Planning](./considerations.md) document.

## Deployment Steps

We provide a one-click deployment script to get started quickly. The total deployment time is approximately 30 minutes.

### Get the Source Code

Run the following command to get the source code and deployment script:

```bash
git clone --recursive https://github.com/aws-samples/stable-diffusion-on-eks
cd stable-diffusion-on-eks
```

### One-Click Deployment

Run the following command to deploy with the simplest setup quickly:

```bash
cd deploy
./deploy.sh
```

The script will:

* Install necessary runtimes and tools
* Create an S3 bucket, download the base model of Stable Diffusion 1.5 from [HuggingFace](https://huggingface.co/runwayml/stable-diffusion-v1-5), and place it in the bucket
* Create an EBS snapshot containing the SD Web UI image using the sample image we provide
* Create a Stable Diffusion solution with the SD Web UI runtime

!!! warning

    The configuration file generated by this script is the simplest configuration, containing only 1 runtime, and cannot be customized (such as scaling thresholds, custom models, custom images, etc.). If you need to customize the configuration, please run the following command:

    ```bash
    ./deploy.sh -d
    ```

    This parameter will make the deployment script complete the pre-deployment preparation only, but not actually deploy. You can modify the configuration according to the [Configuration](./configuration.md), and then run the following command to deploy:

    ```bash
    cdk deploy --no-rollback --require-approval never
    ```


### Deployment Parameters

The script provides some parameters for you to customize the deployed solution:

* `-h, --help`: Show help information
* `-n, --stack-name`: Customize the name of the deployed solution, affecting the naming of generated resources. Default is `sdoneks`.
* `-R, --region`: The region where the solution is deployed. Default is the region of the current AWS configuration profile.
* `-d, --dry-run`: Generate configuration files only, do not deploy.
* `-b, --bucket`: Specify the name of an existing S3 bucket to store the model. The S3 bucket must already exist and be in the same region as the solution. You can manually create the S3 bucket according to [this document](./models.md).
* `-s, --snapshot`: Specify an existing EBS snapshot ID. You can build the EBS snapshot yourself according to [this document](./ebs-snapshot.md).
* `-r, --runtime-name`: Specify the name of the deployed runtime, affecting the name used when calling the API. Default is `sdruntime`.
* `-t, --runtime-type`: Specify the type of the deployed runtime, only accepting `sdwebui` and `comfyui`. Default is `sdwebui`.

## Manual Deployment

You can also deploy this solution on AWS manually without using the script by following these steps:

1. [Create an Amazon S3 model bucket](./models.md) and store the required models in the bucket
2. *(Optional)* [Build the container image](./image-building.md)
3. *(Optional)* [Store the container image in an EBS cache to accelerate startup](./ebs-snapshot.md)
4. [Deploy and launch the solution stack](./deploy.md)