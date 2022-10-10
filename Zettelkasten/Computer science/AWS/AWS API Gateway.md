222022-07-27

Status: #CourseNotes 

Tags: #AWS 

# AWS API Gateway

**What is Amazon API Gateway?**

Amazon API Gateway is an AWS service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs at any scale. API developers can create APIs that access AWS or other web services, as well as data stored in the AWS Cloud. As an API Gateway API developer, you can create APIs for use in your own client applications. Or you can make your APIs available to third-party app developers**.**

API Gateway creates RESTful APIs that:

-   Are HTTP-based.
    
-   Enable stateless client-server communication.
    
-   Implement standard HTTP methods such as GET, POST, PUT, PATCH, and DELETE.
    



### **API Gateway REST APIs**

API Gateway supports multiple types of APIs, in this course we will be focusing on REST APIs. 

A REST API in API Gateway is a collection of resources and methods that are integrated with backend HTTP endpoints, Lambda functions, or other AWS services. You can use API Gateway features to help you with all aspects of the API lifecycle, from creation through monitoring your production APIs.

API Gateway REST APIs use a request/response model where a client sends a request to a service and the service responds back synchronously. This kind of model is suitable for many different kinds of applications that depend on synchronous communication.

API Gateway REST APIs have multiple configurable pieces:![API Gateway](https://courses.edx.org/assets/courseware/v1/cb659a5a4eeefe501042bc291cb0fc05/asset-v1:AWS+OTP-AWSD12+1T2022a+type@asset+block/image_API_Gateway.png)

![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/ld5LDEK8RuyeSwxCvHbsCQ_707f9991c6824e77b2fd1bf115e659b2_diagram1.png?expiry=1600300800000&hmac=KDT-QzF-B56RHEig_32SE81j9ce3XgWxuXXh_TpxNgY)

The diagram above shows what steps a client call makes to the API to access backend resources. A Method resource is integrated with an Integration resource. Both consist of a request and one or more responses. The method request takes the client input, and optionally validates it if configured - either against JSON Schema models or it checks the required request parameters in the URI, query string, and headers of an incoming request are included and non-blank. 

If the validation fails, API Gateway immediately fails the request, returns a 400 error response to the caller, and publishes the validation results in CloudWatch Logs. This reduces unnecessary calls to the backend.

Then, the client input is passed to the back end through the integration request. The integration request is where you configure what backend resource the API will be passing the client input to. This is also where you perform any mappings or data transformations potentially using VTL Mappings.

The request then gets passed to the backend resource.

A method response returns the output from the backend to the client through an integration response. You can configure data mappings on the response from the backend at the integration request level. An integration response is an HTTP response encapsulating the backend response. You can map backend responses to specific HTTP codes.

A method request is embodied in a Method resource, whereas an integration request is embodied in an Integration resource. On the other hand, a method response is represented by a MethodResponse resource, whereas an integration response is represented by an IntegrationResponse resource.

### **Request Validation**

API Gateway can perform basic validation. This enables you, the API developer, to focus on app-specific deep validation in the backend. You can offload basic validation to API Gateway. For the basic validation, API Gateway verifies either or both of the following conditions:

The required request parameters in the URI, query string, and headers of an incoming request are included and non-blank.

The applicable request payload adheres to the configured JSON schema request model of the method.

Validation is performed in the Method Request and Method Response of the API. You can associate models to validate against at the Method Request or Method Reponse level for each HTTP method. This means that different HTTP methods under a resource can use different models.


### **Models**

A model in API Gateway allows you to define a schema for validating requests.

The model is used to define the format of the incoming data on the Method Request, or the format of the outgoing data on the Method Response. 


### **Mappings**

Mappings are templates written in Velocity Template Language (VTL) that you can apply to the Integration Request or Integration Response of a REST API. The mapping template allows you to transform data, including injecting hardcoded data, or changing the shape of the data before it passes to the backing service or before sending the response to the client. 

You can access information on the payload of the request and other data in your mapping template by using variables. 

### **Stage Variables**

Stage variables are name-value pairs that you can define as configuration attributes associated with a deployment stage of a REST API. They act like environment variables and can be used in your API setup and mapping templates.

With deployment stages in API Gateway, you can manage multiple release stages for each API, such as alpha, beta, and production. Using stage variables you can configure an API deployment stage to interact with different backend endpoints. 

You can reference stage variables in Mapping templates through the use of $stageVariables. 

### **API Gateway Stages and Deployment**

Once you create a REST API in API Gateway, it doesn’t automatically become available to invoke. You need to publish the API first. In API Gateway, you publish the API to a stage.

A stage is a named reference to a deployment, which is a snapshot of the API. You use a Stage to manage and optimize a particular deployment. For example, you can configure stage settings to enable caching, customize request throttling, configure logging, define stage variables, or attach a canary release for testing.

Every time you make a change to your API, you must deploy it to a stage for that change to go live. You can host multiple versions of your API simultaneously by deploying changes to different stages. 

Using stages is perfect for setting up dev, qa, and production environments for you API. You can deploy your API to the appropriate stage as it moves through the software development lifecycle.



### **Invoking your API** 

Once an API is published to a stage, an invoke URL is given to the API for that specific stage. Each stage gets it’s own invoke URL. 

### **Amazon API Gateway Proxy for AWS Services**

One API Gateway integration type is the AWS integration type. This allows you to proxy other AWS services with API Gateway.

Each method you setup for a reason using an AWS integration type would expose one API or AWS service action. Because the AWS API will need the data in a specific format that your clients likely won’t be using, you must configure both the integration request and integration response and set up necessary data mappings from the method request to the integration request, and from the integration response to the method response. This will transform the request and response to adhere to the specifications needed for the AWS API.

Read more about API Gateway integration types here: [https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-integration-types.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-integration-types.html)

### **Amazon API Gateway HTTP APIs**

HTTP APIs are designed for low-latency, cost-effective AWS Lambda proxy and HTTP proxy APIs. HTTP APIs support OIDC and OAuth 2.0 authorization, and come with built-in support for CORS and automatic deployments. Previous-generation REST APIs currently offer more features, and full control over API requests and responses.

Read about how to choose between REST and HTTP APIs here: [https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html)


___
# References
Read more about Developing REST APIs with API Gateway at: [https://docs.aws.amazon.com/apigateway/latest/developerguide/rest-api-develop.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/rest-api-develop.html)

Read more about Amazon API Gateway at: [https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)

Read more about Request Validation at: [https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-method-request-validation.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-method-request-validation.html)

For instructions on how to create a Model click here: [https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-create-model.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-create-model.html)

Read about the variables that are available to use in your VTL mappings here: [https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-mapping-template-reference.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-mapping-template-reference.html)

Read more about stageVariables here: [https://docs.aws.amazon.com/apigateway/latest/developerguide/stage-variables.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/stage-variables.html)

Read more about how to use $stageVariables and other variables and functions you can use in your Mapping templates here: [https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-mapping-template-reference.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-mapping-template-reference.html)

Read more about stages at: [https://docs.aws.amazon.com/apigateway/latest/developerguide/rest-api-publish.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/rest-api-publish.html)

If you are interesting in using custom domains for the invoke url, click here: [https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-custom-domains.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-custom-domains.html)