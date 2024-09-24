# Godspeed Express Plugin

Welcome to the [Godspeed](https://www.godspeed.systems/) Express Plugin! 🚀

## Introduction

The Godspeed Express Plugin is an integral part of the Godspeed framework, designed to facilitate the integration of event-driven and serverless functionalities into your projects. This plugin leverages the popular Express.js framework to handle HTTP events, making it easy to define event subscriptions and process incoming events.

## How to Use
- Create a godspeed project from the CLI and by default the Express plugin is integrated into your project if not, add the plugin from the CLI and select the `@godspeedsystems/plugins-express-as-http` to integrate the plugin.
```
> godspeed plugin add
       ,_,   ╔════════════════════════════════════╗
      (o,o)  ║        Welcome to Godspeed         ║
     ({___}) ║    World's First Meta Framework    ║
       " "   ╚════════════════════════════════════╝
? Please select godspeed plugin to install: (Press <space> to select, <Up and Down> to move rows)
┌────┬───────────────────────────────────┬─────────────────────────────────────────────────────────────────┐
│    │ Name                              │ Description                                                     │
├────┼───────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
│ ❯◯ │ express-as-http                   │ Godspeed event source plugin for express as http server         │
├────┼───────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
│  ◯ │ aws-as-datasource                 │ aws as datasource plugin for Godspeed Framework                 │
├────┼───────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
│  ◯ │ mailer-as-datasource              │ mailer as datasource plugin for Godspeed Framework              │
├────┼───────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
│  ◯ │ excel-as-datasource               │ excel as datasource plugin for Godspeed Framework               │
├────┼───────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
│  ◯ │ kafka-as-datasource-as-eventsource│ kafka as datasource-as-eventsource plugin for Godspeed Framework│
└────┴───────────────────────────────────┴─────────────────────────────────────────────────────────────────┘
```
- You will find the files in your project related to the Express plugin at `src/eventsources/types/express.ts` and `src/eventsources/http.yaml`.

### express.ts

```typescript
import { ExpressEventSource } from '@godspeedsystems/plugins-express-as-http';
export default ExpressEventSource;
```

### Express config (src/eventsources/http.yaml)

```yaml
type: express
port: 3000
```
### Express event (src/events/sample.yaml)
```yaml
http.get./sample_api:
    fn: sample      #redirects to src/functions/sample.yaml
    body: 
      content:
        application/json:
          schema:
            type: object
            properties:
              name: 
                type: string
              message: 
                type: string                         
    params:     
      - in: query
        name: user
        required: true  
        schema: 
          type: string   
    responses:      
      200:
        content:
          application/json:
            schema:
              type: string
```
```yaml
http.<method>./<endpoint_url>:
    fn: <function_yaml>
    body:
    params:
    responses:
```
- The event YAML defines properties for handling specific HTTP requests within the Express app. In the YAML, `<method>` should be replaced with actual HTTP methods such as `GET, POST, PUT, or DELETE`, specifying how the app handles those requests. The `<endpoint_url>` field should contain the API URL for the respective HTTP route.
- A function will be triggered on sending a request to the respective url. The functions are created under `src/functions/`.

### Function to be called (src/functions/sample.yaml)
```yaml
summary:
description:
tasks:
    - id: example
      fn: com.gs.return #its an inbuilt function
      args: |
        <%"Hello "+inputs.query.user+". This is a message from body "+inputs.body.message%>
```

## How It Helps

The Godspeed Express Plugin provides the following benefits:

1. **Express Integration:** The plugin abstracts away the complexities of setting up an Express.js application, making it effortless to define routes and handlers for incoming HTTP events.

2. **Event Subscription:** Developers can easily subscribe to specific HTTP events by defining routes and handlers using a uniform API.

3. **Customizable Configuration:** The plugin allows for easy configuration of Express settings, such as port, request body limits, and file size limits.

4. **Integration with Godspeed Core:** The plugin works seamlessly with the Godspeed Core library, enabling the processing of cloud events and facilitating event-driven architecture.

5. **File upload feature:** The Express plugin allows you to upload your files using postman

## How file upload feature works

### Uploading file

The Express plugin allows you to upload your files

### Steps to use fileupload feature

Framework will give you below folder structure.

```
    .
    ├── src
        ├── datasources
        │   ├── types
        │   |    └── axios.ts
        |   |
        │   └── api.yaml
        │
        ├── events
        |   |
        │   └── helloworld.yaml
        |
        ├── eventsources
        │   ├── types
        │   |    └── express.ts
        |   |
        │   └── http.yaml
        |
        └── functions
            |
            └── helloworld.yaml
```

The default file size accepted is 50MB. If you wish to specify a custom file size, you can modify the value in `"./src/eventsources/http.yaml`".

### Configuration( src/eventsources/http.yaml )
```yaml
type: express
port: 3003
request_body_limit: 30000000
file_size_limit : 30000000
docs:
  endpoint: /
  info: 
    version: 0.0.1
    title: 'Godspeed: Sample Microservice'
    description: Sample API calls demonstrating the functionality of Godspeed framework
    termsOfService: 'http://swagger.io/terms/'
    contact:
      name: Mindgrep Technologies Pvt Ltd
      email: talktous@mindgrep.com
      url: 'https://docs.mindgrep.com/docs/microservices/intro'
    license:
      name: Apache 2.0
      url: 'https://www.apache.org/licenses/LICENSE-2.0.html'
  servers:
    - url: https://api.example.com:8443/api
      description: staging
```

- The file size may vary from the original size and could potentially increase in kilobytes(KB) after uploading. Please take this into consideration when setting your file size.


### Example Event

```yaml
http.post./helloworld:
  fn: helloworld
  body:
    content:
      multipart/form-data:
        schema:
          type: object
          properties:
            fileName:
              type: string
              format: binary


  responses:
    200:
      content:
        application/json:
          schema:
            type: object

```

### Example workflow

```yaml
summary: Returning a file
tasks:
  - id: first_task
    fn: com.gs.return
    args: <% inputs.files.name %>

```

### Example success response

![image](https://res.cloudinary.com/dzdcjchdc/image/upload/v1704369051/Screenshot_from_2024-01-04_17-20-32_dfzirt.png)

- Upon successful upload of the file in Postman, an autogenerated "tmp" folder is created within the scaffolding directory, containing the uploaded file.

## Setting Base Url:
- The base url is set in datasources/api.yaml

```
type: axios
base_url: https://httpbin.org
```

## Configuring jwt

You can configure JWT settings within the eventsources/http.yaml. Here's an example of such a configuration:

```
type: express
jwt:
  issuer: <#config.issues#> # must be equal to the key iss in your jwt token
  audience: <#config.audience#> #must be equal to the key aud in your jwt token
  secretOrKey: <#config.secret#>
```
## Plugin Explaination

This plugin is designed to integrate with the Godspeed framework and provides functionality related to event sources using Express.js, a popular Node.js web application framework. It allows you to create event sources that can listen for incoming HTTP requests and trigger actions based on those requests.

## Plugin Components

The plugin consists of several key components:

### 1. `EventSource` Class

- This class extends `GSEventSource`, a base class provided by the Godspeed framework for creating event sources.

- It initializes an Express.js server to listen for incoming HTTP requests based on the provided configuration options.

- The `subscribeToEvent` method is used to define how the plugin should respond to specific HTTP routes. It maps incoming HTTP requests to Godspeed Cloud Events, processes them using a provided function, and sends a response.

- The `createGSEvent` static method is used to create a Godspeed Cloud Event from an incoming Express.js request.

### 2. Constants

- `SourceType`: A constant representing the source type of the plugin, which is 'ES' (event source).

- `Type`: A constant representing the loader file of the plugin. The final loader file will be located in the 'types' directory and named `${Type.js}`, where `Type` is 'express' in this case.

- `CONFIG_FILE_NAME`: In the context of an event source, this constant also serves as the event identifier. For a data source, it works as the data source name. In this plugin, it is set to 'http'.

- `DEFAULT_CONFIG`: A default configuration object with options like the port number for the Express.js server.


## Conclusion

Our Express plugin is a powerful addition to the Godspeed framework, allowing you to seamlessly integrate and manage event sources within your applications. With this plugin, you can effortlessly handle HTTP requests, define routes, and trigger actions, making it an essential tool for building robust and responsive applications.

We value your feedback and contributions. If you have questions, suggestions, or encounter any issues while using the plugin, please don't hesitate to reach out to us. Your insights and ideas can help us improve and enhance this plugin for the entire community.

We're excited to see what you'll create with the Express plugin, and we look forward to collaborating with you to make your projects even more successful. Happy coding!

### Get in Touch

- [Discord](https://discord.com/invite/mjBa3RvTP5)
- [Plugin Repository](https://github.com/godspeedsystems/gs-plugins/tree/main/plugins/express-as-http)
- [Issue Tracker](https://github.com/godspeedsystems/gs-plugins/issues)

## Thank You For Using Godspeed 
