---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
starred: false
keywords: []
description: A RESTful Easter Sunday
datePublished: '2016-03-28T04:55:19.876Z'
dateModified: '2016-03-28T04:55:14.801Z'
title: ''
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
sourcePath: _posts/2016-03-28-a-restful-easter-sunday.md
published: true
url: a-restful-easter-sunday/index.html
_type: Article

---
A RESTful Easter Sunday

One of my favorite things to do during national holidays is sit down and get some real technical work done - things that require hours of focus, zero interruptions, etc.

This Easter sunday, I decided to try to implement something I've been salivating over for a while.

Tech Ambassadors uses the simply incredible Asana (http://www.asana.com) to manage our tasks and projects- our entire team uses it to stay on top of things, know what's coming next, and be accountable to deliverables.

We also recently switched to Harvest (http://www.getharvest.com) for our timesheets, and we really like it.

A small hiccup in our workflow, however, is the notes for timesheet entries. Tech Ambassadors stands behind invoices that clearly record what was performed during the given hours- in order to do that, we have our technicians track 2-3 bullet points per billable hour to describe what was done.

Currently, this means tracking tasks in Asana, then entering in our time into Harvest and manually adding notes- this step could be avoided if we were able to take the names and comments of completed tasks and input them into Harvest's timesheet entry.

Basically, when a new Time Sheet Entry is created, our app would pull the names of all tasks completed by that technician for that project, then insert that information into the notes field of the Time Sheet Entry.

Then, our technicians only need to focus on keeping Asana up to date, and the billing system automatically tracks what they were working on during that time.

After getting the authorization key and getting a 'hello world' request working, I started poring through the Asana API documentation at https://asana.com/developers/api-reference/tasks\#query and looking for the combination of options that gives me what I want.

Unfortunately, it looks like I can't request tasks completed within a time frame- a query using their completed\_since variable will return complete and incomplete tasks.

The solution is to ask for the 'completed' variable, and parse out any tasks returned by the API who don't match completed==true. Then the question becomes- what is the best platform to host this?

I feel I need a 'daemon' running and checking Harvest for new timesheet entries, then pulling the tasks completed during that time from Asana and updating the timesheet entry.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/398e8bc9-d207-4747-800d-fe9238a60390.png)

Sounds like a job for Zapier! We use Zapier for a few things already, and it can indeed trigger on new timesheet entries from Harvest.

Well, after 8.5 hours, I have something that 'works'! IU

Here is the process:

Step 1: Upon trigger by Zapier detecting a new Harvest time sheet entry, it pulls the data of that entry.

Step 2: Using a custom GET webhook, Zapier queries Asana and looks for TASKS MODIFIED during the time period of the TIME SHEET ENTRY in the PROJECT that matches the ASANA PROJECT ID that you manually input once in the NOTES field of the HARVEST PROJECT.

Step 3: Take the tasks returned and clean up the formatting a little bit, adding a space after each one (already separated by commas).

Step 4: Using a custom PUT webhook request, UPDATE the NOTES of the HARVEST TIME SHEET ENTRY to be the ORIGINAL NOTE FROM STEP 1 + 'Asana tasks modified during this work period:' + FORMATTED ASANA TASKS FROM STEP 3\.

So, as long as one manually inputs the Asana project ID of each project into the notes field of each Harvest project, this one Zap will automatically import the names of tasks modified during the work period, which is pretty sweet.

To do list:

What happens if you submit a time sheet entry after the fact? Can this script accomodate that? Should we just include all Asana tasks modified during that day?

Can one Zap do this for each user? Currently, this requires each user to have their own Harvest integration. I might be able to do it with a cusom webhook in step 1 instead of Zapier's standard Harvest module.

I'm having a major problem where I can't use Zapier to successfully request a list of all Harvest projects - my goal was to avoid the manual input of the Project ID by just having Harvest compare the project names in Asana and Harvest, but until/unless I can get that to cooperate, it's a non-starter.