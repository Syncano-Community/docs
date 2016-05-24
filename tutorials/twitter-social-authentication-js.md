---
title: "Twitter Social Authentication (JS)"
excerpt: ""
---
## Chapter Content
1. [Introduction](#introduction)
2. [Setup](#setup)
3. [Updating settings](#updating-settings)
4. [Run the app!](#run-the-app) 
[block:api-header]
{
  "type": "basic",
  "title": "Introduction"
}
[/block]
This tutorial will show you how to implement Syncano as a proxy for Twitter Social Auth using hello.js library.

Working demo is available at: http://syncano-social-auth.herokuapp.com/
Github repository: https://github.com/Syncano/social-auth-examples

Prerequisites:
- An account on Syncano Platform. You can sign up [here](https://dashboard.syncano.io/)
- [Node.js](https://nodejs.org/en/)
- git

[block:api-header]
{
  "type": "basic",
  "title": "Setup"
}
[/block]
1. Clone the social-auth-examples repo from the Github repository. You can do this by going to your terminal and pasting this line:


[block:code]
{
  "codes": [
    {
      "code": "git clone git@github.com:Syncano/social-auth-examples.git",
      "language": "shell"
    }
  ]
}
[/block]
2. `cd` into the cloned repository:
[block:code]
{
  "codes": [
    {
      "code": "cd social-auth-examples/",
      "language": "shell"
    }
  ]
}
[/block]
3. Install the dependencies with npm:
[block:code]
{
  "codes": [
    {
      "code": "npm install",
      "language": "shell"
    }
  ]
}
[/block]
Dependencies needed for the project to work will be installed. Now we'll have to change the apps settings to work with your Syncano Instance.
[block:api-header]
{
  "type": "basic",
  "title": "Updating Settings"
}
[/block]
There are two files that need to be updated for the application to work:
- hello.js
- index.ejs

### hello.js

This demo is using [hello.js](https://adodson.com/hello.js/#hellojs) for handling the authentication flow on the client side. To make it work with your application based on syncano you need to follow these steps:

#### 1. Copy your Instance name from the Syncano Dashboard

You'll find your Instance name on the Instances list in the Syncano Dashboard:

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/VlY7Bo0mQ62o5VTuGY1O",
        "instances_list.png",
        "1919",
        "985",
        "#264474",
        ""
      ]
    }
  ]
}
[/block]
Select the name of an Instance you want to use and copy it to your clipboard.

#### 2. Add your instance name to hello.js file
You can find the hello.js library in the `lib/` directory of the project. Open it in your favorite editor. Line 82 of this file should look more or less like this:
[block:code]
{
  "codes": [
    {
      "code": "\t\toauth_proxy   : 'https://api.syncano.io/v1/instances/INSTANCE_NAME/user/auth/backend/twitter/proxy',\n",
      "language": "json"
    }
  ]
}
[/block]
Change `INSTANCE_NAME` to the name of your Syncano Instance that you just copied.

### index.ejs

index.ejs file is in the `views/pages/` folder of the project. In lines `48`-`51` we are initiating a connection to Syncano via the Syncano javascript library:

[block:code]
{
  "codes": [
    {
      "code": "var instance = new Syncano({\n  apiKey: 'API_KEY', \n  instance: 'INSTANCE_NAME'\n});",
      "language": "javascript"
    }
  ]
}
[/block]
You'll need to generate a new API KEY. To do this:
1. Go to Syncano Dashboard -> Chosen Instance -> API KEYS tab
2. Click `Generate an API Key` button
3. Check the `user registration` toggle button
4. Click `Confirm` button
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/2EYenqNsQkaorJVc12Yx",
        "Add_user_regiter_API_Key.png",
        "1279",
        "659",
        "#10263d",
        ""
      ]
    }
  ]
}
[/block]
Once you have your API Key, paste it into line `49` of index.ejs file.

Also paste the instance name into line `50` of index.ejs (the same as you did in hello.js file in previous step).
[block:api-header]
{
  "type": "basic",
  "title": "Run the app!"
}
[/block]
That's all that needs to be done in order to implement social login by using Syncano as a proxy to Twitter authentication. Now you can run the application by typing this command from the root of the project:
[block:code]
{
  "codes": [
    {
      "code": "npm start",
      "language": "shell"
    }
  ]
}
[/block]
It will start the web server that is available under `http://localhost:5000/`. Paste the url into your browser bar and go through the Twitter signup flow. Now go to the Syncano Dashboard. Click on the Instance that you used for the purposes of this demo. Go to users page. You'll see that a new user was added to your application!


[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/Bse9FNlJQIynsLiyYCyw",
        "twitter_new.png",
        "1919",
        "985",
        "#264574",
        ""
      ]
    }
  ]
}
[/block]