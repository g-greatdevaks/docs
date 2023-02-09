---
title: api.route.get()
description: Register a handler for HTTP GET requests to the route.
---

Register a handler for HTTP GET requests to the route.

```csharp
using Nitric.Sdk;

var route = Nitric.Api("main").Route("/customers");

route.Get(context => {
    // Construct a response for all incoming HTTP requests
    var responseBody = new Dictionary<string, string>();
    context.Res.Json(responseBody);

    return context;
});

Nitric.Run();
```

## Parameters

---

**match** required `string`

The path matcher to use for the route. Matchers accept path parameters in the form of a colon prefixed string. The string provided will be used as that path parameter's name when calling middleware and handlers. See [create a route with path params](#create-a-route-with-path-params)

---

**...middleware** required `Middleware<HttpContext>` or `Func<HttpContext, HttpContext>`

One or more middleware functions to use as the handler for HTTP requests. Handlers can be sync or async.

---

## Examples

### Register a handler for GET requests

```csharp
using Nitric.Sdk;

var route = Nitric.Api("main").Route("/customers");

route.Get(context => {
    // Construct a response for all incoming HTTP requests
    var responseBody = new Dictionary<string, string>();
    context.Res.Json(responseBody);

    return context;
});

Nitric.Run();
```

### Chain functions as a single method handler

When multiple functions are provided they will be called as a chain. If one succeeds, it will move on to the next. This allows middleware to be composed into more complex handlers.

```csharp
var route = Nitric.Api("main").Route("/customers");

route.Get((context, next) => {
    var user = context.Req.PathParams["userId"];

    // Validate the user identity
    if (user != "1234")
    {
        context.Res.Text($"User {user} is unauthorised");
        context.Res.Status = 403;

        // Return prematurely to end the middleware chain.
        return context;
    }

    // Call next to continue the middleware chain.
    return next(context);
  }, (context, next) => {
    var user = context.Req.PathParams["userId"];

    context.Res.Text($"Getting {user}");

    return next(context);
  }
);

Nitric.Run();
```