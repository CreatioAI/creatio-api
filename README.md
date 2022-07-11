# How to start with Creatio

## PRE-Requisites
#### These are the following pre-requisites to perform this tutorial:
- [Node](https://nodejs.org/en/) (v6 or higher)
- Npm 3 or higher (which comes bundled with Node)
- Git 
- [Visual Studio Code](https://code.visualstudio.com/) (or similar IDE)
- Download [Curl](https://curl.se/windows/) for Windows (Linux and iOS is already installed), include the bin folder in path.
- Install [Yeoman](https://yeoman.io/)
- Ask for a token at the following email: support.creatio@creatio.ai

## How to create a custom template with Creatio

Open a terminal on your computer (cmd, bash, terminal...)

Download and install the generator via npm:
```bash
$ npm i -g @creatioai/generator-helloworldtemplate
```
 
Use yeoman to start with "Hello world template":
```bash
$ yo @creatioai/helloworldtemplate
```

Open the created template folder with your prefered IDE (this is for VSCode) and modify the parameters you wish to change.
```bash
$ code .
```

Once you have finished the different parameters, upload the template with the following call:
```bash
-curl -X post -H "Bearer YOUR_TOKEN_HERE" https://api.creatio.ai/api/templates 
```

Get the new template:
```bash
curl -X 'GET' \
  'https://api.creatio.ai/api/templates/YOUR_TEMPLATE_ID_HERE' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer YOUR_TOKEN_HERE'
```

Create video using the new template:
```bash
curl -X 'POST' \
  'https://api.creatio.ai/api/videos' \
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

Pick the id form POST response, needed to download the video:
```bash
{
  "success": [
    {
      "externalId": "YOUR_VIDEO_EXTERNAL_ID",
      "id": "YOUR_VIDEO_ID"
    }
  ],
  "failure": []
}
```

Download video (return 404 if not processed yet):
```bash
curl -X 'GET' \
  'https://api.creatio.ai/api/videos/YOUR_VIDEO_ID_HERE/download' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer YOUR_TOKEN_HERE'
```

> If you're an advanced user, you can check our Swagger at the following URL: https://alpha-api.creatio.ai/swagger/index.html#/
