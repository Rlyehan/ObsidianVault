222022-09-07

Style: 

Domain: #Databases 

Tags:

# Redis
## What is Redis?

Redis is an In-Memory database taht stores the data in memory rather than on disk. Because of that it is often used as a Caching db to improve performance.

But i can be used as a primary db as well since it has become a mulit model DB.


## Multi Model DB

In larger projects, we might need to use multiple types of DBs to properly store all teh different data we have in our App. This means we ahve multiple databases deployed at the same time that need to be maintained and scaled based on their specific requirments. And if we need to have more complex communication paths, this would increase the complexity of our code and mith add network latency.

Multi -Model DBs help here by us only needing  to manage one single database. This makes the interactions simpler.


## How does Redis work?

Wr make use of redis modules for the different type of DBs. And since it is an in memory DB it is fast by default. One thing we need to note is that Redis is a schemaless DB.

## Data persistance

Since Redis is an in memory DB, data perstistance might seem lik e problem. There are ways we can deal with this however. 

One wy is to make use of replication. That is nice but not real persistance.

To get real persitance we can make snapshots based on some triggers. Thos store our data on disk, but we will lose some of our data since we need to have teh snapshots created.

There is also AOF. here we log every write operation continuoulsy. When we start up Redis again, we replay all of these logs to recreate the last state of our DB.

And we can also make use of a cmbination of Snapshots and AOF.








___
# References
[Redis Crash Course - the What, Why and How to use Redis as your primary database - YouTube](https://www.youtube.com/watch?v=OqCK95AS-YE)