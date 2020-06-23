# REST Builder

## Introduction

REST Builder provides an easy way to develop a REST API for your new or existing services. This services can be invoke through HTTP requests from external systems. Decoupling your service with a REST API gives you flexibility to integrate with different front-end technologies, mobile apps and 3rd party or legacy systems.

## What it is REST Builder?

REST Builder is a code generation tool built by Liferay that allows developers to define custom REST APIs following the [OpenAPI Specification (OAS)](https://www.openapis.org/) in a Design First approach.

Developers can define the API's contract using OpenAPI documents to describe what the API does and what resources handles in a language agnostic and human readable way, allowing both, machines and humans, to understand what the API is supposed to do.

REST Builder generates the API resources model layer and the API implementation layer using the JAX-RS specification, part of the JAVA EE standards. The generation tool implements all the necessary code to access the API defined in the API contract and let the developer focus in the service business logic. 

REST Builder can also generate an API client implementation to communicate with the REST API and an integration test module to test the API.

## Project Structure

REST Builder tool generate code splitted in multiple modules in order to offer a clean and decoupled implementation of a REST API.

The default project structure is a parent folder with the name of the REST API, usually the artifact identifier, containing a subfolder with the API module, with -api suffix, and a subfolder with the implementation module, with the -impl sufix. If REST API client and integration tests are configured in the REST Configuration file, the parent folder should contain two additional subfolders with -client and -test suffixes, where the client and test code will be generated.

Each module should contain the usual module configuration files: bnd.bnd and build.gradle or pom.xml depending on the desired build mechanism.

Last, the implementation module should include the REST configuration file and OpenAPI documents needed by the generation tool.

The resulting folder structure should be similar to the following:

<img src="folder_structure.png" alt="Example of the REST Builder folder structure" width="220" style="display: block;"/>

## Defining the OpenAPI document and REST Configuration file

In this section you'll learn how to configure the REST Builder generation with the REST Configuration file and describe your API using the OpenAPI document (or set of documents).

### REST Configuration file

REST Configuration file allows you to provide required parameters to REST Builder and configuration the resulting generated code.

The REST configuration file has YAML format and should be located inside the implementation module with name "rest-config.yaml" by default.

The configuration parameters are:
| Parameter | Description | Required | Default value |
| --------- | ----------- | :------: | ------------: |
| apiDir    | Directory of the API module | true | |
| apiPackagePath | API package name | true | |
| application | [Application](####application-object) information | true | |
| author | Author | true | |
| clientDir | Directory of the API module | false | |
| clientMavenGroupdId | Group identifier of the client module | false | |
| forcePredictableContentApplicationXML | Force the usage of XML media type in operation request bodies and responses. If no XML definition is provided, JSON definition will be used | false | true |
| generateBatch | Generate batch endpoints | false | true |
| generateGraphQL | Generate GraphQL endpoints | false | true |
| generateREST | Generate REST endpoints | false | true |
| implDir | Directory where the generated code will be stored inside each module | false | src/main/java |
| licenseName | Application license name | false | Apache 2.0 |
| licenseUrl | License URL | false | http://www.apache.org/licenses/LICENSE-2.0.html |
| testDir | Directory of the integration test module | false | |
| warningsEnabled | Enable warnings during the REST Builder execution | false | true |

#### Application object
The Application object parameters are:
| Parameter | Description | Required | Default value |
| --------- | ----------- | :------: | ------------: |
| baseUri    | Application base URI | true | |
| className | Name of the class with the JAX-RS application | true | |
| name | Application name | true | |
| security | [Security](####security-object) information | false | |

#### Security object
The Security object parameters are:
| Parameter | Description | Required | Default value |
| --------- | ----------- | :------: | ------------: |
| basicAuth | Basic authentication | false | |
| guestAllowed | Allow guest access | false | |
| oAuth | oAuth authentication | false | |

An example of a valid REST Configuration file is:
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

The OpenAPI Document describes what the API does and which are the resources to interact with. REST Builder is compatible with the OAS 3.*.* versions and support OpenAPI documents in YAML format.

## Using REST Builder template

## Running REST Builder and understanding generating code

## Implementing REST API
