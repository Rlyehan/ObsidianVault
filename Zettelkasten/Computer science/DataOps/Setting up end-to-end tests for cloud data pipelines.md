222022-09-01

Style: #ArticleNotes 

Domain: #DataOps 

Tags:

# Setting up end-to-end tests for cloud data pipelines


### Setting up locally
Setting up our pipeline components locally is not always possible. Som ecan be easily deployed locally in a docker container, others can be at least mocked in some form, and some might actually require us to use an alternetive if we would like to set them up locally, but that can come with quite some drawbacks.

Example of local setup:
![[Pasted image 20220901104143.png]]

![[Pasted image 20220901104152.png]]

Setting up end-to-end data pipeline tests can take a long time depending on your stack. Despite its difficulties, the end-to-end test can provide a lot of value when you modify your data pipelines and want to ensure that you do not introduce any bugs. A few points to keep in mind when setting up end-to-end tests are

1.  Be aware of what exactly you are testing. It might not be the best use of time to mock automatic triggering/queuing systems locally, since one can be pretty confident that this will be handled by the cloud provider.
    
2.  If you are using vendor services (E.g. Snowflake, Redshift, etc), it might be acceptable to use an OSS such as Postgres to mock the data warehouse. However, this can be difficult if you end up using service-specific functions. E.g. [Snowflake’s QUALIFY](https://stackoverflow.com/questions/52879035/is-there-a-way-to-use-qualify-in-postgresql)
    
3.  Generally, end-to-end tests take a longer time to run compared to unit and integration tests. Use [google’s test pyramid](https://testing.googleblog.com/2015/04/just-say-no-to-more-end-to-end-tests.html) to get an idea of the number of units, integration, and end to end tests to write.
    
4.  If you are short on time, concentrate on writing unit tests for the complex, error-prone, and critical pipeline components, before setting up an end-to-end test.


___
# References
[Setting up end-to-end tests for cloud data pipelines · Start Data Engineering](https://www.startdataengineering.com/post/setting-up-e2e-tests/)