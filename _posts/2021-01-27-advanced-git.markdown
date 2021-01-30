---
layout: post
title:  "Advanced Git"
date:   2021-01-27 21:03:36 +0530
---

## What is git rebase and how to perform one (With screenshots)?
Git rebase is when we have two seperate branches and we clean up the history by combining those two branches which allows us to bring back the branches into one history line. It is very similar to the `git merge` but it focuses on cleaning up the history by setting the point of reference to the most change to the branch being rebased to. In reality, both `rebase` and `merge` are trying to acheive the same goal: get the commits from the `feature` branch onto the `master` branch. But `rebase` is a little bit different. Instead of simply mergin those `feature` branch commits into a new merge commit, git, behing the scenes, gets rid of those feature banch commits and copies them over into new master commits. Let's use an example: 

In this case we have the `master` branch and the `feature` branch. View the screenshot below to see a sample of my git repository
![Image of git visualization](../../Screen Shot 2021-01-27 at 5.50.06 PM.png)

Soon we will `rebase`. Before doing so, I will make sure to `checkout` the master branch and then do a `git pull` to make sure I am up to date with the remote `master` branch. (In this case it will be `main` branch in the screenshots). 
![Image of git pull](../../images/git-pull.png)

Now, my `master` branch is still ahead of my `feature` branch by two commits. I next want to re-anchor my feature branch against the latest commit from the master branch. I do this by doing a `checkout` on the `feature` branch and then doing a `git rebase master`. If there are any conflicts git will alert me.
![Image of git rebase re-anchor](../../images/git-rebase-re-anchor.png)

Now that I know there arent any conflicts, I can now do another `rebase` but this time it will be against my `feature` branch. I'll do this by first doing a `checkout` on my `master` branch and then doing a `git rebase feature` to then attach my feature's commits onto the `master` branch.
![image of git rebase onto main](../../images/git-rebase-main.png)

And there it is! Everything up to date, clean history, and good looking code.
![image of completed rebase](../../git-rebase-example.png)

## Advantages and Disadvantages of Git Rebase
Now that you have seen an example of a git rebase lets take a look at why you should/n't use `rebase`.

### Advantages
  - It makes your tree organized! There is a lot of funky things that can happen when a team of developers are all pulling and pushing and merging onto a single master branch. It can be healthy from time-to-time to declutter your tree's history. Rebase does the job.
  - It can make it easier to track. It can be easy to get lost trying to fix an issue that happened in a commit with such a messy tree with so many commits. Rebase cleans that up so its easy to trace back an error.
  
### Disadvantages
  - Do not use rebase when you are working on open source code. Being that there are so many other developers on a single project, it's best to stick with pull requests and merges to tackle that type of code.
  - Be careful with rebasing off of branches that are shared with other developers. Rebase will rewrite commits and even changes the commit hash so this can easily mess up anothers work. Be careful when to use it.
  


## What is Git Reset
A git reset is a command one can run to undo changes. Now, it is a little bit more complex than that. First, it is imperative to understand that commiting code actually can be seperated into three different steps: 
  1. The changes you have recently made ans saved onto your machine (Working Directory).
  2. Staged changes. Changes you have staged and are now awaiting to be committed (Staging Area).
  3. The commit. Commits are packaged code ready to be sent to github or alternative (Local Repository).
  Extra: You push those commits to github by doing a `git push` (Remote Repository).
Here is an image which might help with visualization:
![Image of basiv git visualization](../../images/git-basics.png)
With that information out of the way we can now move to git reset. When you have been working along, the time comes when you are ready to stage those changes. You do so but then realize you have made a mistake or want to 'reset' those staged changes back into the Working Directory changes.
Here is a screen shot of my local repo that I have manipulated with some git commands.
![repo shot of git reset](../../images/shot-git-reset-basic.png)

### Git Reset --hard
Beware of git reset --hard! This will get rid of all of your committed changes and will whipe you machine clean of changes back to a previous state. This will get rid of work you have completed. But, by all means, if that is your intent then git gives you the tools to achieve that. Personally, I manage my changes piece by piece. `Git reset --hard` resets all three of our points above from Local Repository to before Working Directory.

### Git Reset --mixed
Similar to `git reset --hard` is adding the `--mixed` flag. This will uncommit your changes from you Local Repository back to the Working Directory. You'll be able to see you changes in the Working Directory and your HEAD will be on the previous commit.

### Git Reset --soft
Again, `--soft` will uncommit you recent commit and send those changes to the stages area. If you were to commit right after using this command you would be back in the place you were right before you typed in the command.


## Git Checkout
We use `git checkout <branch name>` to move our HEAD (pointer reference to a local branch) to the desired branch. This also includes the master branch. We can then make changes on that branch and commit them to further the progress of that branch. In the end, we'll probably want to merge that branch into the master branch either with `git merge <branch name>` or `git rebase <branch name>`.
Here is an example in my local repo where I checkout feature branch, commit to it, then checkout the main branch then merge the feature branch into it. All while resolving conflicts on the way.
![git checkout example](../../images/git-checkout.png)


## Git Revert
This can totally save your butt when you have pushed/commited some code up to your remote repo and you need it back. Instead of having to go into your code base and copy/pasting all that work back into your code editor, you can do a `git revert <commit hash>`. It will look something like this:
![git revert screenshot](../../images/git-revert.png)







![Click Link Above to see Photo!](../../croppedMe.jpg)

Hi! My name is Wyatt Allan and I am from Santa Clarita, California. You may know the area due to the theme park called Magic Mountain. I am the second oldest of 4 brothers and we all love to snowboard and dirt bike! 2020 has been quite the year for me. I got married, got my first developer job, and now my wife and I are building a house! 

I work at SecurityMetrics as an Angular Developer. I recently picked up another gig with a start-up called Xenon. They found me through LinkedIn so I know that updating and working with people through social media really works at building a network. Everything I have job related is due to networking so be sure to make the time to connect with others. 

Web Developement amazes me. It is so complex and vast. Truly a man-made language to display content for users. I was originally a Cinematography student at UVU but, after realizing the job market in that field, I decided to switch gears into coding. From the beginning, I thought coding was a hackers trade but soon realized how vast it really is. To me, web development is the wild west. It stands on the frontier of whats new and seeks to explore the great unknown. How exciting to be a part of it. 

In this class, I hope to really learn how to apply great JavaScript Logic to all of my projects. I realized, even as an Angular Developer, I still need a good foundation in JS to really connect all my pieces together in Angular. JavaScript has always been a hard task in my mind and I hope to dispell the belief in my head that it may be too hard for me. Time to learn like crazy... again.


Check out my [LinkedIn][linked-in] for more info on my job history and about me.

[linked-in]: https://www.linkedin.com/in/wyatt-allan-a41340112/
