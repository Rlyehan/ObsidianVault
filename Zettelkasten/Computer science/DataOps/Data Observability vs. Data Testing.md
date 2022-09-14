222022-08-26

Style: #Research 

Domain: #TestAutomation  

Tags: #Data 

# Data Observability vs. Data Testing

[[data testing]] is an important tool for us as a first line of defence against bad data, like we use unit tests in a traditional software project. This approach was and still is fine if the volume of our data remains managable, but many modern projects ingest so much data that this is not reallz feasable any more. 

For this we can make use of the same mechanisms for our data pipelines that has founds its way into modern software applications, [[Observability]]. 

And why might we might want that? The main problem we want to deal with is the simple fact that we can only test what we expect (of course tools liek [[hypothesis]] can help extend that somewhat) but what about changes in our data that we couldn't predict?

### When should I test my data?
Testing data allows us to validate our assumptions we have for a given dataset. This is a must and allows us to catch known problems.

Examples:
- NULL values, like any where none should be, an unexpectedly large amount in one set, etc.
- Volume, did we get the data we expected?
- Distribution, are our values within expected ranges?
- Uniqueness, are values that should be unique actually unique?
- Known Invariants, is the enddate never before the startdate?


This alone can already catch most of the issues with our data, especially the most critical issues. But, same as with unit tests, that alone is not enough.


### When should I use Data Observability?
[[Data observability]] gives us the possibility to have visibility of our data pipeline end-to-end. 
To do so we make use of automated tools taht allow monitoring, alerting, and triageing to identify and evaluate data quality issues.

Example issues we would detect with [[Data observability]]:
- A report stops updating and keeps showing old data
- An API stops delivering data after an update
- A JSON schema cahnge turns 500 rows into 5000 rows
- Some data pipeline issue lets data trough without running the required tests.
- A test was not updated when we made changes to a data pipeline

Though it sounds like [[Data observability]] and [[data testing]] are the same, that is only the case in what they are trying to achieve, what differentiates them mainly is how the go about it.

**End-to-end coverage** 
There are many points tha make it hard to acheive true end to end test coverage for our data-pipelines, so it makes sense that we allow observability to fill that gap.

**Scaleability**
Data tests can run into scaleability issues, especiallty since said scalability often comes with things like spliting of teams and as suck means that the knowlege of the data gets spread apart. Data observability can help with some of these issues based on the tools used to achieve it.

**Root cause analysis**
Testing can rpevent many issues, but for those issues that our tests cannot catch, data observability can help us find the root cause much faster
___
# References
[Data Observability Vs. Data Testing: Everything You Need To Know](https://www.montecarlodata.com/blog-data-observability-vs-data-testing-everything-you-need-to-know/)