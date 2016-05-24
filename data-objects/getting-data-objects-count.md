---
title: "Getting count estimation"
excerpt: ""
---
[block:api-header]
{
  "type": "basic",
  "title": "Data Objects Count"
}
[/block]
You can get the Data Objects count **estimation** when listing your objects. Syncano gives an exact count for Classes that have less than 1000 Data Objects and an estimate for Classes that have more than 1000 Data Objects.

Here's how you can view the Data Objects count estimation:
[block:code]
{
  "codes": [
    {
      "code": "# page_size set to zero to exclude any Data Objects from the response for clarity\n\ncurl -X GET -G \\\n-H \"X-API-KEY: API_KEY\" \\\n-d include_count=true \\\n-d page_size=0 \\\n\"https://api.syncano.io/v1.1/instances/INSTANCE_NAME/classes/CLASS_NAME/objects/\"",
      "language": "curl"
    },
    {
      "code": "import syncano\nfrom syncano.models.base import Class\n\nconnection = syncano.connect(api_key='API_KEY')\nklass = Class.please.get(instance_name='INSTANCE_NAME', name='CLASS_NAME')\nklass.objects.count()\n                             \n# or:\n\nObject.please.list(\n  instance_name='INSTANCE_NAME', class_name=\"CLASS_NAME\").count()",
      "language": "python"
    },
    {
      "code": "// Planned for v1.1.0",
      "language": "javascript"
    },
    {
      "code": "Response<Integer> response = Syncano.please(Book.class).getCountEstimation();\nInteger count = response.getData();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "SCPlease *please = [Book please];\n[please giveMeDataObjectsWithParameters:@{SCPleaseParameterIncludeCount : @(YES)} completion:^(NSArray *objects, NSError *error) {     \n  NSNumber *objectsCount = please.objectsCount;\n}];",
      "language": "objectivec"
    },
    {
      "code": "let please:SCPlease = Book.please()\nplease.giveMeDataObjectsWithParameters([SCPleaseParameterIncludeCount : true]) { (objects, error) in\n    let objectsCount:NSNumber = please.objectsCount\n}",
      "language": "objectivec",
      "name": "Swift"
    }
  ]
}
[/block]
Response:
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"prev\":null,\n  \"objects_count\":48843,\n  \"objects\":[],\n  \"next\":null\n}",
      "language": "json"
    },
    {
      "code": "48837",
      "language": "python"
    }
  ]
}
[/block]