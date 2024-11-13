## github-action-s3-and-cloudfront

This Github Action is designed to automate the upload of static Website files into AWS on commit to main. It will upload the changed files in the Repo into the S3 Bucket and then invalidate those same files in the Cloudfront Distribution. It uses Open ID Connect (OIDC) to authenticate with AWS.

To enable this action you need to first OIDC in AWS if not already configured; the official guide can be found [here](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services).

Alternatively follow these steps:
1. Setup the Open ID Provider:
   - Got to your AWS account -> IAM -> Identity providers -> Create Identity Provider. Select OpenID connect.
   - For the provider URL: Use ‘https://token.actions.githubusercontent.com’
   - For the “Audience”: Use ‘sts.amazonaws.com’
   - Click on ‘Get thumbprint’ and then ‘Add provider’ button.
2. Create the IAM Role:
   - IAM -> Roles -> Create role.
   - Select ‘Web Identity’.
   - Identity provider: ‘token.actions.githubusercontent.com’.
   - Audience: ‘sts.amazonaws.com’.
   - Click Next and add permissions. Either use the native roles 'AmazonS3FullAccess' and 'CloudFrontFullAccess' or create a custom policy that restricts to certain buckets and distributions.
   - Click Next and name the role to your choice.
3. Congfigure the Repo Secrets:
   - AWS_ROLE_ARN = This is the ARN of the OIDC Role created in AWS.
   - AWS_REGION = The region where the Bucket is hosted in AWS.
   - AWS_PROD_S3_BUCKET = Simply the name of the bucket you wish to upload files to.
   - AWS_CLOUDFRONT_DISTRIBUTION_ID = The Cloudfront Distribution ID.
4. At this point it should work and upload changed files in the Commit to Main to your bucket, and Invalidate those same files in your Cloudfront Distribution.
