# Entidades
## Templates
Las templates son la base de la generación de vídeos. Una plantilla además de tener elementos informativos propios (identificador, nombre, descripción, preview, etc), dispone de un listado de escenas que la componen. Estas escenas están comprendidas por elementos visuales llamados Widgets, que son cumplimentables mediante el uso de variables.

Cuando se quiere generar un vídeo se hace mediante el uso de las plantillas + variables.
### Endpoints
* `GET /api/templates`: Obtiene el listado de plantillas disponibles y lo devuelve en formato JSON. Devuelve 200 OK, o 401 FORBIDDEN si no se dispone de permisos.

```json
[
  {
    "id": "employee-birthday",
    "name": "Felicitación de cumpleaños",
    "description": "Vídeo ideado para felicitar el cumpleaños a un empleado.",
    "previewGIF": "https://test123.net/storage/videos/employee-birthday-preview.gif"
  },
  {
    "id": "employee-performance-female",
    "name": "Rendimiento empleada",
    "description": "Vídeo dirigido a felicitar a una empleada por su rendimiento el último mes.",
    "previewGIF": "https://test123.net/storage/videos/employee-performmance-female-preview.gif"
  },
  {
    "id": "employee-performance-male",
    "name": "Rendimiento empleado",
    "description": "Vídeo dirigido a felicitar a un empleado por su rendimiento el último mes.",
    "previewGIF": "https://test123.net/storage/videos/employee-performmance-male-preview.gif"
  }
]
```
* `GET /api/templates/<id>`: Devuelve el listado de variables disponibles para el uso de la plantilla con id igual a `<id>`. Devuelve 200 OK si se encuentra template con el ID, 401 FORBIDDEN si no se dispone de permisos, sino 404 NOT FOUND en caso de no existir la ID. Formato de retorno:
```json
[
    {
        "variableName": "employee-name",
        "description": "Nombre del empleado que cumple años",
        "type": "text",
        "properties": {}
    },
    {
        "variableName": "employee-age",
        "description": "Edad que cumple",
        "type": "text",
        "properties": {}
    }
]
```



## Videos
Los vídeos son la finalidad de la API. Como entidad, se puede interactuar con vídeos para generarlos bajo demanda, programar su generación, obtener enlaces para descargarlos, así como comprobar el listado de vídeos generados/pendientes/programados e interactuar con ellos.

### Endpoints
* `GET /api/videos`: Obtiene el listado de vídeos que se han enviado a generar en algún momento, con información sobre su estado, plantilla de la que proviene, etc. Devuelve 200 OK, o 401 FORBIDDEN si no se dispone de permisos. Formato de retorno:

```json
[
	{
		"id": "cc638780-b0ac-4c79-9227-64e4387b0ee0",
		"externalId": "349793c4-c72b-4cce-b80f-0a6be33096fe",
		"status": "SUCCESSFUL",
		"templateId": "employee-birthday-europastry",
		"preferedDate": "12/05/2022",
		"properties": {
			"employeeMail": "alfredo.garcia@europastry.com",
			"employeeId": "2828"
		}
	},
		{
		"id": "a4171dc9-a410-4b8c-bd79-c96b23e3a07a",
		"externalId": "65b9d188-2ab6-44fd-bacb-f24a76531578",
		"status": "SCHEDULED",
		"templateId": "employee-birthday-europastry",
		"preferedDate": "18/05/2022",
		"properties": {
			"employeeMail": "pepito.perez@europastry.com",
			"employeeId": "1291"
		}
	}
]
```

* `GET /api/videos/<id>`: Obtiene toda la información del vídeo con id `<id>`. Devuelve 200 OK si existe el vídeo, y 404 NOT FOUND en caso contrario, o 401 FORBIDDEN si no se dispone de permisos. Formato de retorno:
```json
{
    "id": "cc638780-b0ac-4c79-9227-64e4387b0ee0",
    "externalId": "349793c4-c72b-4cce-b80f-0a6be33096fe",
    "status": "SUCCESSFUL",
    "templateId": "employee-birthday-europastry",
    "preferedDate": "12/05/2022",
    "queued": "01/01/2022T00:00:01",
    "rendered": "12/05/2022T00:00:01",
    "reason": "OK",
    "webhook": {
        "url": "https://test.com/webhook",
        "token": "token123123123"
    },
    "variables": [
        {
            "variableName": "employeeName",
            "value": "Alfredo García",
            "properties": {}
        },
        {
            "variableName": "employeeAge",
            "value": "43",
            "properties": {}
        }
    ],
    "properties": {
        "employeeMail": "alfredo.garcia@europastry.com",
        "employeeId": "2828"
    }
}
```


* `POST /api/videos`: Lanza una petición de generación de vídeo automatizado. Devuelve 201 CREATED, o 401 FORBIDDEN si no se dispone de permisos. El cuerpo que es necesario enviarle es un JSON con el siguiente formato:
```json
[
	{
		"templateId": "employee-birthday-europastry",
		"variables": [
            {
                "id": "employeeName",
                "value": "Alfredo García",
                "properties": {}
            },
            {
                "id": "employeeAge",
                "value": "43",
                "properties": {}
            }
        ],
        "properties": {
			"employeeMail": "alfredo.garcia@europastry.com",
			"employeeId": "2828"
		},
        "webhook": {
	        "url": "https://test.com/webhook",
	        "token": "token123123123"
	    },
    	"preferedDate": "12/05/2022",
    	"externalId": "349793c4-c72b-4cce-b80f-0a6be33096fe"
	},
    {
		"templateId": "employee-birthday-europastry",
		"variables": [
            {
                "id": "employeeName",
                "value": "Pepito Pérez",
                "properties": {}
            },
            {
                "id": "employeeAge",
                "value": "38",
                "properties": {}
            }
        ],
        "properties": {
			"employeeMail": "pepito.perez@europastry.com",
			"employeeId": "1291"
		},
        "webhook": {
	        "url": "https://test.com/webhook",
	        "token": "token123123123"
	    },
    	"preferedDate": "18/05/2022",
    	"externalId": "65b9d188-2ab6-44fd-bacb-f24a76531578"
	}
]
```
El resultado de la llamada es un JSON con el siguiente formato:
```json
[
    {
        "externalId": "349793c4-c72b-4cce-b80f-0a6be33096fe",
        "id": "cc638780-b0ac-4c79-9227-64e4387b0ee0"
    },
    {
        "externalId": "65b9d188-2ab6-44fd-bacb-f24a76531578",
        "id": "a4171dc9-a410-4b8c-bd79-c96b23e3a07a"
    }
]
```

* `PUT /api/videos/<id>`: Modifica una petición de generación de vídeo. Devuelve 204 NO CONTENT si se ha realizado correctamente, 401 FORBIDDEN si no se dispone de permisos, o 404 NOT FOUND en caso de que no exista petición de video con la ID.
```json
[
	{
        "id": "cc638780-b0ac-4c79-9227-64e4387b0ee0",
		"templateId": "employee-birthday-europastry",
		"variables": [
            {
                "id": "employeeName",
                "value": "Alfredo García",
                "properties": {}
            },
            {
                "id": "employeeAge",
                "value": "44",
                "properties": {}
            }
        ],
        "properties": {
			"employeeMail": "alfredo.garcia@europastry.com",
			"employeeId": "2828"
		},
        "webhook": {
	        "url": "https://test.com/webhook",
	        "token": "token123123123"
	    },
    	"preferedDate": "12/05/2022",
    	"externalId": "349793c4-c72b-4cce-b80f-0a6be33096fe"
	}
]
```
El resultado de la llamada es un JSON con el siguiente formato:
```json
[
    {
        "externalId": "349793c4-c72b-4cce-b80f-0a6be33096fe",
        "id": "cc638780-b0ac-4c79-9227-64e4387b0ee0"
    },
    {
        "externalId": "65b9d188-2ab6-44fd-bacb-f24a76531578",
        "id": "a4171dc9-a410-4b8c-bd79-c96b23e3a07a"
    }
]
```

* `DELETE /api/videos/<id>`: Elimina un vídeo generado o una generación de vídeo. Devuelve un 204 NO CONTENT si se ha realizado correctamente, 401 FORBIDDEN si no se dispone de permisos, o un 404 NOT FOUND si no existe petición o vídeo.


* `GET /api/videos/<id>/download`: Genera enlace para compartir vídeo previamente generado. Devuelve 200 OK en caso de que exista el vídeo y se tengan permisos para acceder, 401 FORBIDDEN si no se dispone de permisos, y 404 si no existe el vídeo con ese ID.



# Formato plantillas y escenas
## Campos
### Template
* name (string): Nombre amigable de la plantilla.
* description (string): Descripción amigable de la plantilla.
* previewGIF (string): Url que lleva al GIF de preview de la plantilla.
* id (string): Identificador único de la plantilla.
* properties (dictionary<string,string>): Propiedades genéricas de la plantilla.
* video (objeto):
    *  ~~use_audio (bool): Flag que marca si se debe usar audio en el vídeo.~~
    * ~~output_format (string): Formato de generación de vídeo, por defecto "mp4"~~
    * scenes (Lista[str]): Lista de SceneIds

### Scene
* id (string): identificador único de la escena.
* video (string): Url que lleva al vídeo que se usa de fondo en la escena.
* widgets (Lista[Widget]): Listado de Widgets gráficos de la escena

### Widget
* id (string): Identificador único del widget
* variableName (string): Nombre de la variable que alimenta el Widget.
* description (string): Descripción del widget.
* type (string): Tipo de Widget (text, image, video).
* value (string): Valor del widget.
* properties (dictionary<string,string>): Propiedades genéricas del widget.
* features (Object): Características del Widget. Dependen del tipo.
* transition (Object): Características de transición del widget.
* Position (Object): Coordenadas y tamaño del Widget.







# Futuras expansiones 

## Templates

### Endpoints
* `GET /api/templates/<id>/definition`: Devuelve el JSON completo de la definición de la plantilla:
```json
{
    "name": "Felicitación de cumpleaños",
    "description": "Vídeo ideado para felicitar el cumpleaños a un empleado.",
    "id": "sample-definition",
    "previewGIF": "https://stvideodemodev.blob.core.windows.net/video-assets/employee-birthday-preview.gif",
    "scenes": [
        "sample-definition"
    ]
}
```

* `POST /api/templates/<id>`: Da de alta una nueva plantilla personalizada en el sistema. Cuerpo a Enviar:
```json
{
    "name": "Felicitación de cumpleaños",
    "description": "Vídeo ideado para felicitar el cumpleaños a un empleado.",
    "id": "sample-definition",
    "previewGIF": "https://stvideodemodev.blob.core.windows.net/video-assets/employee-birthday-preview.gif",
    "scenes": [
        "sample-definition"
    ]
}
```

* `PUT /api/templates/<id>`: Modifica una plantilla personalizada del sistema. Devuelve 204 NO CONTENT si la puede modificar, 404 NOT FOUND si no existe, 400 BAD REQUEST si tiene un formato incorrecto. Cuerpo a enviar:
```json
{
    "name": "Felicitación de cumpleaños",
    "description": "Vídeo ideado para felicitar el cumpleaños a un empleado.",
    "id": "sample-definition",
    "previewGIF": "https://stvideodemodev.blob.core.windows.net/video-assets/employee-birthday-preview.gif",
    "scenes": [
        "sample-definition"
    ]
}
```

* `DELETE /api/templates/<id>`: Da de baja una plantilla personalizada del sistema. Devuelve 204 NO CONTENT si existe, 404 NOT FOUND si no.


## Scenes
Las escenas son los conjuntos audiovisuales de los que se componen las plantillas, y contienen información gráfica de los elementos que van a aparecer, así como sus variables para su correcta generación automatizada.

### Endpoints
* `GET /api/scenes/<id>/definition`: Devuelve el JSON completo de la definición de la escena:
```json
{
    "id": "sample-definition",
    "video": "https://stvideodemodev.blob.core.windows.net/video-assets/happybirthday.mp4",
    "widgets": [
        {
            "id": "hola",
            "type": "text",
            "description": "Salutación",
            "variableName": "employeeName",
            "value": "Hola Empleado!",
            "features": {
                "font": "Titan-One",
                "color": "#EAF2FC",
                "align": "center"
            },
            "transition": {
                "delay": 1.2,
                "duration": 2.83,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 470,
                "y": 221.23,
                "w": 320,
                "h": 285.71
            },
            "properties": {}
        },
        {
            "id": "infodia",
            "description": "Acontecimiento del día",
            "value": "Hoy es el día mundial de la pizza",
            "type": "text",
            "variableName": "eventOfTheDay",
            "features": {
                "font": "Titan-One",
                "color": "#252729",
                "align": "center"
            },
            "transition": {
                "delay": 11.1,
                "duration": 4,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 421,
                "y": 218,
                "w": 453,
                "h": 280
            },
            "properties": {}
        },
        {
            "id": "feliz",
            "description": "Deseos",
            "value": "Deseamos que seas muy feliz en tu día",
            "type": "text",
            "features": {
                "font": "Titan-One",
                "color": "#EAF2FC",
                "align": "center"
            },
            "transition": {
                "delay": 31.4,
                "duration": 4.6,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 470,
                "y": 800,
                "w": 980,
                "h": 200
            },
            "properties": {}
        },
        {
            "id": "gracias",
            "description": "Agradecimientos",
            "value": "¡Gracias por regalarnos esa sonrisa cada día!",
            "type": "text",
            "features": {
                "font": "Titan-One",
                "color": "#252729",
                "align": "center"
            },
            "transition": {
                "delay": 36.08,
                "duration": 2.22,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 335,
                "y": 360,
                "w": 610,
                "h": 130
            },
            "properties": {}
        },
        {
            "id": "felicidades",
            "description": "Felicitación",
            "type": "text",
            "variableName": "congratulationMessage",
            "value": "Muchísimas felicidades, Empleado!",
            "features": {
                "font": "Titan-One",
                "color": "#EAF2FC",
                "align": "center"
            },
            "transition": {
                "delay": 41,
                "duration": 3,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 310,
                "y": 375,
                "w": 665,
                "h": 100
            },
            "properties": {}
        }
    ]
}
```

* `POST /api/scenes/<id>`: Da de alta una nueva escena personalizada en el sistema. Cuerpo a Enviar:
```json
{
    "id": "sample-definition",
    "video": "https://stvideodemodev.blob.core.windows.net/video-assets/happybirthday.mp4",
    "widgets": [
        {
            "id": "hola",
            "type": "text",
            "description": "Salutación",
            "variableName": "employeeName",
            "value": "Hola Empleado!",
            "features": {
                "font": "Titan-One",
                "color": "#EAF2FC",
                "align": "center"
            },
            "transition": {
                "delay": 1.2,
                "duration": 2.83,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 470,
                "y": 221.23,
                "w": 320,
                "h": 285.71
            },
            "properties": {}
        },
        {
            "id": "infodia",
            "description": "Acontecimiento del día",
            "value": "Hoy es el día mundial de la pizza",
            "type": "text",
            "variableName": "eventOfTheDay",
            "features": {
                "font": "Titan-One",
                "color": "#252729",
                "align": "center"
            },
            "transition": {
                "delay": 11.1,
                "duration": 4,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 421,
                "y": 218,
                "w": 453,
                "h": 280
            },
            "properties": {}
        },
        {
            "id": "feliz",
            "description": "Deseos",
            "value": "Deseamos que seas muy feliz en tu día",
            "type": "text",
            "features": {
                "font": "Titan-One",
                "color": "#EAF2FC",
                "align": "center"
            },
            "transition": {
                "delay": 31.4,
                "duration": 4.6,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 470,
                "y": 800,
                "w": 980,
                "h": 200
            },
            "properties": {}
        },
        {
            "id": "gracias",
            "description": "Agradecimientos",
            "value": "¡Gracias por regalarnos esa sonrisa cada día!",
            "type": "text",
            "features": {
                "font": "Titan-One",
                "color": "#252729",
                "align": "center"
            },
            "transition": {
                "delay": 36.08,
                "duration": 2.22,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 335,
                "y": 360,
                "w": 610,
                "h": 130
            },
            "properties": {}
        },
        {
            "id": "felicidades",
            "description": "Felicitación",
            "type": "text",
            "variableName": "congratulationMessage",
            "value": "Muchísimas felicidades, Empleado!",
            "features": {
                "font": "Titan-One",
                "color": "#EAF2FC",
                "align": "center"
            },
            "transition": {
                "delay": 41,
                "duration": 3,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 310,
                "y": 375,
                "w": 665,
                "h": 100
            },
            "properties": {}
        }
    ]
}
```

* `PUT /api/scenes/<id>`: Modifica una escena personalizada del sistema. Devuelve 204 NO CONTENT si la puede modificar, 404 NOT FOUND si no existe, 400 BAD REQUEST si tiene un formato incorrecto. Cuerpo a enviar:
```json
{
    "id": "sample-definition",
    "video": "https://stvideodemodev.blob.core.windows.net/video-assets/happybirthday.mp4",
    "widgets": [
        {
            "id": "hola",
            "type": "text",
            "description": "Salutación",
            "variableName": "employeeName",
            "value": "Hola Empleado!",
            "features": {
                "font": "Titan-One",
                "color": "#EAF2FC",
                "align": "center"
            },
            "transition": {
                "delay": 1.2,
                "duration": 2.83,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 470,
                "y": 221.23,
                "w": 320,
                "h": 285.71
            },
            "properties": {}
        },
        {
            "id": "infodia",
            "description": "Acontecimiento del día",
            "value": "Hoy es el día mundial de la pizza",
            "type": "text",
            "variableName": "eventOfTheDay",
            "features": {
                "font": "Titan-One",
                "color": "#252729",
                "align": "center"
            },
            "transition": {
                "delay": 11.1,
                "duration": 4,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 421,
                "y": 218,
                "w": 453,
                "h": 280
            },
            "properties": {}
        },
        {
            "id": "feliz",
            "description": "Deseos",
            "value": "Deseamos que seas muy feliz en tu día",
            "type": "text",
            "features": {
                "font": "Titan-One",
                "color": "#EAF2FC",
                "align": "center"
            },
            "transition": {
                "delay": 31.4,
                "duration": 4.6,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 470,
                "y": 800,
                "w": 980,
                "h": 200
            },
            "properties": {}
        },
        {
            "id": "gracias",
            "description": "Agradecimientos",
            "value": "¡Gracias por regalarnos esa sonrisa cada día!",
            "type": "text",
            "features": {
                "font": "Titan-One",
                "color": "#252729",
                "align": "center"
            },
            "transition": {
                "delay": 36.08,
                "duration": 2.22,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 335,
                "y": 360,
                "w": 610,
                "h": 130
            },
            "properties": {}
        },
        {
            "id": "felicidades",
            "description": "Felicitación",
            "type": "text",
            "variableName": "congratulationMessage",
            "value": "Muchísimas felicidades, Empleado!",
            "features": {
                "font": "Titan-One",
                "color": "#EAF2FC",
                "align": "center"
            },
            "transition": {
                "delay": 41,
                "duration": 3,
                "fadeIn": 0.1,
                "fadeOut": 0.1
            },
            "position": {
                "x": 310,
                "y": 375,
                "w": 665,
                "h": 100
            },
            "properties": {}
        }
    ]
}
```

* `DELETE /api/scenes/<id>`: Da de baja una escena personalizada del sistema. Devuelve 204 NO CONTENT si existe, 404 NOT FOUND si no.


## Otros 
### Endpoints

* `GET /api/me`: Devuelve información del usuario/dominio solicitante como:
    - Nombre de la corporación
    - Tier de suscripción

* `GET /api/analytics/...`: Devuelve información y métricas con distintos sub-endpoints:
    - Métricas de uso (por template, por tiempo, etc)
    - Capacidad/consumo
    - etc