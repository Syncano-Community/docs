---
title: "Arduino"
excerpt: ""
---
1. [Overview](#overview)
2. [Installing the Syncano Arduino Library](#installing-the-syncano-arduino-library)
3. [Using the Syncano library](#using-the-syncano-library)
4. [Example - Temperature sensor](#example-temperature-sensor)
5. [Support](#support)
[block:callout]
{
  "type": "info",
  "title": "Sockets release",
  "body": "We recently released a new version of our API that uses Scripts instead of CodeBoxes and Script Endpoints instead of Webhooks.\nFor us, it's not just about the name change - you can [read about it](https://www.syncano.io/blog/what-are-we-cooking-up-with-sockets/) on our blog.\nFor now, you can keep using old methods, and when we release new version of this library, we will update our docs here."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Overview"
}
[/block]
This guide will walk you through the installation of the Syncano Library for Arduino, with a complete example at the end.

If you don't have Syncano account yet, you can read about how to create one [here](doc:getting-started-with-syncano), or sign up directly in our [Dashboard](http://dashboard.syncano.io/#/signup).
[block:api-header]
{
  "type": "basic",
  "title": "Installing the Syncano Arduino Library"
}
[/block]
Simplest way to get Syncano Arduino library is to download the source from [GitHub](https://github.com/Syncano/syncano-arduino). 
[block:callout]
{
  "type": "info",
  "title": "Arduino IDE",
  "body": "If you use Arduino IDE environment, download entire repository as a ZIP file. \n  \nNext, open IDE and choose `Sketch -> Include library -> Add .ZIP Library`."
}
[/block]
If you use other IDE, manually place all files in your current project directory.

Last step is to include the library in your project files by adding at the beginning following line:
[block:code]
{
  "codes": [
    {
      "code": "#include <Syncano.h>",
      "language": "cplusplus",
      "name": "C++"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Using the Syncano library"
}
[/block]
1. [Connect to Syncano](#section-connect-to-syncano)
2. [Add your class](#section-add-your-class)
3. [Get a single Data Object](#section-get-a-single-data-object)

## Connect to Syncano

Establishing connection needs an API key (read more about creating it in our [Authentication & API Keys section](authentication#section-creating-an-api-key)). 

Code below shows all commands needed to establish a connection to Syncano on working Arduino. Last line enables user to select a Syncano Instance to which connection will be made.
[block:code]
{
  "codes": [
    {
      "code": "#define ACCOUNT_KEY \"API_KEY\"\n#define INSTANCE_NAME \"INSTANCE_NAME\"\n\n// Create pointer for Syncano client\nSyncanoClient* syncano;\n\n// Create Syncano client\ninitSyncanoClient(ACCOUNT_KEY);\n\n// Setup Syncano client used to connect\nsyncano = getSyncanoClient();\n\n// Set Syncano client variables from #define\nsyncano->setInstanceName(INSTANCE_NAME);",
      "language": "cplusplus"
    }
  ]
}
[/block]
## Add your class

In order to exchange data with Syncano, create local version of class directly on the device.
[block:callout]
{
  "type": "info",
  "title": "Note to remember",
  "body": "Remember that IoT devices usually don't have a lot of system resources. \nIn order to comply with that, data send and received from Syncano will contain only fields added in class container."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "#define CLASS_NAME \"CLASS_NAME\"\n#define CLASS_DESCRIPTION \"CLASS_DESCRIPTION\"\n\n// Create pointer for class\nSyncanoClass* classHolder;\n\n// Create local class\nclassHolder = new SyncanoClass(CLASS_NAME,CLASS_DESCRIPTION);\n    \n// Add fields to class\nclassHolder->addField(\"id\");\nclassHolder->addField(\"firstname\");\nclassHolder->addField(\"lastname\");\n\n// Initialize class, remember that you cannot add new fields after this command\nclassHolder->initClass();",
      "language": "cplusplus"
    }
  ]
}
[/block]
## Get a single Data Object

Data Objects are tied strictly to Classes from example above. Remember to have a class created before proceeding to downloading data objects from, or adding new object on Syncano.
[block:code]
{
  "codes": [
    {
      "code": "#define CLASS_NAME \"CLASS_NAME\"\n#define CLASS_DESCRIPTION \"CLASS_DESCRIPTION\"\n\n// Create pointers for class and data object\nSyncanoClass* classHolder;\nSyncanoDataObject* object;\n\n// Create local class\nclassHolder = new SyncanoClass(CLASS_NAME,CLASS_DESCRIPTION);\n    \n// Add fields to class\nclassHolder->addField(\"id\");\nclassHolder->addField(\"firstname\");\nclassHolder->addField(\"lastname\");\n\n// Initialize class, remember that you cannot add new fields after this command\nclassHolder->initClass();\n\n// Initialize empty object based on class\nobject = classHolder->initObject();\n\n// Insert data into object fields\nobject->setFieldValue(\"firstname\",\"John\");\nobject->setFieldValue(\"lastname\",\"Smith\");\n\n// Send object to Syncano\nobject->add();\n\n// Parse object from Syncano, this overwrite local object fields\nobject->details(OBJECT_ID);\n\n// Print the object fields with values to Serial to see results\nobject->printDetails();",
      "language": "cplusplus"
    }
  ]
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Example - Temperature sensor"
}
[/block]
1. [Using Arduino Yún with Arduino Syncano Library](#section-using-arduino-y-n-with-arduino-syncano-library)
2. [Step 1. Before you start](#section-step-1-before-you-start)
3. [Step 2. Connect it all together](#section-step-2-connect-it-all-together)
4. [Step 3. Syncano configuration](#section-step-3-syncano-configuration)
5. [Step 4. Prepare Arduino](#section-step-4-prepare-arduino)
6. [Step 5. Main program](#section-step-5-main-program)
7. [Step 6. Run!](#section-step-6-run-)

### Using Arduino Yún with Arduino Syncano Library

Usually when learning new things, one starts from something simple. In programming, it usually is a "Hello world" applications. 
What can we do when it comes to `IoT` world? Answer is simple - either a `blinking diode` or a `temperature sensor`.

This time we are going to create the second example and build a simple weather station. Data gathered by device will be send to Syncano using our library. All communication will be performed using secure HTTPS.

After completing this tutorial, you will have access to a personal weather information on any device connected to Syncano!
[block:callout]
{
  "type": "success",
  "body": "Assemble device which will be able to send temperature data directly to Syncano using Syncano Arduino Library.",
  "title": "Main goal:"
}
[/block]
## Step 1. Before you start
In order to create a simple weather station, we will need things from the list below:
[block:parameters]
{
  "data": {
    "h-0": "Component",
    "h-1": "Count",
    "h-2": "Image",
    "0-0": "Arduino Yún",
    "0-1": "`1`",
    "0-2": "![Arduino Yún](https://docid81hrs3j1.cloudfront.net/contents/small/Yun-500_7T9vAd1.jpg \"Arduino Yún\")",
    "1-0": "DS18s20+ temp sensor",
    "1-1": "`1`",
    "1-2": "![ds18s20](http://midondesign.com/IMAGES/DS18S20_pinout.JPG \"ds18s20\")",
    "2-0": "MicroUSB cable",
    "2-1": "`1`",
    "2-2": "![microUSB cable](http://www.voltonics.com/wp-content/uploads/2015/08/Mucho-Charge-and-Sync-Micro-USB-Cable-for-All-your-Micro-USB-Mobile-Phones-and-Tablets-with-4-Feet-12-mtr-Length-BLACK-0-128x128.jpg \"microUSB\")",
    "3-0": "Jumper wires",
    "3-1": "`3`",
    "3-2": "![jumper wires](http://miniimg8.rightinthebox.com/images/128x128/201211/cadcnm1354280342603.jpg \"jumper wires\")",
    "4-0": "Breadboard",
    "4-1": "`1`",
    "4-2": "![breadboard](http://miniimg6.rightinthebox.com/images/m/201211/gggdik1354248254351.jpg \"breadboard\")",
    "5-0": "4.7k Resistor",
    "5-1": "`1`",
    "5-2": "![4.7k resistor](\nhttp://miniimg4.rightinthebox.com/images/128x128/201412/gbaghf1417842745688.jpg \"4.7k resistor\")"
  },
  "cols": 3,
  "rows": 6
}
[/block]
## Step 2. Connect it all together  
Assembly is really easy and shouldn't cause any problems. One important thing to remember is the `pull-up resistor` between the Arduino and a temperature sensor.

![assembly diagram](https://s3.eu-central-1.amazonaws.com/eyedea.ninja/static/ArduinoYUNDS18s20.png "jumper wires")  

## Step 3. Syncano configuration  
In this step we prepare Syncano for use with an IoT device. Main steps are creating data holder and assuring proper access control.  

 * Class
   * We have to create a class which will store your data. In this example, we need a field for temperature value and device name. Our sensor will return numbers like: `21.5`, `25.0`, `22.5` ... etc. In this case we will use `FLOAT` variable type. For device name we will use `STRING` variable type.
	  
* API Key
  * For proper security, create new API Key in your Instance (see how to do it in [Authentication & API Keys section](authentication#section-creating-an-api-key) of our docs) - we will use it for establishing connection between Syncano and Arduino.

## Step 4. Prepare Arduino  
In this step we connect our device to Internet and do basic setup before moving on.

 * Connect Arduino to WiFi network
   * We recommend to follow a great guide directly from Arduino website: [Configure WiFI Guide](https://www.arduino.cc/en/Guide/ArduinoYun#toc14)
  
* Extend storage for OpenWRT (Linux inside)
  * Follow guide from Arduino website: [Extend storage for OpenWRT](https://www.arduino.cc/en/Tutorial/ExpandingYunDiskSpace)

Now, connect to your Arduino and install SSL packages using commands below:
[block:code]
{
  "codes": [
    {
      "code": "ssh root@arduino.local\nopkg update\nopkg install python-openssl",
      "language": "shell",
      "name": "Bash"
    }
  ]
}
[/block]
## Step 5. Main program  
[block:callout]
{
  "type": "info",
  "title": "Note about libraries",
  "body": "Sketch below needs 3 external libraries, which you should install before compiling your project. \n\nTo install them, simply download ZIP files with libraries and include them inside your project using Arduino IDE:\n`Sketch -> Include Library -> Add *.ZIP Library`\n\nLinks to libraries we used:\n* [OneWire Library](http://www.pjrc.com/teensy/arduino_libraries/OneWire.zip)\n* [DallasTemperature Library](https://github.com/milesburton/Arduino-Temperature-Control-Library/archive/master.zip)\n* [Syncano Library](https://github.com/Syncano/syncano-arduino/archive/master.zip)"
}
[/block]

Below you can find source code responsible for sending the temperature to Syncano. Sending data is invoked only on temperature changes to avoid sending same data multiple times.

[block:code]
{
  "codes": [
    {
      "code": "#include <Syncano.h>                           // Syncano Arduino library\n#include <Bridge.h>\n#include <OneWire.h>                           // Library used to connect OneWire sensors\n#include <DallasTemperature.h>                 // DS18s20 sensor library\n\n#define ONE_WIRE_BUS 2                         // DS18B20+ pin\n\n#define ACCOUNT_KEY \"YOUR_API_KEY\"\n#define INSTANCE_NAME \"INSTANCE_NAME\"          // Name of the Syncano Instance\n#define CLASS_NAME \"INSTANCE_NAME\"             // Name of the Syncano Class\n#define CLASS_DESCRIPTION \"CLASS_DESCRIPTION\"  // Name of the Syncano\n#define DEVICE_ID \"MY_YUN\"                     // ID of the device\n\n// Initialize temperature sensor\nOneWire oneWire(ONE_WIRE_BUS);\nDallasTemperature DS18B20(&oneWire);\n\n// Create pointer for Syncano client\nSyncanoClient* syncano;\n\n// Create pointers for class and data object\nSyncanoClass* classHolder;\nSyncanoDataObject* object;\n\nfloat lastTemp = 0;\n\n// Read temperature from sensor, return value as float\nfloat readTemperature()\n{\n  float temperature;\n\n  do {\n    DS18B20.requestTemperatures();\n    temperature = DS18B20.getTempCByIndex(0);\n  } while (temperature == 85.0 || temperature == (-127.0));\n\n  return temperature;\n}\n\nvoid setup() {\n  initSyncanoClient(ACCOUNT_KEY);\n  Serial.begin(115200);\n\n  // Call function used to initialize bridge and check wifi connection\n  startBridge();\n  checkWifiStatus();\n\n  // Setup Syncano client used to connect\n  syncano = getSyncanoClient();\n\n  // Set Syncano client variables from #define\n  syncano->setInstanceName(INSTANCE_NAME);\n\n  // Create example class\n  classHolder = new SyncanoClass(CLASS_NAME,CLASS_DESCRIPTION);\n\n  // Add fields to class\n  classHolder->addField(\"celcius\");\n  classHolder->addField(\"name\");\n\n  // Initialize class, remember that you cannot add new fields after this command\n  classHolder->initClass();\n\n  // Initialize empty object based on class\n  object = classHolder->initObject();\n\n  // Put a name for your device\n  object->setFieldValue(\"name\", DEVICE_ID);  // Put a name for your device\n}\n\nvoid loop() {\n  float temperature = readTemperature();\n  if (temperature != lastTemp)\n  {\n    // Put temperature inside local Data Object\n    object->setFieldValue(\"celcius\",String(temperature,2));\n\n    // Send object to Syncano\n    object->add();\n\n    // Print information about new temperature to Serial\n    Serial.print(\"New temperature:\");\n    Serial.println(temperature);\n\n    // Save last temperature to avoid creating Data Object with the same values\n    lastTemp = temperature;\n  }\n  delay(1000);\n}\n\n// Start Arduino Bridge, used to connect Arduino processor to OpenWRT Linux\nvoid startBridge(){\n  while(!Serial);\n  Serial.println(F(\"Serial ready.\"));\n  Serial.println(F(\"Starting bridge...\\n\"));\n  pinMode(13,OUTPUT);\n  digitalWrite(13, LOW);\n  Bridge.begin();\n  digitalWrite(13, HIGH);\n}\n\n// Check wifi status\nvoid checkWifiStatus(){\n  delay(1000);\n  Process wifiCheck;\n  wifiCheck.runShellCommand(\"/usr/bin/pretty-wifi-info.lua\");\n  while (wifiCheck.available() > 0) {\n    char c = wifiCheck.read();\n    Serial.print(c);\n  }\n  Serial.println();\n  delay(500);\n}",
      "language": "cplusplus",
      "name": "SimpleTemperature.ino"
    }
  ]
}
[/block]

## Step 6. Run!
After creating new sketch and pasting source code from previous step, you can compile and upload your code into Arduino device. 

Next, open a Serial Monitor - Arduino will wait with program start until Serial connection is established. You should see information about `New Temperature` on the screen. In the background, data will be sent to Syncano - each time temperature changes, new object will be created with temperature value.
[block:api-header]
{
  "type": "basic",
  "title": "Support"
}
[/block]
Now you’re ready to use Syncano in your Arduino project. We really hope you will enjoy working with our platform. If you have any issues or suggestions, let us know at support@syncano.com.