---
author: Ruben Ramos
title: "Un Vistazo Al Teams App Package"
description: "Un Vistazo Al Teams App Package"
date: 2019-01-09T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: false
---

Un vistazo al Teams app package
Publicado en 9 enero, 2019 por Ruben Ramos
Un paquete de una aplicación de Teams es un archivo.zip que contiene lo siguiente:

- Un archivo de manifiesto llamado "manifest.json", que especifica los atributos de su aplicación.
- Dos iconos en formato .png que se utilizarán en distintos sitios dentro de Teams.

## Iconos
Cuando creamos un paquete de una aplicación de Teams debemos incluir 2 iconos:

- Color: Es un icono con el fondo en color que debe tener un tamaño de 192×192 pixels.
- Outline: Es un icono con el fondo transparente que debe tener un tamaño de 32×32 pixels
En la siguiente imagen vemos cómo se vería el icono en los distintos sitios que se utilizan

![Icon showcase](/images/un-vistazo-al-teams-app-package-01.png)

## manifest.json
Este fichero es en el que vamos a incluir todos los datos de nuestra aplicación y los recursos y funcionalidades que vamos a utilizar, como por ejemplo, la url de una pestaña o los datos de un bot.

Este fichero contiene varias secciones:

- Datos del paquete de la aplicación: En esta sección se incluyen datos como la versión del paquete (actualmente la versión 1.3), nombre del paquete y datos del desarrollador.

```json

"$schema": "https://developer.microsoft.com/en-us/json-schemas/teams/v1.3/MicrosoftTeams.schema.json",

"manifestVersion": "1.3",

"version": "1.0.0",

"id": "%MICROSOFT-APP-ID%",

"packageName": "com.example.myapp",

"developer": {

"name": "Publisher Name",

"websiteUrl": "https://website.com/",

"privacyUrl": "https://website.com/privacy",

"termsOfUseUrl": "https://website.com/app-tos"

},

```

- Datos de la aplicación: En esta sección incluimos el nombre y descripción de la aplicación, así como la ruta a los iconos color y outline que hemos incluido en el paquete

```json
"name": {

"short": "Name of your app (<=30 chars)",

"full": "Full name of app, if longer than 30 characters"

},

"description": {

"short": "Short description of your app",

"full": "Full description of your app"

},

"icons": {

"outline": "%FILENAME-32x32px%",

"color": "%FILENAME-192x192px"

},

"accentColor": "%HEX-COLOR%",
```

- Componentes del paquete: en esta sección se incluye todos los componentes que vamos a incluir en nuestro paquete

```json
"configurableTabs": [ …],

"staticTabs":  [ …],

"bots":  [ …],

"connectors": [ …],

"composeExtensions": [ …],
```

- Seguridad: en esta sección incluiremos los permisos de nuestra app y los dominios desde donde permitiremos que nuestros contenidos sean cargados

```json
"permissions": [

"identity",

"messageTeamMembers"

],

"validDomains": [

"contoso.com"

]
```

Referencias
https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/apps/apps-package

https://docs.microsoft.com/en-us/microsoftteams/platform/resources/schema/manifest-schema