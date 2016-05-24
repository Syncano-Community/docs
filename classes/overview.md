---
title: "Class Overview"
excerpt: "What is a Syncano Class, and why do I need it?"
---
## Topics:
1. [Overview](#overview)
4. [Class Status](#class-status)
5. [Class Data Objects count](#class-data-objects-count)

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
**Classes** are templates for data objects you will store in Syncano. In order to be able to [add Data Objects](data-objects#section-creating-a-data-object), you have to define a **Class** for that type of data object. To [create a class](#creating-a-class) you have to provide:

- The name of the class
- A description (optional)
- A schema


[block:callout]
{
  "type": "info",
  "title": "Limits",
  "body": "For paid accounts, maximum number of Classes that can be created within an Instance is `100`."
}
[/block]


[block:api-header]
{
  "type": "basic",
  "title": "Class Status"
}
[/block]
Each class has a status, which is one of two values:
- `ready` - means that the Class is ready and Data Objects can be created within the Class.
- `migrating` - class schema and relations are being set up. It's not possible to create Data Objects within the Class.

A Class can have a `migrating` status in two situations:
- after the Class was created
- after the Class schema was edited.

Class migrations usually don't take longer than a couple of seconds. If your app doesn't rely on editing or creating Classes, you won't be affected by this behavior.

[block:api-header]
{
  "type": "basic",
  "title": "Data Objects count"
}
[/block]
As I mentioned in the beginning of this chapter, Classes define a structure for the Data Objects to base their properties on. Another cool feature of Classes, is that you can see a number of Data Objects that inherit from the Class.
To see that, go to `Dashboard` -> `Classes` view. The `Objects` column on the Classes list will tell you the current number of Data Objects:
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/w3mNoLYHQVSn2b9lHHkY",
        "classes_foo.png",
        "2050",
        "699",
        "#265eb5",
        ""
      ]
    }
  ]
}
[/block]
As you can see, there are `5` Objects in the Class called, wait for it.. `class`!
That's is quite straightforward.
What can be a bit more surprising is the object count for the `trigger` Class -- which is `~ 48837`. The `~` sign shows that this is an **estimation**. Syncano shows an estimate count for Classes that have more than 1000 Data Objects. The reason for it is that we can't provide a precise count without affecting the performance of the platform.
[block:callout]
{
  "type": "info",
  "title": "Did you know?",
  "body": "You can also get count of Data Objects using our API, when making a call directly to the Data Objects endpoint! \nSee the [Data Objects](data-objects) chapter for details."
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Learn More",
  "body": "For more details about the available methods for classes, visit the [Classes API reference](http://docs.syncano.com/v0.1.1/docs/classes-list)."
}
[/block]
