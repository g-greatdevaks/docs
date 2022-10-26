---
title: schedule.every()
description: Sets the frequency and one or many handlers to be triggered.
---

Sets the frequency and one or many handlers to be triggered.

```javascript
import { schedule } from '@nitric/sdk';

// Create a schedule that runs every 3 hours
schedule('send-reminder').every('3 hours', async (ctx) => {
  // do some processing
});
```

## Parameters

---

**rate** required `string`

The rate to run the schedule, e.g. '7 days'. All rates accept a number and a frequency. Valid frequencies are 'days', 'hours' or 'minutes'.

**middleware** required `EventMiddleware` or `EventMiddleware[]`

One or more middleware functions to use as the handler which will run on defined frequency.

---

## Examples

### Create a Schedule to run every 3 minutes

```javascript
import { schedule } from '@nitric/sdk';

// Create a schedule that runs every 3 minutes
schedule('send-reminder').every('3 minutes', async (ctx) => {
  // code which sends a reminder
});
```

### Create a Schedule with multiple middleware/handlers

```javascript
import { schedule } from '@nitric/sdk';

// Create a middleware to handle report generation
async function generateReport(ctx: faas.EventContext): Promise<void> {
  // code which generates a report
}

// Create a middleware to handle notifications
async function sendNotification(ctx: faas.EventContext): Promise<void> {
  // code which sends a notification
}

// Create a schedule that runs every 7 days
schedule('release').every('7 days', generateReport, sendNotification);
```