---
layout: post
title:  "Advanced Git"
date:   2021-01-27 21:03:36 +0530
---

## What is git rebase?
Git rebase is when we have two seperate branches and we clean up the history by combining those two branches which allows us to bring back the branches into one history line. It is very similar to the `git merge` but it focuses on cleaning up the history by setting the point of reference to the most change to the branch being rebased to. In reality, both `rebase` and `merge` are trying to acheive the same goal: get the commits from the `feature` branch onto the `master` branch. But `rebase` is a little bit different. Instead of simply mergin those `feature` branch commits into a new merge commit, git, behing the scenes, gets rid of those feature banch commits and copies them over into new master commits. Let's use an example: 

In this case we have the `master` branch and the `feature` branch. View the screenshot below to see a sample of my git repository
![Image of git visualization](../../Screen Shot 2021-01-27 at 5.50.06 PM.png)

Soon we will `rebase`. Before doing so, I will make sure to `checkout` the master branch and then do a `git pull` to make sure I am up to date with the remote `master` branch. (In this case it will be `main` branch in the screenshots). 
![Image of git pull](../../images/git-pull.png)

Now, my `master` branch is still ahead of my `feature` branch by two commits. I next want to re-anchor my feature branch against the latest commit from the master branch. I do this by doing a `checkout` on the `feature` branch and then doing a `git rebase master`.

As we begin to perform a git rebase, what will happen is that the `feature` branch will `merge` itself into the `master` branch and git will copy all of its commits with it. In this case it will be two commits ahead of the `master` branch. The difference will be that `master` is still not caught up with `feature` although their histories are now aligned and the reference point for `feature` is now the most recent master commit. Here is what it looks like:
![Image of git rebase visualization](../../git-rebase-example.png)

Let's also take a look at the command I used in my VSCode Project to my Git Repo:
![Git Repo Commands Rebase]()










![Click Link Above to see Photo!](../../croppedMe.jpg)

Hi! My name is Wyatt Allan and I am from Santa Clarita, California. You may know the area due to the theme park called Magic Mountain. I am the second oldest of 4 brothers and we all love to snowboard and dirt bike! 2020 has been quite the year for me. I got married, got my first developer job, and now my wife and I are building a house! 

I work at SecurityMetrics as an Angular Developer. I recently picked up another gig with a start-up called Xenon. They found me through LinkedIn so I know that updating and working with people through social media really works at building a network. Everything I have job related is due to networking so be sure to make the time to connect with others. 

Web Developement amazes me. It is so complex and vast. Truly a man-made language to display content for users. I was originally a Cinematography student at UVU but, after realizing the job market in that field, I decided to switch gears into coding. From the beginning, I thought coding was a hackers trade but soon realized how vast it really is. To me, web development is the wild west. It stands on the frontier of whats new and seeks to explore the great unknown. How exciting to be a part of it. 

In this class, I hope to really learn how to apply great JavaScript Logic to all of my projects. I realized, even as an Angular Developer, I still need a good foundation in JS to really connect all my pieces together in Angular. JavaScript has always been a hard task in my mind and I hope to dispell the belief in my head that it may be too hard for me. Time to learn like crazy... again.


Check out my [LinkedIn][linked-in] for more info on my job history and about me.

[linked-in]: https://www.linkedin.com/in/wyatt-allan-a41340112/
