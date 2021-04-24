# python-custom-resource-type-example

Based on [Use Python to manage third-party resources in AWS CloudFormation | Amazon Web Services](https://aws.amazon.com/blogs/infrastructure-and-automation/using-python-to-create-aws-cloudformation-resource-providers/)

## Create and Deploy

```sh
cfn init

# generate model python code from schema .json
cfn generate

# test with SAM
sam local invoke TestEntrypoint --event example_inputs/inputs_1_create.json

# register in CloudFormation registry
cfn submit --set-default --region us-east-1
```

## Use `MyOrg::MyService::MyResource` in Stack

see [`app.yaml`](app.yaml) for `MyOrg::MyService::MyResource` usage

```sh
REGION="us-east-1"
STACK_NAME="python-custom-resource-type-example-stack-01"

# create application (app) that uses ``MyOrg::MyService::MyResource`
aws cloudformation create-stack \
    --region "${REGION}" \
    --stack-name "${STACK_NAME}" \
    --template-body file://app.yaml \
    --capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM" \

# wait for stack to be created
aws cloudformation wait stack-create-complete \
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

---

## screenshots

### Cloudformation Registry | Custom Resource

<img src="https://www.evernote.com/l/AAFS2jD6YstKwr0sO8DlIX0DgNDLvZkUmOMB/image.png" alt="" width="75%" />

### Cloudformation Registry | Custom Resource Detail View

<img src="https://www.evernote.com/l/AAE0a18Y_45GkLtGN375nLj7G0e6_17zas4B/image.png" alt="" width="75%" />

### Cloudformation | stack creation with custom resource type

<img src="https://www.evernote.com/l/AAEy7sTaWrRCqI0r0vujLsdQDpTR2Ifx77UB/image.png" alt="" width="75%" />

### Cloudformation | stack output for custom resource type

<img src="https://www.evernote.com/l/AAHZwUytc8lOxo8SkCSwAFDOzXyoBk7IlREB/image.png" alt="" width="75%" />

### Custom Resource Lambda Handler Logs Sent to CloudWatch Logs

<img src="https://www.evernote.com/l/AAHbbrUkD4pClbbpKZq4FKMb99QGzo5WwLQB/image.png" alt="" width="75%" />

---

original `README.md` below from `cfn init`

---

# MyOrg::MyService::MyResource

Congratulations on starting development! Next steps:

1. Write the JSON schema describing your resource, `myorg-myservice-myresource.json`
2. Implement your resource handlers in `myorg_myservice_myresource/handlers.py`

> Don't modify `models.py` by hand, any modifications will be overwritten when the `generate` or `package` commands are run.

Implement CloudFormation resource here. Each function must always return a ProgressEvent.

```python
ProgressEvent(
    # Required
    # Must be one of OperationStatus.IN_PROGRESS, OperationStatus.FAILED, OperationStatus.SUCCESS
    status=OperationStatus.IN_PROGRESS,
    # Required on SUCCESS (except for LIST where resourceModels is required)
    # The current resource model after the operation; instance of ResourceModel class
    resourceModel=model,
    resourceModels=None,
    # Required on FAILED
    # Customer-facing message, displayed in e.g. CloudFormation stack events
    message="",
    # Required on FAILED: a HandlerErrorCode
    errorCode=HandlerErrorCode.InternalFailure,
    # Optional
    # Use to store any state between re-invocation via IN_PROGRESS
    callbackContext={},
    # Required on IN_PROGRESS
    # The number of seconds to delay before re-invocation
    callbackDelaySeconds=0,
)
```

Failures can be passed back to CloudFormation by either raising an exception from `cloudformation_cli_python_lib.exceptions`, or setting the ProgressEvent's `status` to `OperationStatus.FAILED` and `errorCode` to one of `cloudformation_cli_python_lib.HandlerErrorCode`. There is a static helper function, `ProgressEvent.failed`, for this common case.

## What's with the type hints?

We hope they'll be useful for getting started quicker with an IDE that support type hints. Type hints are optional - if your code doesn't use them, it will still work.
