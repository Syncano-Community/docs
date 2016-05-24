---
title: "Snippets - Templates"
excerpt: ""
---
## Chapter Contents:
1. [Overview](#overview)
2. [Adding a Template](#adding-a-template)
3. [Writing Template Code](#writing-template-code)
4. [Rendering a template](#rendering-a-template)
5. [Bonus Stage! (ordering the response)](#bonus-stage-ordering-the-response)
6. [Template Context](#template-context)

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
Templates are a bit like Snippets scripts. They are objects that contain code, that can be executed at certain conditions. The difference is that this code can be used in a another way. One of the examples would be to wrap your data in html tags and display it as a webpage. Or create csv documents. Possibilities are quite extensive and we will dive into them in a moment.
[block:callout]
{
  "type": "warning",
  "body": "What is important to know is that templates use jinja templating engine. You can read more about jinja syntax here: http://jinja.pocoo.org/docs/dev/templates/"
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Adding a template"
}
[/block]
Adding a template is pretty straightforward and can be done from [Syncano Dashboard](dashboard.syncano.io):
1. On Instance view, click Templates in left menu
2. Click "ADD" button
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/KDbhIHOHQGCQrZ3KRFVY",
        "Add_template_00.png",
        "1279",
        "655",
        "#234675",
        ""
      ]
    }
  ]
}
[/block]
3. Fill in template name and content type


[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/YTXpeG9SSeyDSEWe5Ara",
        "Add_template_01.png",
        "1279",
        "655",
        "#254f7b",
        ""
      ]
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Content type",
  "body": "The purpose of the Content-Type field is to describe the data contained in the body fully enough that the receiving user agent can pick an appropriate method to deal with the data.\nExamples of popular content types are:\n- text/html\n- image/png\n- application/json\n\nYou can find a list of common content types [here.](http://hul.harvard.edu/ois/systems/wax/wax-public-help/mimetypes.htm)"
}
[/block]
4. Click "CONFIRM" button

Template was created so we can start writing some code!
[block:api-header]
{
  "type": "basic",
  "title": "Writing Template Code"
}
[/block]
In this section we will be:
I. [Preparing Data for the Template](#section-i-preparing-data-for-the-template)
II. [Writing a Template code](#section-ii-writing-a-template-code) 


### I. Preparing Data for the Template
Once you have a template created, we should add some data that would feed the template.
First, create a Class:
1. Go to Classes page
2. Click "ADD" button
3. Type "cat_data" as Class name
4. Add 3 schema fields: 
- `description` as `string` type 
- `rank` as `integer` type (Click `order` checkbox. We will need it later)
-  `gif` as string type.
5. Click "CONFIRM" button

Now it's time to create Data Objects: 
1. Go to the Cats Class Data Object view.
2. Click "+" button and fill the `description`, `rank` and `gif` fields with random data.
3. For additional awesomeness, you can use the urls below for the `gif` field:
http://bit.ly/1q6BjD2
http://bit.ly/1TC9LBr
http://bit.ly/1Vx7M1g
http://bit.ly/1SABmyb
http://bit.ly/1SQ3waZ

After you are done, your Data Objects view should look more or less like this (I removed all non custom fields so you can see only `description`, `rank` and `gif` on the image):

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/RiMWNf2dTw6SYuUVx1ow",
        "data_objects_view.png",
        "2554",
        "1306",
        "#264672",
        ""
      ]
    }
  ]
}
[/block]
Once you have the data in place, we can start creating a Template for it!

### II. Writing a template code 
Go back to templates view and click on `cats` list item. You'll see an editor that is similar to Scripts code editor. Paste this code into the editor:

[block:code]
{
  "codes": [
    {
      "code": "{% set objects = response.objects %}\n{% if objects %}\n  <div class=\"container\">\n    <table class=\"table\">\n      <tr>\n        {% for key in ['rank', 'description', 'gif'] %}\n          <th>{{ key }}</th>\n        {% endfor %}\n      </tr>\n        {% for object in objects %}\n      <tr>\n        <td>{{ object['rank'] }}</td> \n        <td>{{ object['description'] }}</td> \n        <td>\n          <img src=\"{{ object['gif'] }}\">\n        </td>              \n      </tr>\n    {% endfor %}\n    </table>\n  </div>\n{% endif %}",
      "language": "jinja2"
    }
  ]
}
[/block]
As mentioned earlier, the syntax used in templates is [jinja](http://jinja.pocoo.org/docs/dev/templates/). Let's make a rundown of what's happening here. 
- 1st line assigns the response to objects variable 
- 2nd line checks if the objects dict is not empty
- 3-5th lines probably look familiar to you. They are simply opening html tags that are base for our document
- 6th line is where the magic&trade; happens. We iterate through keys that we want to have as table column names and assign them to subsequent <th> tags
- In 10th line we start iterating through objects from the response.
- In lines 12-16 we fill the table cells with the data from Data Objects' that we created in previous steps
[block:api-header]
{
  "type": "basic",
  "title": "Rendering a response"
}
[/block]
Now let's go ahead and render the template! If you followed this tutorial, a link to render a template should look more or less like this:

[block:code]
{
  "codes": [
    {
      "code": "https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/cats_data/objects/?template_response=cats&api_key=API_KEY",
      "language": "text"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "A bit of explanation is needed here. The above url is simply a Syncano API endpoint that directs to Data Objects inside the `cats_data` Class. \nWhat's different here, is the extra `template_response` parameter that we pass with the request. It takes `cats` as a value, because this is the template name we want to use, to render our data.",
  "title": "template_response parameter"
}
[/block]
- Replace the INSTANCE_NAME with your Instance and API_KEY with either Account Key or Instance API Key (`allow_anonymous_read` set to `true` and Data Objects created with `other` [permissions equal to `read` or higher](http://docs.syncano.io/docs/permissions#using-api-key-with-allow_anonymous_read-flag)).
- Now paste the url into the browser

Instead of pasting the url into the browser you can also use the preview function: 
- Paste the url into the `Data Source Url` input.
- Click "Render" to see 'raw' preview in the editor 
- Click "Render in tab" to see the preview in a new browser tab
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/FvwDVXcPSemKw7ZtpKfA",
        "render_template.png",
        "1279",
        "655",
        "#22497a",
        ""
      ]
    }
  ]
}
[/block]
 
You should see an html table with cat gifs! Unfortunately the table has no styling whatsoever and is just plain ugly. Let's add some css to make it look professional:

1. Go back to the cats Template Snippet
2. Paste the code below to the top of your Template and save it
[block:code]
{
  "codes": [
    {
      "code": "<style>\n.table {\n  margin: 0 auto;\n  min-width: 600px;\n  overflow: hidden;\n  background: #fff;\n  color: #757575;\n}\n.table tr {\n  border: 1px solid #f6f6f6;\n  background-color: #ebf1fe;\n}\n.table tr:nth-child(odd) {\n  background-color: #f6f6f6;\n}\n.table th {\n  display: table-cell;\n  text-align: center;\n  text-transform: capitalize;\n  border: 1px solid #fff;\n  background-color: #244273;\n  color: #FFF;\n  padding: 1em;\n}\n\n.table td {\n  display: table-cell;\n  text-align: center;\n  word-wrap: break-word;\n  max-width: 7em;\n  margin: .5em 1em;\n  padding: 1em;\n}\n.table img {\n  height: 9em;\n  max-width: 100%;\n}\n\n.container {\n  width: 100%;\n}\n\nbody {\n  padding: 0 2em;\n  font-family: Avenir, sans-serif;\n  color: #757575;\n  background: #fff;\n}\n</style>",
      "language": "html"
    }
  ]
}
[/block]

Now go back to the browser tab with the html table and refresh it. You should see a complete, functional, user-friendly cat table just like the one below:
 
[block:embed]
{
  "html": false,
  "url": "https://api.syncano.rocks/v1.1/instances/holy-bush-9948/classes/cats/objects/?template=new_cats&api_key=356fc871f02aba74713e717a0b4191c2c71f4d2b",
  "title": null,
  "favicon": null,
  "iframe": true
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Bonus stage! (ordering the response)"
}
[/block]
Look at the ranking in the above table. It's in incorrect order. To fix this issue we could add some more logic to the template but why not use Data Endpoints?

1. Go to Instance -> Sockets view
2. Click "ADD" button and again "ADD" button in Data Endpoint section
3. Name the Data Endpoint `cat_gifs_ranking`
4. Choose a 'cats' class
5. In the response options choose `order by` `rank (ascending)`
6. Click "CONFIRM" button
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/ZgSGDL4RGqfNPAeQjyDZ",
        "Add_data_endpoint_01.png",
        "1279",
        "655",
        "#2a4667",
        ""
      ]
    }
  ]
}
[/block]
Now you can create a template from this Data Endpoint and it will be sorted in an ascending order. 
[block:callout]
{
  "type": "info",
  "title": "Hint",
  "body": "you can easily copy the Data Endpoint url by clicking on the pin icon:"
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/9I3wLiECSqGfKLFSM3sq",
        "data_endpoint_list.png",
        "978",
        "70",
        "#282328",
        ""
      ]
    }
  ]
}
[/block]
The url should look like this now:
[block:code]
{
  "codes": [
    {
      "code": "https://api.syncano.io/v1.1/instances/INSTANCE_NAME/endpoints/data/cat_gifs_ranking/get/?api_key=API_KEY&template_response=cats",
      "language": "text"
    }
  ]
}
[/block]
That's it. Now you should be able to view the list of cat gifs ordered by ranking!
[block:api-header]
{
  "type": "basic",
  "title": "Template Context"
}
[/block]
Template Context is a concept similar to Config dictionary in Snippet Scripts. It's an object used to store some additional variables that can be accessed from a Template Snippet. This is how a Context object can look like:

[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"very_important_value_0\": \"42\",\n\t\"very_important_value_1\": null,\n\t\"array_with_stuff\": [1, \"two\", false]\n}",
      "language": "json"
    }
  ]
}
[/block]
Accessing these values from the Template is easy. If you wanted to get the value from `very_important_value_0` property, you would add this code to the Template:
[block:code]
{
  "codes": [
    {
      "code": "{{ very_important_value_0 }}",
      "language": "jinja2"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Response types in Templates"
}
[/block]
When you look at the first couple of lines in a default `objects_html_table` template snippet created in the dashboard, you should see this code:
[block:code]
{
  "codes": [
    {
      "code": "{% if action == 'list' %}\n    {% set objects = response.objects %}\n{% elif action == 'retrieve' %}\n    {% set objects = [response] %}\n{% else %}\n    {% set objects = [] %}\n{% endif %}",
      "language": "jinja2"
    }
  ]
}
[/block]
This is because the data is accessed differently based on the HTTP request type. See the table below to learn how to assign the data properly:

[block:parameters]
{
  "data": {
    "h-0": "HTTP Request",
    "0-0": "`GET`",
    "0-1": "list",
    "h-1": "Action",
    "1-0": "`GET`",
    "h-2": "Access from template",
    "0-2": "`{% set objects = response.objects %}`",
    "2-0": "`POST`",
    "h-3": "Comment",
    "1-1": "retrieve",
    "3-0": "`PUT`",
    "4-0": "`PATCH`",
    "5-0": "`DELETE`",
    "3-1": "update",
    "4-1": "partial_update",
    "5-1": "N/A",
    "1-2": "`{% set objects = [response] %}`",
    "2-1": "create",
    "5-3": "Delete method returns empty content.",
    "5-2": "N/A",
    "2-2": "`{% set objects = [response] %}`",
    "3-2": "`{% set objects = [response] %}`",
    "4-2": "`{% set objects = [response] %}`",
    "0-3": "`list` action is for endpoints  that list multiple items (Data Objects, Api Keys, Classes, Sockets etc.)",
    "1-3": "`retrieve` action is for item details (Single Data Object, API Key, Class etc.)"
  },
  "cols": 4,
  "rows": 6
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Summary"
}
[/block]
Now you know how to:
- Create a Template
- Write Template code
- Render Template responses
- Use Template Context dictionary.

You can now:
- Check out the [API Reference section](http://docs.syncano.io/v0.1.1/docs/templates-list) for the Template Snippets to view all the possible methods
- Read more about [Data Endpoints](http://docs.syncano.io/docs/endpoints-data) which were just briefly mentioned in this chapter.