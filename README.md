# How to start with Creatio

## PRE-Requisites
#### These are the following pre-requisites to perform this tutorial:
- [Node](https://nodejs.org/en/) (v6 or higher)
- Npm 3 or higher (which comes bundled with Node)
- Git 
- [Visual Studio Code](https://code.visualstudio.com/) (or similar IDE)
- Download [Curl](https://curl.se/windows/) for Windows (Linux and iOS is already installed), include the bin folder in path.
- Ask for a token at the following email: support.creatio@creatio.ai

## How to create a custom template with Creatio

Open a terminal on your computer (cmd, bash, terminal...)

Clone the template example base to create your own template:
```bash
$ git clone https://github.com/CreatioAI/creatio-templates.git
```

Open the created template folder with your prefered IDE (this is for VSCode) and modify the parameters you wish to change.
```bash
$ cd creatio-templates
$ code .
```

Open file "template_testing.json" and complete all metadata:
```bash
{
    "name": "", //YOUR_TEMPLATE_NAME_HERE
    "description": "", //YOUR_TEMPLATE_DESCRIPTION_HERE
    "id": "", //YOUR_TEMPLATE_ID_HERE
    "previewGIF": "", //YOUR_GIF_NAME_HERE
    "visibility": "Private", //Private_OR_Public
    "owner": "", //YOUR_OWNER_NAME_HERE
    "tags": "", //YOUR_TAGS_HERE - tags 1, tags 2
    "scenes": //LIST_YOUR_SCENES_HERE 
    [
        //YOUR_SCENES_NAME_HERE
    ]
}
```

Edit the existing scenes (scene1 & scene2) or add new scenes inside the Scenes folder.
To see the configuration of scenes and their widgets, please go to the following [link](https://github.com/CreatioAI/creatio-api/tree/main/templates/widgets)

Each scene has some variables to refer dynamics fields for the widgets. 

```bash
...
    "parameters": [
        {
            "id": "variable_name1", //YOUR_VARIABLE_NAME_HERE
            "description":"Variable Name" //YOUR_VARIABLE_DESCRIPTION_HERE
        },
        {
            "id": "variable_name2",
            "description":"Variable Name"
        }
    ],
...
"widgets": [
        {
          ...
            "value": "{variable_name1}", //YOUR_WIDGET_DEFAULT_VALUE_HERE
          ...
        },
...
```

The Assets folder is for PreviewGif of template and Video parameter of each scene.
The video assets could be in mp4 format.

Once you have finished the different parameters, generate the package.zip:
```bash
$ npm run package
```

Go to the package.zip path:
```bash
$ cd packages/package
```

Upload the template with the following call: 
```bash 
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" -H "accept: */*" -H "Content-Type: multipart/form-data" -F "files=@package.zip;type=application/x-zip-compressed" -X "POST" https://api.creatio.ai/api/templates
```

Get the new template:
```bash
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" -H "accept: application/json" -X "GET" https://api.creatio.ai/api/templates/YOUR_TEMPLATE_ID_HERE
```

Create data.json file with the information of your request. New file on IDE and paste the following information:
```bash
[
    {
      "properties": {
      },
      "templateId": "YOUR_TEMPLATE_ID_HERE",
      "variables": {
          "YOUR_VARIABLE_NAME_HERE": "YOUR_VARIABLE_VALUE_HERE",
          "YOUR_VARIABLE_NAME_HERE": "YOUR_VARIABLE_VALUE_HERE"
        },
      "webhook": {
        "url": "",
        "token": ""
      },
      "preferedDate": "",
      "externalId": "YOUR_VIDEO_NAME_HERE"
    }
]
```


Create video using the new template:
```bash
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" -H "Accept: application/json" -H "Content-Type: application/json" -d "@data.json" -X "POST" https://api.creatio.ai/api/videos
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

Download video (return url null if not processed yet):
```bash
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" -H "Accept: application/json" -X "GET" https://api.creatio.ai/api/videos/YOUR_VIDEO_ID/download
```

> If you're an advanced user, you can check our Swagger at the following URL: https://alpha-api.creatio.ai/swagger/index.html#/
