222022-08-26

Style: #Research 

Domain: #TestAutomation 

Tags: #Data 

# data testing

### Make the data assumptions in your code explicit
make sure that any assumptions about the data in your pipeline are explicit. That starts from making sure to use static typing, and to keep these types as explicit as possible. To strict types can lead to some problems, but to loose typing loses so much of the potential benefit we can have here.

Other explicit assumptions can be for example to verify that only the column names you want are present and that the columns that need to be complete are, taht unique values are unique and so on.

Some of these more metadata focused checks can also be made part of [[Data observability]] instead to keep the performance overhead from our tests lower if that is a concern.

It is also important to make sure to have our code tested with fake data. How we go about that depends on our needs. We can make use of synthetic data greation tools to create data for our tests, use tools like [[hypothesis]] that does that and some more, and make use of fake files/DBs. The best would probably be a combination of all three.




___
# References
[Why Testing Your Data Is Insufficient](https://www.montecarlodata.com/blog-what-is-data-testing/)