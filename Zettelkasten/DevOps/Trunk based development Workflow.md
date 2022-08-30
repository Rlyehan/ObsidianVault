222022-07-22

Status: #Research 

Tags: #DevOps 

# [[Trunk based development]] Workflow

So how could trunk based develpment look like in a project? One aspect could be working against the trunk directly. Now while this is not the recommended way to do things in general, for simple hotfixes there is nothing that speaks against it. 

Of course that is only if there is a solid [[CICD]] process in place. Because if there is and the process fails, all that happens is that the build get's marked as failed and is not released into production. 

Now if that happens it is up to you or whoever made that commit to drop everything else and fix the problem until the build passes.

The recommended git workflow is a bit different from the usual one. First we make heavy use of `git fetch --all -p` go get information about any changes that might have happened to the trunk. Then we want to `git rebase origin/master` to get up-to-date. If there are conflicts, you resove them in your IDE and the use `git rebase --continue` to finish the rebase.

If the changes you want to commit to the trunk are more complex and happened on a different shortlived branch, you might wan tto use `git rebase -i origin/master` instead, the -i flag marks it as interactive, allowing you to make changes like squashin multiple commits into one and/or changing commit messages to be more clear. This happens inside of a Vim instance, as such it makes sense to learn the basic Vim commands or have a cheatsheet at the ready for this.

after this reworking of history you use `git push origin/<branch> --force` you need to use --force since you changed history. Never do this to the trunk, only to the branch!

Once you are done with this you are now ready to commit your changes to the trunk.

Of course this is just one example workflow, using the rebase method. But there is no problem in using more traditional merge methods either.


___
# References
[A Guide to Git with Trunk Based Development | HackerNoon](https://hackernoon.com/a-guide-to-git-with-trunk-based-development-93a350c)