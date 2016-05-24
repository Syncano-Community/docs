---
title: "Billing"
excerpt: ""
---
This section of the docs is aimed at explaining:
- [The differences between paid and free plans](#section-the-differences-between-paid-and-free-plan)
- [How to add credit card details](#section-how-to-add-credit-card-details)
- [How to switch to a Production (paid) plan](#section-how-to-switch-to-a-production-paid-plan)
- [Billing periods](#section-billing-periods)
- [What are soft and hard limits and how to set them](#section-what-are-soft-and-hard-limits-and-how-to-set-them)
- [How to update billing address](#section-how-to-update-billing-address)
- [How to get an Invoice](#section-how-to-get-an-invoice)
- [Freezing an account](#section-freezing-an-account)

# The differences between paid and free plan

After signing up to Syncano, your account will be in a "Builder Plan" by default. Builder Plan is free and lasts for 6 months. At the end of the 6 months, you can switch your account to production to continue using all the functionality the Syncano platform has to offer. But don't worry, we will make sure you have plenty of advance notice that your plan is about to expire. And you can always contact us with any questions you may have at payments@syncano.com.

Apart from being free of charge, the Builder plan has several differences in comparison to paid accounts:
[block:parameters]
{
  "data": {
    "h-1": "Builder",
    "h-2": "Production",
    "h-0": "Difference",
    "0-0": "Cost",
    "0-1": "Free",
    "0-2": "Paid starting from 25$ and more depending on usage",
    "1-0": "Limits",
    "1-1": "100000 API Calls + 1000 CodeBoxes / month",
    "1-2": "No limits",
    "2-0": "Reaching account Limits",
    "2-1": "Account suspended until the first day of next month",
    "2-2": "No limits",
    "3-0": "API Requests per second",
    "3-1": "15 API Calls / sec",
    "3-2": "60 API Calls / sec"
  },
  "cols": 3,
  "rows": 4
}
[/block]
# How to add credit card details
To add a credit card, go to [Payment methods page](https://dashboard.syncano.io/#/account/payment-methods) 

In the input fields, fill in:
- Your credit card number 
- Credit cards CVC number (three digits that can be found on the back of the card)
- Credit cards expiration month and expiration year
- Press "Add Payment" button. 

That's it! Your card has been added to the system and you can start using Syncano in Production mode by flipping the switch on [this page.](https://dashboard.syncano.io/#/account/plan)

# How to switch to a Production (paid) plan

1. If you want to switch to Production Plan, go to [Billing plan page](https://dashboard.syncano.io/#/account/plan)
2. Click on 'Open Plans Explorer' button
3. Choose the amount of API Calls and CodeBoxes you'd like your plan to cover
4. Fill in your credit card details if you haven't previously done so
5. Click 'Confirm' button
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/xFu0X6mZR0C5DFy0mPxj",
        "billing_choose_plan.png",
        "1351",
        "986",
        "#0d325f",
        ""
      ]
    }
  ]
}
[/block]
Congratulations! Your account is now in Production. What it means is that:
- Unlimited amount of API Calls or CodeBox runs can be performed from this account.
- Once the account reaches the limit, it won't be suspended. The account will continue to work but all the API Calls or CodeBox requests will be subject to overage pricing. You can see the overage pricing details on [this page](https://www.syncano.io/pricing/)
- You can set hard and soft limits for your account

# Billing periods

To make things simpler billing periods are in sync with calendar months. When you switch to Production plan in the middle of the month you'll be billed immediately for the amount of days that are left to the end of the month (including the current day). So if you switched to Production plan on the 16th of a 30 day month you would pay half of the bill.

#### Switching to a different plan
On occasion, you might want to switch your current paid plan into a different one. In order to do this: 
- While being on [billing plan page](https://dashboard.syncano.io/#/account/plan), open up the plans explorer by clicking "Upgrade your plan" button
- Adjust your plan by moving the sliders for API Calls and CodeBoxes
- Click "Confirm" button

The new plan will take effect from the next billing period. You can always click "Cancel Change" button to revert the change before the new plan starts. You can also click "Upgrade" button to make further changes to your plan.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/R1x2eSElS4e7rx310wN2",
        "billing_choose_plan_again.png",
        "1350",
        "940",
        "#216fc3",
        ""
      ]
    }
  ]
}
[/block]
# What are soft and hard limits and how to set them

Soft and Hard Limits are available for Production Plans and were created to give a Syncano administrator greater control over his/her expenses. They are triggered when your account reaches a budget threshold. Once the Soft Limit is reached, the administrator will receive an email notification about this event taking place. Reaching hard limit blocks the account so that no API Calls or CodeBox executions can be made.
[block:callout]
{
  "type": "info",
  "body": "By default Soft Limit is set at 1.5 time of the amount of your Plan fee and Hard Limit is set at 3 times the amount (For a 50$ plan, Soft and Hard Limit would be 75 and 150 respectively)."
}
[/block]
For example, an administrator switches to a 25$ Production Plan. He also decides that he can afford to pay more in the event of large traffic that his website/app generates but no more than 100$ a month. In this case he will set up the Hard Limit to 100. This way whenever his app reaches 100$  budget in combined  API Calls and CodeBox executions, his account will be blocked. He also wants to be notified a bit earlier about the possibility of his account being blocked so he sets up the Soft Limit to 85$. This way he'll be notified with an email that his account has reached a Soft Limit and he can take some actions (change the decision about Hard Limit, inform the users about downtime etc.).

To change your limits:
- Go to the [Billing plan](https://dashboard.syncano.io/#/account/plan) page
- Scroll to the bottom of the page
- Input the desired values in the soft and hard limit fields
- Click "Set Limits" button to apply changes


[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/4rjskytiRt1bI6be444g",
        "billing_limit.png",
        "1350",
        "980",
        "#206abc",
        ""
      ]
    }
  ]
}
[/block]
# How to update billing address

Go to [Billing address](https://dashboard.syncano.io/#/account/address) page. Here you can input data that will be displayed on your invoices from Syncano.

# How to get an Invoice
Your invoices are available on [this page](https://dashboard.syncano.io/#/account/invoices)

First invoice will be generated right after you switch to Production Plan. Next invoices will show up during the following billing periods. You can download an invoice in pdf format by clicking the "View" button on the invoices list.

# Freezing an Account

If you would like to stop using Syncano, you can freeze an account. It can be done by:
- Clicking the plan switch on Billing Plan page
- Clicking "Yes I want to Cancel" button in the modal window
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/wxwH9Bi2SU171EMOKuzv",
        "billing_freeze.png",
        "1355",
        "951",
        "#0e325e",
        ""
      ]
    }
  ]
}
[/block]
After canceling, your account will stay active until the end of the current month. During this period you can backup all your data and shut down all the lights. With the beginning of the next month your account will become inactive. You'll still be able to login but the instances list will disappear so you won't be able to perform any actions on them or the data within. In frozen state the only accessible part of the Dashboard are your Account settings. You can always unfreeze your account by: 
- Going to [Billing plan page](https://dashboard.syncano.io/#/account/plan)
- Clicking "Subscribe" button. 
- Choosing your plan and clicking "Confirm" in the modal window

Your account will become active again and you will regain an access to all your data.