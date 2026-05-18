# Welcome to your CDK TypeScript project

This is a blank project for CDK development with TypeScript.

The `cdk.json` file tells the CDK Toolkit how to execute your app.

## Useful commands

* `npm run build`   compile typescript to js
* `npm run watch`   watch for changes and compile
* `npm run test`    perform the jest unit tests
* `npx cdk deploy`  deploy this stack to your default AWS account/region
* `npx cdk diff`    compare deployed stack with current state
* `npx cdk synth`   emits the synthesized CloudFormation template

## GitHub Token Setup for CI/CD

This section describes the steps required to configure GitHub authentication for the CDK CI/CD pipeline.

### Steps to Set Up GitHub Token

1. **Create a GitHub Token**
   - Go to GitHub and navigate to your account settings
   - Create a personal access token with the necessary permissions
   - The token requires the following permissions:
     - `repo` - to read the repository
     - `admin:repo_hook` - if you plan to use webhooks (enabled by default)

2. **Add the Token to AWS Secrets Manager**
   - Store the GitHub token in AWS Secrets Manager
   - Create a secret named `github-token` (unless specified otherwise)
   - This secret will be used by the CDK pipeline for authentication

### Important Notes

- Authentication for the CodePipeline source will be performed automatically using the `github-token` secret stored in AWS Secrets Manager
- If you rotate the value in the Secret, you must also change at least one property on the Pipeline to force CloudFormation to re-read the secret

## Resources

- 🎓 [Udemy Course — AWS TypeScript CDK, Serverless & React](https://www.udemy.com/course/aws-typescript-cdk-serverless-react/?couponCode=CP260518ALTMX)
- 📦 [Original Course Repository](https://github.com/alexhddev/CDK-course-resources)
- 📚 [AWS CDK CodePipelineSource API Documentation](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.pipelines.CodePipelineSource.html)
