---
title: Topics
description: How Nitric deploys Topics to Google Cloud
---

Nitric Topics are deployed to GCP using [pubsub](https://cloud.google.com/pubsub).

## GCP Resources

The following resources are created when deploying Topics to GCP:

- Pubsub Topic
- Pubsub Subscription (push)
- Cloud Run (subscribers)

## Deployment

During deployment, your topics will be provisioned as follows:

- The topic definition and resource group are deployed as a PubSub topic
- Cloud Run applications that subscribe will have a push subscription deployed for them