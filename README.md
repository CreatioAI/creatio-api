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

To generate the package.zip:
```bash
$ npm run package
```

Once you have finished the different parameters, go to the package.zip path:
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
