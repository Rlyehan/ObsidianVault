232023-01-04

Style: 

Domain:

Tags:

# What is data profiling

Data profiling is the act of evaluating your data and its state, and it is [essential](https://hbr.org/2017/09/only-3-of-companies-data-meets-basic-quality-standards).   
  
During data profiling, you identify and measure the characteristics of your data that are meaningful to you: the structural, content, and relationship information that tells you what you need to know in order to appropriately use the dataset.

## What is data profiling used for?

The better question is: what _isn’t_ data profiling used for?  
  
[Data profiling](https://www.oreilly.com/library/view/principles-of-data/9781491938911/ch04.html) is the cornerstone of just about any business intelligence, data science, or data quality initiative.  
  
It’s how you answer fundamental questions like:

-   What is this data about?
    
-   Is it the data I thought I had?
    
-   Does it contain what I expected it to?
    
-   Does it contain anything unexpected?
    
-   Is there a different amount of it than I expected?
    
-   How is it formatted?
    
-   Am I parsing it in a way that makes sense?
    
-   What’s the quality of the data?
    
    -   Is it complete?
        
    -   Does it make sense?
        
    -   Is it accurate?
        

The answers to these questions (and others you ask) direct the next steps of your data project.   
  
From the data profile, you’ve identified what you need:

-   To gather more data?
    
-   Different data?
    
-   To investigate the meaning of any of the data?
    
-   A different tool for the data?
    
-   Data quality repairs on the data?
    

In essence, data profiling tells you whether you’re ready to move to the next stage of your project. And if you’re not, data profiling tells you what directions you need to go to _become_ ready.  
  
Usually, data profiling is talked about in the context of broad, company-wide business intelligence or data quality projects. But [any downstream project can benefit from data profiling](https://blog.hubspot.com/website/data-profiling).  
  
And in fact, you’re probably already doing data profiling without realizing it: if you glance at your data to see if it ‘looks right,’ that’s the most basic form of data profiling.   
  
Of course, a more thorough data profiling is going to give you better information. So let’s discuss how to do that.

### Structural data profiling
Structure data profiling describes the meta-arrangement of the data. This includes basic parsing information like the file or database format, delimiters, column and row order, and column names. You can also look at properties like value patterns and statistics when profiling data structure.

Note that there are several ways this kind of change could be detected: using column headers, type prediction, column-level statistics, or pattern identification, among other techniques. Statistics and pattern identification, in particular, can act as important data quality guardrails at this point in the data profiling process.

### Content Data profiling
Content data profiling is when you examine the semantic values of the data.  
  
This includes evaluating the data’s completeness and types, as well as less tangible qualities like accuracy and validity.

Data can be incomplete at the field, column, row, and source levels. Data that is incomplete because all the timestamps have been replaced with a null is a materially different issue from data that is incomplete because it’s supposed to be for all of Q1 but only February timestamps are present.

In addition to checking that the expected kind of data is present—and that unexpected data isn’t—type detection and profiling can also play a role in structural data profiling, acting as a way to detect structural changes in the absence of meaningful column headers.  
  
When profiling data’s accuracy and validity, the difference between ‘data profiling’ and ‘data quality’ can become extremely blurry. There’s no universally-applicable divide between ‘profiling’ and ‘quality’ when it comes to accuracy and validity evaluation, so if you need to make a distinction, do so in whatever way makes the most sense for your workflows.

### Relationship data profiling
Relationship data profiling looks at the connections between individual fields and rows in the data. It’s sometimes referred to as _relationship discovery_.  
  
Within records, relationship data profiling can check things like: does the address data in this record actually form a valid address? Address validation is an extremely common form of relationship data profiling, and is frequently supplemented by external address validation services even if the rest of the data profiling is done in-house.  
  
Relationship data profiling can even extend across datasets; concepts like householding are usually not supported by reliable keys in the raw data, and require the [complex understanding](https://greatexpectations.io/blog/data-assistants-close-the-data-trust-gap) of entities as a whole that relationship data profiling can provide.

## Data profiling vs Data mining
When doing data profiling, you are aiming to understand what the data is—essentially, you’re meeting the data where it’s at. Regardless of your ultimate goals for the data, data profiling is where you evaluate whether the data is suitable to use for achieving those goals.  
  
Data mining is a step or two further down the road: it’s one of the ‘ultimate goals’ that you might have for your data. Specifically, data mining is often predictive work, identifying the patterns and trends from your current data and using those to draw conclusions that you act on.  
  
Data profiling is crucial to data mining because data mining is a classic garbage-in-garbage-out scenario: if you don’t profile your data, you have no idea whether your data mining results are trustworthy.




___
# References
[Great Expectations • Great Expectations](https://greatexpectations.io/blog/what-is-data-profiling)