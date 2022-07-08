# Creatio

## How to create custom template on Creatio

Install [Yeoman](https://yeoman.io/).

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
-curl -X post -H "Bearer YOUR_TOKEN_HERE" https://api.creatio.com/api/templates 
```

Get the new template:
```bash
curl -X 'GET' \
  'https://localhost:44353/api/templates/YOUR_TEMPLATE_ID_HERE' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer YOUR_TOKEN_HERE'
```

Create video using the new template:
```bash
curl -X 'POST' \
  'https://localhost:44353/api/videos' \
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
  'https://localhost:44353/api/videos/YOUR_VIDEO_ID_HERE/download' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer YOUR_TOKEN_HERE'
```
