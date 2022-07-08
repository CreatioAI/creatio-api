# Creatio

## PRE-Requisites
- Install [Node](https://nodejs.org/en/)
- Install [Visual Studio Code](https://code.visualstudio.com/)
- Install [Curl](https://curl.se/download.html)
- Install [Yeoman](https://yeoman.io/).

## How to create custom template on Creatio


Download and install the generator via npm:
```bash
$ npm i -g @creatioai/generator-helloworldtemplate
```
 
Use the yeoman:
```bash
$ yo @creatioai/helloworldtemplate
```

Modify files and fill it with the information of your template.

Upload template:
```bash
-curl -X post -H "Bearer YOUR_TOKEN_HERE" http://app-creatio-api-dev-weu.azurewebsites.net/api/templates 
```

Get the new template:
```bash
curl -X 'GET' \
  'http://app-creatio-api-dev-weu.azurewebsites.net/api/templates/YOUR_TEMPLATE_ID_HERE' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer YOUR_TOKEN_HERE'
```

Create video using the new template:
```bash
curl -X 'POST' \
  'http://app-creatio-api-dev-weu.azurewebsites.net/api/videos' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer YOUR_TOKEN_HERE' \
  -H 'Content-Type: application/json' \
  -d '[
  {
    "templateId": "YOUR_TEMPLATE_ID_HERE",
    "variables": [
    {
        "properties" : {}, 
        "variableName": "YOUR_SCENE_WIDGET_VARIABLENAME_HERE",
        "value": "YOUR_DESIRED_VALUE_HERE"
    },
    {
        "properties" : {}, 
        "variableName": "YOUR_SCENE_WIDGET_VARIABLENAME_HERE",
        "value": "YOUR_DESIRED_VALUE_HERE"
    }
    ],
    "webhook": {
        "url": "",
        "token": ""
    },
    "externalId": "YOUR_VIDEO_NAME_HERE",
    "properties": {},
]'

```

Download video (return 404 if not processed yet):
```bash
curl -X 'GET' \
  'http://app-creatio-api-dev-weu.azurewebsites.net/api/videos/YOUR_VIDEO_ID_HERE/download' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer YOUR_TOKEN_HERE'
```

> If you're an advanced user, you can check our Swagger at the following URL: http://app-creatio-api-dev-weu.azurewebsites.net/swagger/index.html#/
