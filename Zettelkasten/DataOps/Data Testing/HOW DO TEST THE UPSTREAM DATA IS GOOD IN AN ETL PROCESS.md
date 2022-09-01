222022-09-01

Style: #ArticleNotes 

Domain: #DataOps 

Tags:

# HOW DO TEST THE UPSTREAM DATA IS GOOD IN AN ETL PROCESS
So we have one or more datasets we need to read, we have a list of attributes, and we have a list of possible values for each attribute. You probably won't be given this level of detail, you will likely have to study the data and figure it out yourself and come up with your own rules. Sometimes your own rules will be wrong, but that is fine if you have a failure in production and the rule you created was incorrect you can figure out what the rule should be and change it.

The goal of thinking about the rules should be to come up with a list of these answers for each attribute:

-   Can it be null?
-   What data type is it?
-   What can the min/max it can be?
-   Is there a known range the data can be in?
-   If an attribute is one particular value, does that restrict what other attributes can be?

This is a guide, only you will know what makes sense for your data, you will need to look at the data and any documentation you have and figure out the best approach.

So armed with the knowledge of what data you expect to receive, you can start formalising that in your ETL pipeline, how exciting.

## Monitoring

What we need to do is to build in monitoring, we need to capture metrics and report on those metrics, we need to create dashboards and look for trends to show when things have gone wrong and have alerting for when things really go wrong.

### What do we need to know?

At a minimum we need to know:

-   how many records you read
-   how many records you write
-   how long the process and ideally each step takes
-   how many failed records you have
-   summaries of essential metrics that you process (total sales for example)
-   any errors or exceptions and ideally failed records should be stored somewhere to be dealt with later

How you capture these metrics depends on which tooling you are using for your ETL process unless you are writing your own framework there will be a way to capture this data if there isn't a built-in way to capture it then write it to your own logging database or system.

If you use something like Azure Log Analytics, Elasticsearch, Datadog, whatever, for your application logging, then these can be good choices for your ETL pipeline logging. I would always recommend using whatever systems your other developers use. This way, you can piggy-back on the work of others.


### What do we do with the data?

You keep it, you keep it a long time, and you use it to:

-   create alerts when things are wrong
-   create charts showing when things are trending towards wrong (getting slower and slower)
-   refer back to when things are going wrong so you can see what is normal or not

If you have a list of previous runs, how long it took and how many records they processed, you can do things like say “the process is still running, but when it had the same number of records last week it was much quicker so there must be something else wrong”, or more helpfully “when it has this number of records, it is expected to take x minutes and we are x - 10 minutes into the process so if it isn't finished in 10 minutes there _might_ be a problem”.

There is nothing worse than trying to troubleshoot something and not having any data to know what is normal and what is abnormal - I have wasted a significant portion of my life because there has either been a partial or total lack of historical data which would have helped an issue.

Having monitoring and alerting and being able to see if your pipelines are working efficiently is the only way to know, for sure, that your pipelines are working correctly in production. You can test for now until eternity but until you are in production you are can never be guaranteed that they will work.




___
# References
[How do test the upstream data is good in an etl process (etl testing part 3)?](https://the.agilesql.club/2019/09/how-do-test-the-upstream-data-is-good-in-an-etl-process-etl-testing-part-3/)