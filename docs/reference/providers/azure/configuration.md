---
title: Azure Configuration
description: Configuring your Azure stacks
---

# Complete Example with comments

```yaml
# The name of your Azure stack
name: my-azure-stack-name
# The target Azure region to deploy to
# See available regions:
# https://azure.microsoft.com/en-us/explore/global-infrastructure/products-by-region/?products=container-apps
region: my-azure-stack-region

# Org to associate deployed API Management services with
org: example-org
# Admin email to associate deployed API Management services with
adminemail: test@example.com

# All configuration below is optional

# Configure your deployed functions/services
config:
  # How functions without a type will be deployed
  default:
    # configure a sample rate for telemetry (between 0 and 1) e.g. 0.5 is 50%
    telemetry: 0
    # configure functions to deploy to Google Cloud Run
    # see: https://learn.microsoft.com/en-us/azure/container-apps/containers#configuration
    containerapps: # Available since v0.26.0
      # set 1/4 vCPU
      cpu: 0.25
      # set 0.5GB of RAM
      memory: 0.5
      # The minimum number of instances to scale down to
      min-replicas: 0
      # The maximum number of instances to scale up to
      max-replicas: 10
  # Additional deployment types
  # You can target these types by setting a `type` in your project configuration
  big-service:
    telemetry: 0
    containerapps:
      memory: 1
      min-replicas: 2
      max-replicas: 100
```

> Missing something? Let us know by raising an issue in [github](https://github.com/nitrictech/nitric) or by dropping us a line on [discord](https://discord.com/invite/Webemece5C)