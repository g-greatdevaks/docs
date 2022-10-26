---
title: Queues
description: How Nitric deploys Queues to AWS
---

Nitric Queues are deployed using [Amazon Simple Queue Service (SQS)](https://aws.amazon.com/sqs/).

## AWS Resources

The following resources are created when deploying queues to AWS:

- SQS queues
- IAM Policies

## Deployment

During deployment Nitric's CLI creates you stack's queues:

- Each uniquely named queue declaration will be setup as a new SQS queue
- Functions provided with access to the queues will assigned policies using IAM to enable that access.