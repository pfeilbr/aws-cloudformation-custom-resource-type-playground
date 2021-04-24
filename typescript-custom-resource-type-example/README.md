# typescript-custom-resource-type-example

Based on [Introducing TypeScript support for building AWS CloudFormation resource types | Amazon Web Services](https://aws.amazon.com/blogs/mt/introducing-typescript-support-for-building-aws-cloudformation-resource-types/)

## Create and Deploy

```sh
cfn init

# generate model python code from schema .json
cfn generate

# build
npm run build

# test with SAM
sam local invoke TestEntrypoint --event sam-tests/create.json

# register in CloudFormation registry
cfn submit --set-default --region us-east-1
```

## Use `Org::PlatformService::CDN` in Stack

see [`app.yaml`](app.yaml) for `Org::PlatformService::CDN` usage

```sh
REGION="us-east-1"
STACK_NAME="typescript-custom-resource-type-example-stack-01"

# create application (app) that uses ``Org::PlatformService::CDN`
aws cloudformation create-stack \
    --region "${REGION}" \
    --stack-name "${STACK_NAME}" \
    --template-body file://app.yaml \
    --capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM"

# wait for stack to be created
aws cloudformation wait stack-create-complete \
    --region "${REGION}" \
    --stack-name "${STACK_NAME}"

# log are sent to CloudWatch log group named `org-platformservice-cdn-logs`
# tail logs and watch
aws logs tail org-platformservice-cdn-logs --follow

# update application (app) that uses ``Org::PlatformService::CDN`
aws cloudformation update-stack \
    --region "${REGION}" \
    --stack-name "${STACK_NAME}" \
    --template-body file://app.yaml \
    --capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM"

# wait for stack to be updated
aws cloudformation wait stack-update-complete \
    --region "${REGION}" \
    --stack-name "${STACK_NAME}"

# clean up
aws cloudformation delete-stack \
    --region "${REGION}" \
    --stack-name "${STACK_NAME}"

# wait for stack to be deleted
aws cloudformation wait stack-delete-complete \
    --region "${REGION}" \
    --stack-name "${STACK_NAME}"
```

## Example Lambda Handler Parameters

* [`example-create-request-lambda-parameters.json`](example-create-request-lambda-parameters.json)
* [`example-update-request-lambda-parameters.json`](example-update-request-lambda-parameters.json)
* [`example-delete-request-lambda-parameters.json`](example-delete-request-lambda-parameters.json)

## Example Resource Event Data

These event data are logged in CloudWatch Logs (`org-platformservice-cdn-logs` log group).

* [`event-data-create.json`](event-data-create.json)
* [`event-data-read.json`](event-data-read.json)
* [`event-data-update.json`](event-data-update.json)
* [`event-data-delete.json`](event-data-delete.json)

## Resources

* [Introducing TypeScript support for building AWS CloudFormation resource types | Amazon Web Services](https://aws.amazon.com/blogs/mt/introducing-typescript-support-for-building-aws-cloudformation-resource-types/)

---

original `README.md` below from `cfn init`

---


# Org::PlatformService::CDN

Congratulations on starting development! Next steps:

1. Write the JSON schema describing your resource, [org-platformservice-cdn.json](./org-platformservice-cdn.json)
2. Implement your resource handlers in [handlers.ts](./org-platformservice-cdn/handlers.ts)

> Don't modify [models.ts](./org-platformservice-cdn/models.ts) by hand, any modifications will be overwritten when the `generate` or `package` commands are run.

Implement CloudFormation resource here. Each function must always return a ProgressEvent.

```typescript
const progress = ProgressEvent.builder<ProgressEvent<ResourceModel>>()

    // Required
    // Must be one of OperationStatus.InProgress, OperationStatus.Failed, OperationStatus.Success
    .status(OperationStatus.InProgress)
    // Required on SUCCESS (except for LIST where resourceModels is required)
    // The current resource model after the operation; instance of ResourceModel class
    .resourceModel(model)
    .resourceModels(null)
    // Required on FAILED
    // Customer-facing message, displayed in e.g. CloudFormation stack events
    .message('')
    // Required on FAILED a HandlerErrorCode
    .errorCode(HandlerErrorCode.InternalFailure)
    // Optional
    // Use to store any state between re-invocation via IN_PROGRESS
    .callbackContext({})
    // Required on IN_PROGRESS
    // The number of seconds to delay before re-invocation
    .callbackDelaySeconds(0)

    .build()
```

While importing the [@amazon-web-services-cloudformation/cloudformation-cli-typescript-lib](https://github.com/eduardomourar/cloudformation-cli-typescript-plugin) library, failures can be passed back to CloudFormation by either raising an exception from `exceptions`, or setting the ProgressEvent's `status` to `OperationStatus.Failed` and `errorCode` to one of `HandlerErrorCode`. There is a static helper function, `ProgressEvent.failed`, for this common case.

Keep in mind, during runtime all logs will be delivered to CloudWatch if you use the `log()` method from `LoggerProxy` class.
