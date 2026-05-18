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

## Troubleshooting

### npm ci fails with "Missing: jsonschema@1.4.1 from lock file"

This is a known issue with aws-cdk-lib@2.254.0 and npm 11. The problem occurs due to conflicting bundled dependency versions:

- aws-cdk-lib bundles `jsonschema@1.5.0` at the top level
- But also bundles `@aws-cdk/cloud-assembly-api@2.2.3` which requires `jsonschema@~1.4.1`
- The version constraint `~1.4.1` excludes version `1.5.0`, creating an unsatisfiable dependency conflict
- npm 11's stricter bundled dependency validation detects this mismatch in the lock file

**Solution:**

Run the following command to regenerate the package lock file:

```bash
npm install --package-lock-only
```

Then proceed with:

```bash
npm ci
npm run build
npm test
```

For more details, see the [AWS CDK GitHub issue #37870](https://github.com/aws/aws-cdk/issues/37870).

## esbuild Requirement for cdk synth

Install `esbuild` in the project so CDK can bundle Lambda assets locally during synthesis:

```bash
npm i esbuild
```

If `esbuild` is not installed, `cdk synth` falls back to Docker to execute the bundling step.

## CDK Workflow: Before and After CI/CD Pipeline

### Until now with CDK

- Define our stacks
- Add them inside the `bin` file (the one with `app`)
- Run `cdk synth`
- Run `cdk deploy`

### With CodePipeline

- Define one Pipeline stack inside the `bin` file (the one with `app`)
- The pipeline contains stages (construct: `Stage`)
- The stages hold other stacks
- The pipeline runs synth/deploy automatically when code changes are pushed

## Resources

- 🎓 [Udemy Course — AWS TypeScript CDK, Serverless & React](https://www.udemy.com/course/aws-typescript-cdk-serverless-react/?couponCode=CP260518ALTMX)
- 📦 [Original Course Repository](https://github.com/alexhddev/CDK-course-resources)
- 📚 [AWS CDK CodePipelineSource API Documentation](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.pipelines.CodePipelineSource.html)
