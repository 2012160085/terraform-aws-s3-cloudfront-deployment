# terraform-aws-s3-cloudfront-deployment

This is a simple Terraform module that deploys a static website to an S3 bucket and creates a CloudFront distribution in front of it. The module also creates a Route 53 record set for the website.

## Prerequisites
- S3 bucket with the website content
- Hosted zone in Route 53
- ACM certificate for the domain

## Post-deployment
After the deployment, you will need to upload the website content to the S3 bucket and create a CloudFront invalidation to update the cache. 
You can do this by running the following command:
```bash
aws aws s3 sync path/to/website s3://<YOUR_BUCKET> --exclude ".git/*"
aws cloudfront create-invalidation --distribution-id <YOUR_DISTRIBUTION_ID> --paths "/*"
```

## Usage example
```hcl
module "deployment" {
  source              = "./module"
  domain              = "www.my-domain.com"
  hosted_zone_name    = "my-domain.com"
  s3_bucket_name      = "my-bucket"
  acm_certificate_arn = "arn:aws:acm:us-east-1:123456789012:certificate/12341234-asdf-1234-1234-asdfasdfasdf"
}

output "deployment_guide" {
  value = module.name.deployment_guide
}
```

## Inputs
### Required
- `hosted_zone_name` - The name of the hosted zone in Route 53
- `domain` - The domain name of the website
- `acm_certificate_arn` - The ARN of the ACM certificate
### Optional
- `s3_bucket_name` - The name of the S3 bucket
- `default_root_object` - The default root object of the website (`default`: `index.html`)
