222022-09-12

Style: #BookNotes 

Domain: #Development 

Tags:

# Chapter 5 bend, or break
### Decoupling

>Tell, don't ask is a principle that you shouldn't make decisions based on the internal state of an object and then update that object.

> TDA is not a law of nature, and it can be more pragmatic to break it sometimes.

>Try not to ahve more than one "." when trying to access something.


### Transforming programming
>Using a pipeline means that you're automatically thinking in terms of transforming data.

>THis way the function simply forwards an error down teh pipeline.

>The and_then function is an example of a bind function: it takes a value wrapped in something, then applies a function to that value, returning a nwe wrapepd value.

### Inheritance tax

>What makes interfaces and protocols so powerful is that we can use them as types, and any class that implements the appropriate interface will become compatible with that type.

>We think a better way is to use mixins to create specialized classes for appropriate situations.
>Now by passing instances of these classes back and forth, our code autoamtically ensures the correct validation is applied.


### Configuration

>When your application will run in different environment, and potentially for different costumers, keep the environment- and costumer-specific values outside the app.

>Configuration-as-a-Service:
>we'd like to see it stored behidn an API, THis way:
>- Multiple applications can share configuration inofrmation, with authentication and access control limiting what each can see
>- Configuration changes can be made globally
>- The configuration data can be maintained via specialized UI
>- The configuration data becomes dynamic







___
# References
