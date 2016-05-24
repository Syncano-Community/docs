---
title: "Creating a Class"
excerpt: ""
---

[block:callout]
{
  "type": "warning",
  "title": "Class permissions",
  "body": "This section explains how to Create Classes as a Syncano Administrator (which is you). If you'd like your users to be able to interact with Classes and their Data Objects, you must make sure to use the correct Class [permissions](doc:permissions)."
}
[/block]

### Creating a Class from the Dashboard
To create a class, you choose the respective instance, and then click  "Classes" in sidebar as shown below.
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/lWafsIguQO6oHViTjLRQ",
        "Add_class_01.png",
        "1213",
        "622",
        "#26426f",
        ""
      ],
      "caption": "Adding class"
    }
  ]
}
[/block]
Click the the plus button, add class name and the schema (field names and types) to your Class.  Additionally you can also add a description, and decide if you you want the field to have `Filter` or `Order` index (more on indexes [here](#section-indexes)).
[block:image]
{
  "images": [
    {
      "image": [
        "https://www.filepicker.io/api/file/GhGfdMT6Rjahwc8mEg8V",
        "Add_class_02.png",
        "1279",
        "653",
        "#6c7c9c",
        ""
      ]
    }
  ]
}
[/block]

Filtering and ordering is explained in detail in the [Data Objects Filtering & Ordering](doc:data-objects-filtering) chapter.


[block:callout]
{
  "type": "warning",
  "title": "index fields limit",
  "body": "You can add only 16 fields of type \"orderIndex\" or \"filterIndex\". For example: a class schema could contain 10 filter indexes and 6 order indexes (8 in total)."
}
[/block]
