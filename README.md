# Static Website Deployment to S3 using GitHub Actions

This project demonstrates how to automatically deploy a static website to Amazon S3 using GitHub Actions.

## Demo (using cloudfront)
[Website Link](https://d173t4rztrz688.cloudfront.net)

## Prerequisites

1. An AWS account
2. A GitHub account
3. A static website (HTML, CSS, JS files)

## Setup

### 1. Create an S3 bucket

1. Log in to your AWS Console
2. Navigate to S3 and create a new bucket
3. Enable static website hosting for the bucket
4. Set the bucket policy to allow public read access

### 2. Create a CloudFront distribution

1. Go to the CloudFront service in the AWS Console
2. Click "Create Distribution"
3. For "Origin Domain," select your S3 bucket
4. For "Viewer Protocol Policy," choose "Redirect HTTP to HTTPS"
5. Set "Default Root Object" to "index.html"

### 3. Update S3 bucket policy

Update your bucket policy to allow CloudFront access:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}
```

### 4. Create an IAM user for deployment

1. In the AWS Console, go to IAM
2. Create a new user with programmatic access
3. Attach a policy that allows S3 access to your bucket
4. Save the Access Key ID and Secret Access Key


### 5. Configure GitHub Secrets

In your GitHub repository:

1. Go to Settings > Secrets
2. Add the following secrets:
   - `AWS_ACCESS_KEY_ID`: Your IAM user's Access Key ID
   - `AWS_SECRET_ACCESS_KEY`: Your IAM user's Secret Access Key
   - `AWS_S3_BUCKET`: Your S3 bucket name
   - `AWS_REGION`: The region of your S3 bucket (e.g., us-east-1)
   - `AWS_CLOUDFRONT_DISTRIBUTION_ID`: Your CloudFront distribution ID


### 6. Create GitHub Actions workflow

Create a file named `.github/workflows/deploy.yml` in your repository