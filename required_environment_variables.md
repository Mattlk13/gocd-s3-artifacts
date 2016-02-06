Required environment variables
===

The task plugins `Fetch` and `Publish` run on the agents. The `Material` plugin runs on the server.

## Server

The `Material` plugin needs AWS credentials to poll S3 for artifacts. These can be provided through the environment variables `AWS_SECRET_ACCESS_KEY` and  `AWS_ACCESS_KEY_ID`. The credentials are also picked up from the alternate locations that the [AWS Java SDK looks at.][1]

Alternatively, you can use EC2 IAM roles by specifying the environment variable AWS_USE_IAM_ROLE. Note that just the presence of this variable is enough, the value doesn't matter.

## Agent

Both the `Fetch` and `Task` plugins require the following environment variables to work. These can be set as environment variables on the agents, pipeline or even Go's Environment. The recommended approach is to set these on the agents.

`AWS_SECRET_ACCESS_KEY` and  `AWS_ACCESS_KEY_ID` are the required AWS credentials to upload and download artifacts from S3. As with the server, EC2 IAM roles can be used if the `AWS_USE_IAM_ROLE` exists (with any value.)

`GO_ARTIFACTS_S3_BUCKET` is the name of the S3 bucket where artifacts will be uploaded. Artifacts are uploaded to the path `s3://<GO_ARTIFACTS_S3_BUCKET>/<PIPELINE_NAME>/<STAGE_NAME>/<JOB_NAME>/<PIPELINE_COUNTER>.<STAGE_COUNTER>` as the root path.

In addition to the above, the `Publish` plugin also requires `GO_SERVER_DASHBOARD_URL` environment variable. `GO_SERVER_DASHBOARD_URL` is the url of the Go server web UI (dashboard). This is needed to reliably determine the dashboard url so that we can provide a trackback url to the source of the artifacts.

With the new release (from 2.0.0) when publishing artifacts you can optionaly choose the storage class using the `AWS_STORAGE_CLASS` environment variable. The accepted values are `standard`, `standard-ia`, `rrs` and `glacier`.

[1]: http://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/s3/AmazonS3Client.html#AmazonS3Client()
