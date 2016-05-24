---
title: "Python"
excerpt: ""
---
[block:callout]
{
  "type": "info",
  "body": "This is just a Getting started section. Here is a full [Python Library Reference](http://syncano.github.io/syncano-python/index.html)"
}
[/block]
Getting Started with Syncano
---------------------------------------

This tutorial will walk you through installing and configuring Syncano, as
well as how to use it to make API calls.

This tutorial assumes you are familiar with Python & that you have a [Syncano account](https://dashboard.syncano.io/#signup).

Installing Syncano
------------------------

You can use ``pip`` to install the latest released version of Syncano:
[block:code]
{
  "codes": [
    {
      "code": "pip install syncano",
      "language": "shell"
    }
  ]
}
[/block]
If you want to install Syncano from the source:
[block:code]
{
  "codes": [
    {
      "code": "git clone https://github.com/Syncano/syncano-python.git\ncd syncano-python\npython setup.py install",
      "language": "shell"
    }
  ]
}
[/block]
Using Virtual Environments
------------------------------------

Another common way to install Syncano is to use a ``virtualenv``, which
provides isolated environments. First, install the ``virtualenv`` Python
package:
[block:code]
{
  "codes": [
    {
      "code": "pip install virtualenv",
      "language": "shell"
    }
  ]
}
[/block]
Next, create a virtual environment by using the ``virtualenv`` command and
specifying where you want the virtualenv to be created (you can specify
any directory you like, though this example allows for compatibility with
``virtualenvwrapper``):
[block:code]
{
  "codes": [
    {
      "code": "mkdir ~/.virtualenvs\nvirtualenv ~/.virtualenvs/syncano\n",
      "language": "shell"
    }
  ]
}
[/block]
You can now activate the virtual environment:
[block:code]
{
  "codes": [
    {
      "code": "source ~/.virtualenvs/syncano/bin/activate\n",
      "language": "shell"
    }
  ]
}
[/block]
Now, any usage of ``python`` or ``pip`` (within the current shell) will default
to the new, isolated version within your virtualenv.

You can now install Syncano into this virtual environment:
[block:code]
{
  "codes": [
    {
      "code": "pip install syncano",
      "language": "shell"
    }
  ]
}
[/block]
When you are done using Syncano, you can deactivate your virtual environment:
[block:code]
{
  "codes": [
    {
      "code": "deactivate",
      "language": "shell"
    }
  ]
}
[/block]
If you are creating a lot of virtual environments, [virtualenvwrapper](http://virtualenvwrapper.readthedocs.org/en/latest/) is an excellent tool that lets you easily manage your virtual environments.

Making Connections
---------------------------

Syncano provides a number of convenience functions to simplify connecting to our services:
[block:code]
{
  "codes": [
    {
      "code": ">>> import syncano\n>>> connection = syncano.connect(email='YOUR_EMAIL', password='YOUR_PASSWORD')\n",
      "language": "python"
    }
  ]
}
[/block]


If you want to connect directly to a chosen instance you can use the `connect_instance()` function:
[block:code]
{
  "codes": [
    {
      "code": ">>> import syncano\n>>> connection = syncano.connect_instance(instance_name='instance_name', email='YOUR_EMAIL', password='YOUR_PASSWORD')",
      "language": "python"
    }
  ]
}
[/block]
   

If you have obtained your ``Account Key`` from the website you can omit ``email`` & ``password`` and pass ``Account Key`` directly to connection:
[block:code]
{
  "codes": [
    {
      "code": ">>> import syncano\n>>> connection = syncano.connect(api_key='YOUR_API_KEY')\n>>> connection = syncano.connect_instance(instance_name='instance_name', api_key='YOUR_API_KEY')",
      "language": "python"
    }
  ]
}
[/block]

Interacting with Syncano
--------------------------------

The following code demonstrates how to create a new `Instance`
and add an `ApiKey` to it:
[block:code]
{
  "codes": [
    {
      "code": ">>> import syncano\n>>> connection = syncano.connect(api_key='abcd')\n>>> instance = connection.instances.create(name='dummy_test', description='test')\n>>> instance\n<Instance: dummy_test>\n\n>>> api_key = instance.api_keys.create()\n>>> api_key\n<ApiKey: 47>\n>>> api_key.api_key\nu'aad17f86d41483db7088ad2549ccb87902d60e45'",
      "language": "python"
    }
  ]
}
[/block]
Each model has a different set of fields and commands. For more information check [available models](http://syncano.github.io/syncano-python/models.html#models).

Next Steps
---------------

If you'd like more information on interacting with the Syncano via python library, check out the [interacting tutorial](http://syncano.github.io/syncano-python/interacting.html#interacting) or if you want to know what kind of models are available check out the [available models](http://syncano.github.io/syncano-python/refs/syncano.html#subpackages) list.