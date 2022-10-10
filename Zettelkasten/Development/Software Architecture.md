222022-09-07

Style: 

Domain: #Development 

Tags:

# Software Architecture

# What is Software architecture?

Its basically hwo we organize stuff when creatign software. Examples of "sutff":

-   **Implementation details** (that is, the folder structure of your repo)
-   **Implementation** **design** decisions (Do you use server side or client side rendering? Relational or non-relational databases?)
-   The **technologies** you choose (Do you use REST or GraphQl for your API? Python with Django or Node with Express for your back end?)
-   **System** **design** decisions (like is your system a monolith or is it divided into microservices?)
-   **Infrastructure** decisions (Do you host your software on premise or on a cloud provider?)


# Important Software Architecture Concepts

### Client-Server Model

As a generic desciption we can say that:

**Client-Server** is a model that structures the tasks or workloads of an application between a resource or service provider (server) adn a service or resource requester (client).

This is in use for most web apps today. We can think of the front end as a client that sends requests to the back-end, and the back-end is the server that delivers the requested resources.

Another important concept to know is that clients and servers are part of the same system, but each is an application/program on its own. Meaning they can be developed, hosted, and executed separately.

### APIs
We just mentioned that clients and servers are entities that communicate with each other to request things and respond to things. The way in which these two parts usually communicate is through an API.

An API is nothing more than a set of defined rules that establishes how an application can communicate with another. It's like a contract between the two parts that says "If you send A, I'll always respond B. If you send C, I'll always respond D..." and so on.

How we implement this is up to us and there are many different ways, like REST, GraphQL, and so on.

### Modularity

When we talk about "modularity" in software architecture, we refer to the practice of dividing big things into smaller pieces. This practice of breaking things down is performed to simplify big applications or codebases.

Modularity has the following advantages:

-   It's good for dividing concerns and features, which helps with the visualization, understanding, and organization of a project.
-   The project tends to be easier to maintain and less prone to errors and bugs when it's clearly organized and subdivided.
-   If your project is subdivided into many different pieces, each can be worked on and modified separately and independently, which is often very useful.


# Different Architecture styles

### Monolithic architecture

![[Pasted image 20220907090333.png]]


This kind of architecture is called a **monolith** because there's a single server application that is responsible for all the features of the system.

The main benefit of a monolithic design is its simplicity. The functioning of it and the set up required is simple and easy to follow, and this is why most applications start out in this way.


### Microservice Architecture

![[Pasted image 20220907090639.png]]

Above is a our new, microservice architecture for the app form before. We ran into issues with veritcal scaling our server to serve enough costumers. And we also started to add new features, making our codebase very complex.

To solve these prolmes we chose to switch to an microservice architecture. That means that we split up the tasks of our server from before into smaller pieces that run on their own servers, each one responsible for one feature. 
That means:

-   You can **scale particular services as needed**, instead of scaling the whole back end at once. Following our example, when we started to experience performance issues we vertically scaled our whole server – but actually the feature that requested the more resources was only the streaming. Now that we have the streaming feature separated into a single server, we can scale only that one and leave the rest alone as long as they keep working right.
-   Features will be more **loosely coupled**, which means we'll be able to develop and deploy them independently.
-   The **codebase** for each server will be much smaller and **simpler**. Which is nice for the dev folks that have been working with us from the start, and also easier and quicker for new developers to understand.

But all of these benefits also come with a cost. As you can see when comparing the charts for the two styles, while we where able to seperate the features out, to make that work we need all of our clients to communicate with all of our services as needed, increasing the complexity of the APIs manyfold.

Microservices is an architecture that is more complex to set up and manage, which is why it makes sense only for very big projects. Most projects will start out as monoliths and migrate to microservices only when needed for performance reasons.


### Back-End for Front-End (BFF)

One of the main challenges with microservices is the complexity of the communication. To stream line this we can implement an additional layer to take over the communication routing.

![[Pasted image 20220907091841.png]]


The benefit of the BFF pattern is that we get the benefits of the microservices architecture, without over complicating the communication with front-end apps.

### Load-balancers & horizontal scaling

We have scaled vertically as much as we could by implementing microservices to balance the use of our resources, but our app user base grows and we start running into performance issues again. 

Before we mentioned that **vertically scaling** means adding more resources (RAM, disk space, GPU, and so on) to a single server/computer. **Horizontally scaling** on the other hand, means setting up more servers to perform the same task.

So now instead of one server we set up three for example. But that alone doesn't help us if all the trafic still gets routed to one server. To properly distribute the load we want to make use of **load balancers.**

They intercept the trafic to our server and distribute it across all of our serves that it knows of. 

Another important fact is that we can extend horizontal scaling also to our databases. One way of implementing this is with a source-replica model, in which one particular source DB will receive all write queries and replicate it's data along one or more replica DBs. Replica DBs will receive and respond to all read queries.

The advantages of DB replication are:

-   Better performance: This model improves performance allows more queries to be processed in parallel.
-   Reliability and availability: If one of your database servers is destroyed or inaccessible for any reason, data is still preserved in other DBs.

So now our architecture might look a little like this:

![[Pasted image 20220907092535.png]]


# Infrastructure

Now that we know how we might want to organize our services, next we want to think about where all of that Infrastructure is going to be.


### On-premise
On premise means you own the hardware in which your app is running. The good thing about this option is that the company gets total control over the hardware. The bad thing is it requires space, time, and money.

One situation in which on premise servers still make sense is when dealing with very delicate or private information. Think about the software that runs a power plant, or private banking information, for example. Many of these organizations decide to have on premise servers as a way to have complete control over their software and hardware.

### Traditional server providers
These are companies that have servers of their own and they just rent them. You decide what kind of hardware you'll need for your project and pay a monthly fee for it (or some amount based on other conditions).

What's great about this option is that you don't need to worry about anything hardware-related anymore. The provider takes care of it, and as a software company you only worry about your main goal, the software.

Another cool thing is that scaling up or down is easy and risk free. If you need more hardware, you pay for it. And if you don't need it anymore, you just stop paying.

### In the cloud
And that's what cloud computing is. Using different services like **AWS** (Amazon web services), **Google Cloud**, or Microsoft **Azure**, we're able to host our applications in these companies' data centers and take advantage of all that computing power.

And there are different options for clooud hosting we can choose from.

**Traditional**:
The first way is to use them in a similar way you'd use a traditional server provider. You select the kind of hardware you want and pay exactly for that on a monthly basis.

**Elastic**:
The second way is to take advantage of the "elastic" computing offered by most providers. "Elastic" means that the hardware capacity of your application will automatically grow or shrink depending on the usage your app has.

The awesome thing about this is you can configure all this beforehand and not have to worry about it again. As the servers scale up and down automatically, you pay only for the resources you consume.

**Serverless**:
Following this pattern, you wont have a server that receives all requests and responds to them. Instead you'll have individual functions mapped to an access point (similar to an API endpoint).

What's very nice about serverless architecture is that you forget all about server maintenance and scaling. You just have functions that get executed when you need them, and each function is scaled up and down automatically as needed.


# Code Architecture

We talked about architcture from an Infrastructure point of view until now, but software achtitecture deals with more than just that. Next we look at how to strucutre our code.


### All-in-one-place folder structure
THis is the simplest approach, we have all of our code in one folder. There is nothing wrong in this approach, but problems can an dwill arise once our codebase becomes larger and more complex.

Following the modularity principle, a better idea is to have different folders and files for the different responsibilities and actions we need to perform.

### Layered folder structure
Layers architecture is about dividing concerns and responsibilities into different folders and files, and allowing direct communication only between certain folders and files.


![[Pasted image 20220907093642.png]]

-   The application layer will have the basic setup of our server and the connection to our routes (the next layer).
-   The routes layer will have the definition of all of our routes and the connection to the controllers (the next layer).
-   The controllers layer will have the actual logic we want to perform in each of our endpoints and the connection to the model layer (the next layer, you get the idea...)
-   The model layer will hold the logic for interacting with our mock database.
-   Finally, the persistence layer is where our database will be.

An important thing to keep in mind is that in these kind of architectures **there's a defined communication flow** between the layers that has to be followed for it to make sense.

This means that a request first has to go through the first layer, then the second, then the third and so on. No request should skip layers because that would mess with the logic of the architecture and the benefits of organization and modularity it gives us.

### Model View Controller structure
MVC is an architecture pattern that stands for **Model View Controller**. We could say the MVC architecture is like a simplification of the layers architecture, incorporating the front-end side (UI) of the application as well.

-   The view layer will be responsible for rendering the UI.
-   The controllers layer will be responsible for defining routes and the logic for each of them.
-   The model layer will be responsible for interacting with our database.

There're many frameworks that allow you to implement MVC architecture out of the box.

___
# References
[The Software Architecture Handbook](https://www.freecodecamp.org/news/an-introduction-to-software-architecture-patterns/)