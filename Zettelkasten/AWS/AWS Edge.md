222022-08-09

Style: 

Domain:

Tags:

# AWS Edge

### **AWS Global Infrastructure: Edge Locations**

AWS Edge Locations are sites that Amazon CloudFront uses to cache copies of your content for faster delivery to users around the world. By deploying content to the edge locations, the latency in your application can be reduced for end users since they are accessing the resources at a location that is physically closer to them than the region you hosted your resources in originally. 

Read more about Amazon CloudFront and Edge Locations here: [https://aws.amazon.com/cloudfront/](https://aws.amazon.com/cloudfront/)

### **Amazon API Gateway Response Caching**

You can enable API caching in Amazon API Gateway to cache your endpoint's responses. With caching, you can reduce the number of calls made to your endpoint and also improve the latency of requests to your API.

When you enable caching for a stage, API Gateway caches responses from your endpoint for a specified time-to-live (TTL) period, in seconds. API Gateway then responds to the request by looking up the endpoint response from the cache instead of making a request to your endpoint. The default TTL value for API caching is 300 seconds. The maximum TTL value is 3600 seconds. TTL=0 means caching is disabled.

To read more about Response Caching click here: [https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-caching.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-caching.html)

### **AWS Lambda @ Edge**

Lambda@Edge is a feature of Amazon CloudFront that lets you run code closer to users of your application, which improves performance and reduces latency. With Lambda@Edge, you don't have to provision or manage infrastructure in multiple locations around the world. You pay only for the compute time you consume - there is no charge when your code is not running.

Read about use cases of Lambda@Edge here:  [https://aws.amazon.com/lambda/edge/#Website_Security_and_Privacy](https://aws.amazon.com/lambda/edge/#Website_Security_and_Privacy)

Read more general information about Lambda@Edge here: [https://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html)



___
# References
