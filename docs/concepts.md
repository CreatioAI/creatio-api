# Conceptos básicos de Creatio
Creatio es una API que permite generar vídeos partiendo de plantillas preconstruidas (o personalizadas) simplemente rellenando valores para unas variables específicas a cada plantillas.

De modo que si, por ejemplo, se está quiere una plantilla para felicitar el cumpleaños a alguien, con informar de variables de la plantilla (como puede ser el: nombre de la persona, edad que cumple) se puede llegar a generar un vídeo dinámico y personalizado para felicitar a esta persona en particular.

## Elementos de Creatio

### Widget
Es el elemento básico que permite dotar de personalización a los vídeos. Un widget es un elemento visual personalizable. Todos widget disponen de las siguientes características:
* Posición y tamaño (expresado en coordenadas superior izquierda (en px), y píxeles de anchura y altura).
* Tipo: Puede ser "text", "image", "video", entre otros.
* Características: Son características propias a cada tipo de widget. Por ejemplo en el caso de text, podría ser la fuente o color del texto.
* Transición: Información acerca de la transición de entrada y salida del widget, como por ejemplo:
    - Segundo de entrada
    - Duración
    - Duración de efecto de fundido de entrada y salida
    - Dirección y duración de efecto de movimiento de entrada y salida
* Identificador único de Widget.
* Nombre de variable asociada al Widget
* Valor (por defecto) de la variable asociada al Widget.
* Propiedades: Diccionario clave-valor que almacena información particular arbitraria.

Existen varios tipos actualmente:
* Text: Widget de tipo texto. Se usa para renderizar texto personalizado en la escena. Se puede personalizar la fuente (entre las disponibles), color (expresado en hex) y alineación del texto.
* Image: Widget de tipo imagen. Se usa para renderizar una imágen en la escena. La única forma de indicarla es mediante una URL públicamente accesible que lleva directamente a la imagen. Se puede configurar para que se respete la relación de aspecto original de la imagen (por defecto), o si no hay problemas en distorsionar su resolución para adaptarla al espacio disponible en el Widget.
* Video: Widget de tipo vídeo. Se usa para renderizar un vídeo o un GIF en la escena. La única forma de indicarla es mediante una URL públicamente accesible que lleve directamente al vídeo o GIF. Se puede configurar para que se respete la relación de aspecto original del vídeo/GIF (por defecto), o si no hay problemas en distorsionar la relación de aspecto original para adaptarla al espacio máximo disponible en el Widget.


Un ejemplo de especificación de widget sería:
```json
{
    "id": "widget-infodia",
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
    }
```

### Escena (Scene)
Una escena es el bloque básico de construcción de una plantilla. Una plantilla se compone de 1 o más escenas. Una escena consta principalmente de:
* Identificador único de escena, que se usa para referenciarla en una plantilla.
* Vídeo: Enlace URL públicamente accesible para el vídeo que se mostrará de fondo en la escena. Por defecto la duración de este vídeo va a ser la duración de la escena.
* Lista de Widgets que componen la personalización de la escena.
* Propiedades: Diccionario clave-valor que almacena información particular arbitraria.

Un ejemplo de la especificación de una escena podría ser:
```json
{
    "id": "raona-birthday",
    "video": "https://stvideodemodev.blob.core.windows.net/video-assets/happybirthday.mp4",
    "widgets": [
        {
            "id": "hola",
            "type": "text",
            "description": "Saludo",
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

### Plantilla (Template)
Elemento a partir del cuál se pueden lanzar generaciones de vídeos en Creatio. Una plantilla no es más que un listado de escenas (ordenadas), con metadatos asociados.

Se compone de:
* Nombre: Nombre amigable para referenciar la plantilla en herramientas externas.
* Descripción: Descripción amigable para usar en herramientas externas.
* Identificador único de plantilla, que sirve para referenciarla en una generación de vídeo.
* GIF de previsualización: GIF a modo de previsualización de cómo es la plantilla (expresado mediante URL públicamente accesible).
* Escenas: Lista de escenas, expresada como una lista de identificadores de escena.


Un ejemplo de template podría ser:
```json
{
    "name": "Rendimiento empleada",
    "description": "Vídeo dirigido a felicitar a una empleada por su rendimiento el último mes.",
    "id": "raona-employee-performance-female",
    "previewGIF": "https://stvideodemodev.blob.core.windows.net/gifs/performancefemalegif.gif",
    "scenes": 
    [
        "raona-employee-performance-female"
    ]
}
```

### Petición
Una petición de generación de vídeo contiene toda la información necesaria para poder generar y compartir/usar el vídeo generado a posteriori.

Los elementos de una petición son:
* Identificador único de petición/vídeo
* Identificador único de la template usada
* Identificador externo de la petición vídeo: Sirve para vincular/almacenar referencias de sistemas externos a Creatio a la petición.
* Fecha de preferencia: Sirve para programar la generación, en lugar de ser una generación bajo demanda.
* Configuración: Almacena la información necesaria para generar los elementos visuales y gráficos de los vídeos, y reproducir la petición de vídeo.
* Propiedades: Diccionario clave-valor que almacena información particular arbitraria vinculada a la petición. Su principal uso es para enviar información que va a ser relevante recuperar por servicios externos en las llamadas Webhook/callback.
* Status: Estado actual de la petición. Existen las siguientes opciones: `IN_QUEUE`,  `PROCESSING`, `SUCCESSFUL`, `FAILED`, `DELETED` y `SCHEDULED`.
* Webhooks: Información para poder enviar un evento al terminar la generación de un vídeo. Cuando el servicio de generación y renderización de vídeos termina con un vídeo, hará una llamada `HTTP POST` a la dirección indicada, con información relevante del vídeo. Consta de dos piezas de información:
    - url: URL a la que hay que hacer la petición POST.
    - token: Token de acceso a usar el servicio (si es necesario). Debe ser tipo `Bearer`.
* Fecha y hora de creación de la petición
* Fecha y hora de producción de la petición (cuando se ha renderizado el vídeo).
* Razón/Motivo de cualquier posible error.