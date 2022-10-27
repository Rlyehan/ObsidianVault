222022-10-27

Style: 

Domain:

Tags:

# Distributed Tracing and [[Observability]] for Azure functions

To demonstrate how to implement custom distributed tracing and observability practices with Azure Functions, I’ll use a common scenario, the publishing and consuming of user update events. Think of a HR or CRM system pushing user update events via webhooks for downstream systems to consume. To better illustrate the scenario, let’s follow a high-level sequence diagram that has the following participants:

-   **Webhook** - an HR or CRM system pushing user update events via webhooks, which sends an array of user events and expects a HTTP response.
-   **User Updated Publisher Azure Function** - receives the HTTP POST request from the webhook, validates the batch message, splits the message into individual event messages, publishes the messages into a Service Bus queue, and returns the corresponding HTTP response to the webhook.
-   **User Updated Service Bus queue** - receives and stores individual user event messages for consumers.
-   **User Updated Subscriber Azure Function** – listens to messages in the queue, performs the required validations and message processing, and delivers the message to the target system.
-   **Target System** – any system that needs to be notified when users are updated, but requires an integration layer to do some validation, processing, transformation, and/or custom delivery.

![[Pasted image 20221027144006.png]]

1. A webhook pushes user update events in batch to the User Updated Publisher Azure Function (publisher function) via a HTTP post.
2.  The publisher function validates the request payload.
3.  If the request payload is invalid: The publisher function returns HTTP 400 – Bad Request to the webhook.
4.  If the request is valid: The publisher function splits the body into individual event messages ([splitter pattern](https://platform.deloitte.com.au/articles/enterprise-integration-patterns-on-azure-routing#splitter)).
5.  For each message event in the batch: The publisher function sends the user event message to the Service Bus queue.
6.  If all event messages were successfully delivered: The publisher function returns HTTP 202 Accepted to the webhook.
7.  In the case of any exception: The publisher function returns HTTP 500 – Internal Server Error to the webhook.

Asynchronously (temporal decoupling provided by Service Bus queues), and for each message available in the queue:

8.  The User Updated Subscriber Azure Function (subscriber function) gets triggered by the user event message in the queue.
9.  The subscriber function tries to deliver the message to the target system via a request.
10.  The target system returns a response with the update request result.
11.  If the update operation succeeded: The subscriber function completes the message, so it is deleted from the queue.
12.  If the update operation was not successful, the message becomes available again for a retry after the message lock expires. When all attempts are exhausted, the message is dead lettered by the queue.


## Publisher Interface Detailed Design

Let’s now consider how we are going to be implementing this in more detail as part of our integration interfaces. The _BatchPublisher_ and _Publisher_ spans will be implemented as one publisher interface/component. So now let’s cover how we want to implement the tracing and observability practices in this interface. We will use the sequence diagram below, which has the following participants:

-   **Webhook** - an HR or CRM system pushing user update events via webhooks, which sends an array of user events and expects a HTTP response.
-   **User Updated Publisher Azure Function** - receives the HTTP POST request from the webhook, validates the batch message, splits the message into individual event messages, publishes the messages into a Service Bus queue, returns the corresponding HTTP response to the webhook, sends the relevant tracing logs to the Diagnostics Logs, and archives the initial request to Azure Storage.
-   **User Updated Service Bus queue** - receives and stores individual user event messages for consumers.
-   **Diagnostic Logs in Application Insights -** captures built-in telemetry and custom tracing produced by the Azure Functions.
-   **Storage Archive Azure Storage blob** - stores request payloads.
![[Pasted image 20221027144209.png]]
## Subscriber Interface Detailed Design

The _Subscriber_ span will be implemented in a subscriber interface. The sequence diagram below depicts the tracing and observability practices in this interface, and includes the participants as follows:

-   **User Updated Service Bus queue** - receives and stores individual user event messages for consumers.
-   **User Updated Subscriber Azure Function** – listens to messages in the queue and subscribes to messages, performs the required validations against the target system - to check whether it is a valid payload, if it is not a [stale message](https://platform.deloitte.com.au/articles/enterprise-integration-patterns-on-azure-endpoints#stale-message), and if all message dependencies are available - tries to deliver the message to the target system, and sends the relevant tracing events to the Diagnostics Logs.  
-   **Target System** – any system that needs to get notified when users are updated, but requires an integration layer to do some validation, processing, transformation, and/or custom delivery.
-   **Diagnostic Logs in Application Insights -** captures built-in telemetry and custom tracing produced by the Azure Functions.

![[Pasted image 20221027144528.png]]







___
# References
[Site Unreachable](https://engineering.deloitte.com.au/articles/distributed-tracing-and-observability-with-azure-functions-1-intro)