Nitric's API Gateway resource provides a convenient option for configuring and deploying RESTful APIs in front of your application's [Functions](./functions)/[Containers](./containers). These APIs are then deployed to the managed API Gateway services of each supported cloud provider to ensure their reliability, scalability and security.

APIs are defined using [Open API 3.0](https://swagger.io/specification/) specifications, ensuring they conform to a standard shared by many providers.

To improve application security, the [Functions](./functions) & [Containers](./containers) in a Nitric application aren't exposed publicly when they're deployed to a cloud provider. Instead, they can to be exposed via an [Entrypoint](./entrypoints) an API or both. When combining related [Functions](./functions) into a cohesive API, an API Gateway is often the best choice to aggregate these services, secure them and map their behavior onto API paths and HTTP Methods.

## How it works

Nitric uses the standard Open API spec with `x-nitric-target` extensions, that are included at the operation level of the spec.

Example extension definition:

```yaml
x-nitric-target:
	name: my-function-name
	type: function
```

> Currently, only [Functions](./functions) & [Containers](./containers) are supported as targets for Nitric APIs

## Defining an API

To add an API to an existing application, create or update the `apis` section of the project stack definition, e.g. `nitric.yaml`. The following is an example of a simple API for a GET request to an `example` function.

<CodeExamples
	languages={[
	{
		label: "nitric.yaml",
		value: "stack"
	},
	{
		label: "api.yaml",
		value: "api"
	}
	]}
	defaultLang="node"
>

<CodeExample lang="stack">

```yaml
functions:
	example:
		handler: functions/handler.ts

apis:
  # 👀 APIs reference a file containing an OAI3 document
	example: example-api.yaml
```

</CodeExample>

<CodeExample lang="api">

```yaml
openapi: 3.0.0
info:
  version: 1.0.0
  title: Sample API
  description: A sample API to illustrate OpenAPI concepts
paths:
  /exampleapi/example:
    get:
      operationId: handler
      # 👀 Note the use of extension here to target the
      # service defined above
      x-nitric-target:
        name: example
        type: function
      description: handles the function
      responses:
        '200':
          description: Successful response
```

</CodeExample>
</CodeExamples>

### Binding API operations to services

In the above example, the API operation `GET: /exampleapi/example` has been bound to a nitric function using an `x-nitric-target` property on the operation. The result of this configuration is that all HTTP requests to `GET: /exampleapi/example` will be forwarded to the `example` function from the API Gateway once deployed.

The x-nitric-target extension consists of the following properties:

| Property | Value    | Description                                                                                                                                                             |
| -------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name     | `string` | The name of another resource in the stack definition, such as a function. Requests to the API operation will be forwarded to this resource to be processed.              |
| type     | `string` | The type of resource used for this target. This value must match a supported top level resource type in the stack definition. Currently supported values are: `functions`, `containers` |

## Testing your API

The easiest way to test your API is running it locally.

```bash
nitric run
```

In the output you should see the local port mapping for your configured APIs:

```
Api         Port
<api-name>  <port>
```

This will be accessible via `localhost`, you can try invoking your API using curl, or an API testing tool such as postman.

Curl example for above definition using nitric run:
`curl http://localhost:<port>`