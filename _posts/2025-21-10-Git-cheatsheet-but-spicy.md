---
layout: post
title: "Git cheatsheet but spicy: have handy my own setup to start coding anywhere"
date:   2025-10-21
categories: coding tools
permalink: /:categories/:year/:month/:day
---

# Git cheatsheet but spicy

For some time I have prefered to remember a few simple steps that leads me through a methodology to found what I'm looking for instead of 
use my memory or use a document that explain specifically what I'm lookgin for because I felt that this way I was putting my mind out of 
the exercise of always know where to look of how instead of hoping to have noted down somewhere the correct and particular answer I searching. 

Ok this is good but, it doesn't scale to much. When your coming and go from differents develoment environments or switching between softwares for a while (the duration of a project let's says a few months) all this "I have a method to find everything I need to start" beggins to build up some friction in your daily execution. 

# Just a regular, everyday, normal git commands

One of the best uses of note what you usually use is the "git help everyday" guide that you can access typing that command on your terminal. They 
really achieve the value taking notes effect


# Tips

## Merge master before commiting your code

Let's say you have work for two weeks in a feature branch. During this time probably a lot of people have make changes or merge other feature in master branch. 
So it is better to switch firts to you master branch, pull all the new changes ('git pull origin master'). Solve conflicts if needed accepting all the new changes. 
Once your master is up to date then switch back to your feature branch and merge master into your branch, now your branch have latest master and your feature changes. So this branch is ready to push it out. 

## Learn to undo a local commit

Undo things could be necessary from time to time. Image the project is big enough or a lot people is working at the same time and you always need to pull 
into your branches the latest changes from the rest of the team. Let's say you always do but this time you forgot to do it because you just have done a little 
change after a big push into the repo. So you feel very confident about making this little change so just go after it, do it and commit it. 
So wrong, you will the codebase is possibly ahead of you so you need to remove the changes, update the branch and put your changes back on top of this. 

''git reset --soft HEAD~''

HEAD~1'

HEAD is the currently branch latest commit, the ~1 is the latest minus one commit, this is the coordinate you give to the reset command. It is discarding forward 
from there (which is actually what we want)

--soft flags say that is not undo the changes, only undo the commit and move the head to the previous commit. 

## There's no fear on branches

A new branch is a safe place. Do not be afraid of it, use it in your advantage. A new branch is a place where you can just try whatever you want and even broke everything if needed to test something new or extreme. 
Use this capability and put it in your side. 
Now. 
Be careful with branch names. Branches should be very very explicit in what they are doing or why they have been created just by reading their name. 
If you make a mistake a merge a different branch just because you put a similar name with another that have a "critical" failure product of a test you do 
the day before this error will pass on superior branches messing all the thing up. 
Think that probably the person merging your branch could not be you, or that day you went on a sick leave and some of your teammates must merge your changes. 
They have to be completely capable of doing it without you. Document branches clearly, use specific ticket or bug number, use tags like FIX, PATCH, BUG, DEV or other communications strategies. 

## Merge conflicts

Hangle merge conflict as a discussion with someone else about ideas. Take your time and think that other developers can have spend a lot of hours putting effort 
in those changes. 
Handle merge conflict in VS code. Communicate with your team. Focus on the workflow to be natural and easy to show and explain. Don't try to go full terminal just because is fast and fancy. Be smart. 

## Forget about - git rebase - 