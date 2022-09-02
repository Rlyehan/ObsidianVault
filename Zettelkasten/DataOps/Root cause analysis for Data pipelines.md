222022-09-02

Style: 

Domain: #DataOps 

Tags:

# Root cause analysis for Data pipelines
Data pipelines can break just as easily, if not more so, than any other software product. as such, proper knowledge of  how to[[Investigating failures]] is an important skill fo any data engineer.

The key to this is [[root cause analysis]], or RCA. This is a set of techniques that have a structured approach to investigating what caused any particular failure. This approach should help whoever is doing the investigation to find the "root" cause of the particular problem, instead of stoping with only treating the symptoms.

While this might sound like it would be straight forward, and it can be, we will often enough run into situations where the root casue is not obvious. 

THe structured approach mentioned before helps in making this easier by giving us steps to walk trough, but all of these steps depend on one important fact, that we have enough information about our system. This information does not come from nothing, we need to put the systems and mechanism in place to make sure we can obtain that information easily and that the information is also actionable.

For that we need to have robust [[Data observability]] and [[observability]] tooling in place and setup appropriatly. 

With this out of the way, what could a hollistic approach to investigating a data pipeline failure look like?

### Step 1: Look at your lineage
To understand why we ahve an issue, we want to look the most upsteram node in our lineage where we coudl find any anomalies.

### Step 2: Look at the code
Once you found the most upstream node, invetigate that node. What ccode was executed for the most recent update of the dat we are investigating? Has there beeen any recent changes to that code?

### Step 3: Look at the data
If we still haven't found the root cause by now, the next step is to investigate the data itself.
-   Is the data wrong for all records? For some records?
-   Is the data wrong for a particular time period?
-   Is the data wrong for a particular subset or segment of the data, e.g. only your android users or only orders from France?
-   Are there new segments of the data (that your code doesn’t account for yet…) or missing segments (that your code relies on…)?
-   Has the schema changed recently in a way that might explain the problem?
-   Have your numbers changed from dollars to cents? Your timestamps from PST to EST?

To write down just a few of the questiopns you might wna to ask yourself when you have to investigate the data.

 Make sure everyone troubleshooting data problems can handily slice and dice data to find how the issue correlates with various segments, time periods and other cuts of the data. Visibility into recent changes to the data or its schema is a lifesaver. Keep in mind that while these statistical approaches are helpful, they are just one piece of the larger RCA process.


### Step 4: Look at your environment
You wen trhough all the steps above, and even when lookign at the data you could not spot the root cause, now it might be time to look at the environmetn our pipeline runs in.

-   Have relevant jobs had any errors?
-   Were there unusual delays in starting jobs?
-   Have any long running queries or low performing jobs caused delays?
-   Have there been any permissions, networking or infrastructure issues impacting execution? Have there been any changes made to these recently?
-   Have there been any changes to the job schedule to accidentally drop a job or misplace it in the dependency tree?

### Step 5: Leverage your peers
If you still could not spot the casue, maybe no its time to go to your colegues and get their help. Maybe they had run into an similar issue than yours and found the problem, or maybe a discussion with them can help you get on the right track to find the issue.

___
# References
[Root Cause Analysis for Data Engineers | by Barr Moses | Towards Data Science](https://towardsdatascience.com/root-cause-analysis-for-data-engineers-782c02351697)