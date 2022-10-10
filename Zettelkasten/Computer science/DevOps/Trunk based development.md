222022-07-21

Status: #Research 

Tags: #DevOps 

# Trunk based development

Is one of the main patterns developers can make us of [[Version Control]], and the most effective one according to the DORA research program.

In trunk based development, each developer merges their work into the trunk (main) at least once a day. In order to do so we do not have feature branches, since the scope would not allow this type of development. 

The only persistent branches we have are release branches, and any changes made to these are also merged back into the trunk. And even they might not be required, especially if we have releases on a daily or even multiple times a day basis. 

Some of the benefits of this approach are that these regular merges keep the number of merg conflicts low and the scope of them small so that they are easy to resolve. It also allows for faster feedback of the impact of the different changes made an wether they work together or not, keeping regression problems to a minimum. 

And both of these benefits additionally keep the testing efforts smaller, allowing us to run them more frequently and thus catching issues even earlier.

To get the most out of trunk based development, we need to pratice [[continuous integration]] (CI), executing [[automated testing]] after every commit to the trunk to ensure that our system is always in a working state. This also works hand in hand with one of the main priciples of CI by elemiting long integration and stabilization phases.

But this approach with requirments beyond the versioning approach. For one the often used appraoch of using pull requests for code reviews, in geral in an asynchronous manner, is almost in direct opposition with trunk based development, especially if developers commit to the trunk multiple times a day. 

So here are some key points if we want to implement trunk based development:
- **Develop in small batches**
- **move away from asynchronuous code reviews**
- **implement [[automated testing]]**
- **Have fast buid times**: if your building  and testingtakes more then a few minutes, this might indicate an optimization potential in your architecture





___
# References
