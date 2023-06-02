---
title: AWS Configuration
description: Configuring your AWS stacks
---

# Complete Example with comments

```yaml
# The name of your AWS stack
name: my-aws-stack-name
# The target aws region to deploy to
# See available regions:
# https://docs.aws.amazon.com/general/latest/gr/lambda-service.html
region: my-aws-stack-region
# The timezone that deployed schedules will run with
# Format is in tz identifiers:
# https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
schedule-timezone: Australia/Sydney # Available since v0.27.0

# Import existing AWS Resources
# Currently only secrets are supported
# Available since v0.28.0
import:
  # A name ARN map of secrets, where the name matches the nitric name of the secret you would like to import
  secrets: # Available since v0.28.0
    # In typescript this would import the provided secret reference for a secret declared as
    # const mySecret = secret('my-secret');
    my-secret: arn:...

# Configure your deployed functions/services
config:
  # How functions without a type will be deployed
  default:
    # configure a sample rate for telemetry (between 0 and 1) e.g. 0.5 is 50%
    telemetry: 0
    # configure functions to deploy to AWS lambda
    lambda: # Available since v0.26.0
      # set 128MB of RAM
      # See lambda configuration docs here:
      # https://docs.aws.amazon.com/lambda/latest/dg/configuration-function-common.html#configuration-memory-console
      memory: 128
      # set a timeout of 15 seconds
      # See lambda timeout values here:
      # https://docs.aws.amazon.com/lambda/latest/dg/configuration-function-common.html#configuration-timeout-console
      timeout: 15
      # set a provisioned concurrency value
      # For info on provisioned concurrency for AWS Lambda see:
      # https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html
      provisioned-concurrency: 0
  # Additional deployment types
  # You can target these types by setting a `type` in your project configuration
  big-service:
    telemetry: 0
    lambda:
      memory: 1024
      timeout: 60
      provisioned-concurrency: 1
```

> Missing something? Let us know by raising an issue in [github](https://github.com/nitrictech/nitric) or by dropping us a line on [discord](https://discord.com/invite/Webemece5C)