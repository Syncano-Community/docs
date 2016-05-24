---
title: "Glossary"
excerpt: ""
---
##Admin GUI Panel##
An interface that allows you to perform all of the tasks in a friendly, graphical environment. Apart from mirroring the methods user in Syncano API, you can add and manage your instances, see usage statistics (in development) etc. You'll see the panel right after you sign up and log in. 

##Administrator##
If you happen to have an account on Syncano, then you are an administrator! Administrators have access to Syncano Platform and can perform various tasks depending on their [roles](administrators).

##API Keys##
API Keys are used for authentication and should be passed as a parameter to all methods that require authorization data.

##Snippet - Script##
A piece of code that can be run in the cloud. Technology used for the environments created during the code execution is powered by [Docker](https://www.docker.com/). Go to [Snippet Scripts](snippets-scripts) chapter for examples and in-depth explanation. 

##Script Runtime##
Environment that the code passed in Script is executed in. Apart from providing simple runtimes that contain preinstalled programming languages like python, ruby or node-js platform, we will be adding more specific ones containing additional libraries and features (i.e. python + requests library). 

##Class##
It's a description of subset of data that is to be used when creating Data Object. It says what kind of fields will be present within that Data Object.
    
##Class Schema##
Metadata for Data Objects. [Json Schema](http://json-schema.org/) that is describing what kind of fields will Data Object contain.

##Data Object##
With Data Objects, you can store your data with additional properties. There are two types of properties, built in and custom (user-defined). Data Objects always have a class ("basic" is the default).

##Instance##
Instance is a set of your data. It contains information about Classes, Data Objects, Users, API Keys and Administrators. You can own and/or belong to multiple instances. 

##Syncano API##
A set of methods that allow you to utilize Syncano's core features like real-time synchronization, data storage, data modeling and access management.

##Triggers##
A way of running a Script. When set, they will run a specified Script based on Data Object changes happening within a specified Class. 

##Script Endpoints##
User-defined HTTP callbacks. On Syncano, you can make publicly available Script that will execute your code.

##Schedules##
Schedules run your Scripts in specified time interval. There are two params to set the time interval: 
  * interval_sec - how often (in seconds) the schedule should run
  * crontab - when schedule should run (in cron syntax)