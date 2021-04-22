# REST Builder

## Introduction

REST Builder provides an easy way to develop your **GraphQL and REST API** for your new or existing web services. This services can be invoke through HTTP requests from external systems. Decoupling your service with an API gives you flexibility to integrate with different front-end technologies, mobile apps and 3rd party or legacy systems.

## What it is REST Builder?

REST Builder is a **code generation tool** built by Liferay that allows developers to define custom APIs following the [OpenAPI Specification (OAS)](https://www.openapis.org/) in a **Design First** approach.

Developers can define the API's contract using OpenAPI documents to describe what the API does and what resources handles in a language agnostic and human readable way, allowing both, machines and humans, to understand what the API is supposed to do.

REST Builder generates the API resources model layer and the API implementation layer using the JAX-RS specification, one of the JAVA EE standards, and the GraphQL servlet. The generation tool implements all the necessary code to access the API defined in the API contract and advanced features such as pagination, search, filter, batch support..., letting the developer focus in the service business logic, what makes this tool very powerful and valuable. 

REST Builder generate a JAVA client to communicate with the API and an integration test module to test the API from the outside as a external application.

## Project Structure

REST Builder tool generates code splitted in multiple modules in order to offer a clean and decoupled implementation of your GraphQL and REST APIs.

The default project structure is a parent folder with the name of the application, usually the artifact identifier, containing a subfolder with the API module, with -api suffix, and a subfolder with the implementation module, with the -impl sufix. If API client and integration tests are configured, the parent folder should contain two additional subfolders with -client and -test suffixes, where the client and test code will be generated.

Each module should contain the usual module configuration files: bnd.bnd and build.gradle or pom.xml depending on the desired build mechanism.

Finally, the implementation module should include the API configuration file and OpenAPI documents needed by the generation tool.

The resulting folder structure should be similar to the following:

<img src="folder_structure.png" alt="Example of the REST Builder folder structure" width="220" style="display: block;"/>

## Defining the OpenAPI document and API Configuration file

In this section you'll learn how to configure the REST Builder generation with the API Configuration file and describe your API using the OpenAPI document (or set of documents).

### API Configuration file

The API Configuration file allows you to provide the required parameters and configuration to REST Builder tool, on which the generated code will depend.

The API configuration file uses YAML format and, by default, is located inside the implementation module with the name *rest-config.yaml*.

The configuration parameters are:

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| apiDir | string | Directory of the API module | true |
| apiPackagePath | string | API package name | true |
| application | [Application Object](#application-object) | Information about the application | true |
| author | string | Author name | true |
| clientDir | string | Directory of the JAVA client module. If it is not set, the client will not be generated | false |
| clientMavenGroupdId | string | Group identifier of the client module | false |
| forceClientVersionDescription | boolean | Force adding a message to the description that reflects the last version of the client. Defaults to `true` | false |
| forcePredictableContentApplicationXML | boolean | Force the usage of XML media type in operation request bodies and responses. If no XML definition is provided, JSON definition will be used. Defaults to `true` | false |
| forcePredictableOperationId | boolean | Force setting a predictable operationId based on the path and the return type. Defaults to `true` | false |
| forcePredictableSchemaPropertyName | boolean | Force a consistent naming scheme for properties based on the schema returned. Defaults to `true` | false |
| generateBatch | boolean | Generate batch endpoints. Defaults to `true` | false |
| generateGraphQL | boolean | Generate GraphQL endpoints. Defaults to `true` | false |
| generateREST | boolean | Generate REST endpoints. Defaults to `true` | false |
| implDir | string | Directory where the generated code will be stored inside each module. Defaults to `src/main/java` | false |
| licenseName | string | Application license name. Defaults to `Apache 2.0` | false |
| licenseUrl | string | License URL. Defaults to `http://www.apache.org/licenses/LICENSE-2.0.html` | false |
| testDir | string | Directory of the integration test module. If it is not set, the integration tests will not be generated | false |
| warningsEnabled | boolean | Enable warnings during the REST Builder execution. Defaults to `true` | false |

#### Application Object
The Application object parameters are:

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| baseUri    | string | Application base URI | true |
| className | string | Name of the class with the JAX-RS application | true |
| name | string | Application name | true |
| security | [Security Object](#security-object) | Information about application security | false |

#### Security Object
The Security object parameters are:

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| basicAuth | string | Basic authentication | false |
| guestAllowed | string | Allow guest access | false |
| oAuth | string | oAuth authentication | false |

### An API Configuration example

An example of a valid API Configuration file is:
```yaml
apiDir: "../guestbook-api/src/main/java"
apiPackagePath: "com.liferay.guestbook"
application:
    baseURI: "/guestbook"
    className: "GuestbookApplication"
    name: "Guestbook"
author: "John Doe"
clientDir: "../guestbook-client/src/main/java"
testDir: "../guestbook-test/src/testIntegration/java"
```

### OpenAPI Document

The OpenAPI Document describes what the API does and which are the resources to interact with. REST Builder is compatible with the OAS 3.\*.\* versions and support OpenAPI documents in YAML format.

The OpenAPI document uses YAML format and, by default, should be located inside the implementation module with the name *rest-openapi.yaml*

The  most common fields are defined in the table below. For an exhaustive and detailed list of fields, please refer to the [official OAS specification](https://swagger.io/specification/)

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| components | [Components Object](#components-object) | Contains the schema resources | false |
| info | [Info Object](#info-object)| Metadata about the API | true |
| openapi | string | The semantic version number of the OpenAPI Specification version used in the document | true |
| paths | [Paths Object](#paths-object) | The available paths and operations for the API | true |

#### Components Object

The components field contains objects to be referenced from other places in the document by the assigned key. It usually contains the schema of the objects used in the API:

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| schema | Map[String, [Schema Object](#schema-object)] | A map with schema definitions | false |

#### Schema Object

The schema is defined by description, format, type and a set of properties that are also [Schema Objects](#schema-object). To read a complete definition of the schema object refer to the specification.

#### Info Object

The info object is composed by:

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| title | string | The title of the application | true |
| description | string | A short description of the application | false |
| license | [License Object](#licenseObject) | The license information for the exposed API | false |
| version | string | The version of the application | true |

#### License Object

The license object is defined by:

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| name | string | The license name | true |
| licenseUrl | string | A URL to the license used for the API | false |

#### Paths Object

The paths object holds the information relative to the individual endpoints and their operations:

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| /{path} | [Path Item Object](#path-item-object ) | Information about an individual endpoint. The field name must begin with a slash | false |

#### Path Item Object

The path item object is composed by:

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| summary| string | A summary, intended to apply to all operations in this path | false |
| description | string | A description, intended to apply to all operations in this path | false |
| get | [Operation Object](##operation-object) | A definition of a GET operation on this path | false |
| put | [Operation Object](##operation-object) | A definition of a PUT operation on this path | false |
| post | [Operation Object](##operation-object) | A definition of a POST operation on this path | false |
| delete | [Operation Object](##operation-object) | A definition of a DELETE operation on this path | false |
| patch | [Operation Object](##operation-object) | A definition of a PATCH operation on this path | false |

#### Operation Object

The operation object describes a single API operation on a path:

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| summary | string | A short summary of what the operation does | false |
| description | string | A description of the operation behavior | false |
| operationId | string | Unique string used to identify the operation. REST Builder use the operationId to uniquely identify an operation, therefore, it is recommended to follow common programming naming conventions | false |
| parameters | [[Parameter Object](#parameter-object)] | A list of parameters that are applicable for this operation | false |
| requestBody | [Request Body Object](#request-body-object) | The request body applicable for this operation | false |
| responses | [Responses Object](#responses-object) | The list of possible responses as they are returned from executing this operation | true |
| tags | \[string\] | A list of tags for the operation | false |

#### Parameter Object

The parameter object describes a single operation parameter. A unique parameter is defined by a combination of a name and location.

There possible parameter locations specified by the `in` field are:
* path - The parameter value is part of the operation's URL.
* query - The parameter is appended to the URL.
* header - Custom headers that are expected as part of the request.
* cookie - Used to pass a specific cookie value to the API.

The parameter is defined by:

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| name | string | The name of the parameter. Parameter names are *case sensitive* | true |
| in | string | One of the possible parameter locations explained in the previous list | true |
| description | string | A brief description of the parameter | false |
| required | boolean | Determines whether this parameter is mandatory | true |
| schema | [Schema Object](#schema-object) | The schema defining the type of the parameter | true |

#### Request Body Object

The request body object describes a single request body:

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| description | string | A brief description of the request body | false |
| content | Map[string, [Media Type Object](#media-type-object)] | The content of the request body | true |
| required | boolean | Determines if the request body is required in the request. Defaults to `false` | false |

#### Media Type Object

Each Media Type Object provides schema and examples for the media type identified by its key

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| schema | [Schema Object](#schema-object) | The schema defining the content of the request, response, or parameter | false |

#### Responses Object

A map from HTTP response code to the expected response. The Responses Object must contain at least one response code, and it should be the response for a successful operation call.
It is not necessary to cover all possible HTTP response codes but is expected to cover a successful operation response and any known errors.

The default may be used as a default response object for all HTTP codes that are not covered individually by the specification.

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| default | [Response Object](#response-object) | The documentation of responses other than the ones declared for specific HTTP response codes | false |
| {HTTP Status Code} | [Response Object](#response-object) | The expected response for that HTTP status code | false |

#### Response Object

The response object describes a single response from an API Operation

| Field Name | Type  | Description | Required |
| ---------- | :---: | ----------- | :------: |
| description | string | A short description of the response | true |
| content | Map[string, [Media Type Object](#media-type-object)] | A map containing descriptions of potential response payloads | false |

### An OpenAPI example

An example of a simple but valid OpenAPI document is shown below:

```yaml
components:
    schemas:
        Guest:
            properties:
                guestId:
                    format: int64
                    type: integer
                name:
                    type: string
            type: object
info:
    description: "Guestbook API"
    license:
        name: "Apache 2.0"
        url: "http://www.apache.org/licenses/LICENSE-2.0.html"
    title: "Guestbook"
    version: v1.0
openapi: 3.0.1
paths:
    /guests:
        get:
            description:
                Retrieves the guests
            operationId: getGuestsPage
            responses:
                200:
                    content:
                        application/json:
                            schema:
                                items:
                                    $ref: "#/components/schemas/Guest"
                                type: array
                        application/xml:
                            schema:
                                items:
                                    $ref: "#/components/schemas/Guest"
                                type: array
            tags: ["Guest"]
        post:
            description:
                Adds a new guest
            operationId: postGuest
            requestBody:
                content:
                    application/json:
                        schema:
                            $ref: "#/components/schemas/Guest"
                    application/xml:
                        schema:
                            $ref: "#/components/schemas/Guest"
            responses:
                200:
                    content:
                        application/json:
                            schema:
                                $ref: "#/components/schemas/Guest"
                        application/xml:
                            schema:
                                $ref: "#/components/schemas/Guest"
            tags: ["Guest"]
    /guests/{id}:
        get:
            description:
                Retrieves the guest with given identifier
            operationId: getGuest
            parameters:
                - in: path
                  name: id
                  required: true
                  schema:
                      type: string
            responses:
                200:
                    content:
                        application/json:
                            schema:
                                $ref: "#/components/schemas/Guest"
                        application/xml:
                            schema:
                                $ref: "#/components/schemas/Guest"
            tags: ["Guest"]
```

## Using Blade and Project Templates

[Blade CLI](https://help.liferay.com/hc/en-us/articles/360017880092-Blade-CLI) is a command tool to help creating a new Liferay module. It creates the folder structure and necessary files to kick-start your project, allowing the developer to select between Gradle or Maven build tools.

Blade offers many project templates, including REST Builder, providing a scaffolding to your API web service.

To create your API web service inside your Liferay Workspace with blade you should go to the modules folder and execute:
```
blade create --template rest-builder guestbook
```

This command will set your API web service folder structure, build script files of the selected build tool, and rest-config.yaml and rest-openapi.yaml with basic structure and parameters.

More detailed information can be found in [Blade CLI](https://help.liferay.com/hc/en-us/articles/360017880092-Blade-CLI) and [Project Templates](https://help.liferay.com/hc/en-us/articles/360018170431-Introduction-to-Project-Templates-) documentation.


## Running REST Builder and understanding the generated code

Once you have the folder structure set and the API configuration file and OpenAPI documents defined, you can run REST Builder tool to generate the scaffolding for your API web service. This can be done using Maven or Gradle REST Builder plugins.

\* *To find out which version of the Gradle and Maven plugins to use, go to Maven Central or Maven Repository to get the latest available version published.*

### Using Gradle

The REST Builder Gradle plugin lets you execute REST Builder to generate the source files of your API web service.

To use the REST Builder Gradle plugin you need to include it in your build.gradle file:
```
buildscript {
    dependencies {
        classpath group: "com.liferay", name: "com.liferay.gradle.plugins.rest.builder", version: "${REST.Builder.version}"
    }

    repositories {
        maven {
            url "https://repository-cdn.liferay.com/nexus/content/groups/public"
        }
    }
}

apply plugin: "com.liferay.portal.tools.rest.builder"

```

The plugin adds the `buildRest` task to your project. This task has two arguments:

| Parameter | Description | Required | 
| --------- | ----------- | :------: |
| copyrightFile | File with the Copyright header to be used in the generated code. Defaults to `null` | false |
| restConfigDir | Directory containing your API web service definition files, rest-config.yaml and rest-openapi.yaml. Usually the implementation module folder. Defaults to `${project.projectDir}` | false |

To run the task you need to use the Gradle Wrapper to execute `gradlew buildRest`. 

The task should finish successfully generating the source files of the configured modules. In case of error, a descriptive message should be provided to allow the developer to easily locate it and fix it.

### Using Maven

REST Builder is also available in Maven with the REST Builder Maven plugin.

To use the plugin you need to include it in the project's root pom.xml file:
```xml
<build>
    <plugins>
    ...
        <plugin>
            <groupId>com.liferay</groupId>
            <artifactId>com.liferay.portal.tools.rest.builder</artifactId>
            <version>${REST.Builder.version}</version>
            <configuration>
            </configuration>
        </plugin>
    ...
    </plugins>
</build>
```

The plugind adds the Maven Goal `build-rest:build` to run the code generation. There are two optional parameters to be added in the configuration section:

| Parameter | Description | Required |
| --------- | ----------- | :------: |
| copyrightFile | File with the Copyright header to be used in the generated code. Defaults to `null` | false |
| restConfigDir | Directory containing your API web service definition files, rest-config.yaml and rest-openapi.yaml. Usually the implementation module folder. Defaults to `${project.projectDir}` | false |

To run the task execute the Maven goal command with `mvn build-rest:build`. The goal should finish generating the source files or with a meaningful error.

### Understanding the generated code

The generated code is splitted at least in two modules, the API module and the implementation module.

#### API module

The API module contains the model of your API, your DTOs (Data Transfer Objects) and resources.

The DTOs represents the objects defined in the components section of the OpenAPI Document. They are meant to be transferred in the API request and responses. So, as a developer, you need to use it in your business logic to process the API requests and return them as response when it is needed.

The resources package contains the representation of the API resources correspondent to the paths section of the OpenAPI Document. These resources are implemented in the implementation module and mapped to the specified path.

The two packages containing DTOs and resources are exported to be used by other modules, as the implementation module.

#### The implementation module

Code generated in the implementation module consists of JAX-RS main entities, the resource endpoints and the GraphQL servlet, queries and mutations.

The jaxrs package contains the JAX-RS Application class that register the API web service in the portal.

The resource package contains the resource endpoints defined in the OpenAPI paths section. Each resource has a base implementation called Base\*ResourceImpl offering helper methods to implement the endpoint logic. This class is extended by the correspondent \*ResourceImpl class where the developer should add the business logic. There is also a OpenAPIResourceImpl class that registers an endpoint in `http://[host]:[port]/o/[APPLICATION_NAME]/[API_VERSION]/openapi.[type]` to obtain the OpenAPI document, the API contract, in JSON and YAML format.

The graphql package contains the GraphQL servlet and with the classes needed to access your API through GraphQL queries and mutations.

#### The client module

The client module will be generated if it is configured in the API configuration YAML file. It is a fully operative client for the API defined in your OpenAPI Document, with its own DTOs, resources, serializers, deserializers and the HttpInvoker class to make the HTTP requests.

#### The test module

The test module will be generated if it is configured in the API configuration YAML file. This module contains generated classes to test all the endpoints and methods defined in your API contract, making requests as a external service. It uses the client generated in the client module to make these requests.

Every endpoint has a Base\*ResourceTestCase class that uses a Template Pattern, providing the general structure to test each endpoint method and letting the developer to fill just the specific code related to the business logic in the \*ResourceTest class.


## Implementing the API

The entry points to implement your API are the \*ResourceImpl classes inside the implementation module. This is where you need to add your business logic to be executed when somebody access your API web service. 

It is important to remark that you should not add any code inside classes annotated as `@Generated` as these classes will be recreated each time REST Builder is executed. This can be necessary if you need to modify the API definition or expand it.

If test module is configured you should also add the correspondent code to test your logic in the *ResourceTest classes.

Once you finished you can deploy your API web service and use it in the URL:
```
http://[host]:[port]/o/[APPLICATION_NAME]/[API_VERSION]/
```

For example, using your Guestbook API web service locally will be in the URL:
```
http://localhost:8080/o/guestbook/v1.0
```

To be sure your API web service is correctly deployed you can use the OSGi console to list all the JAX-RS endpoints with the command `jaxrs:check`
