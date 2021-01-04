---
author: Ruben Ramos
title: "Creando Tabs Para Teams Con Spfx"
description: "Creando Tabs Para Teams Con Spfx"
date: 2019-05-16T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: true
---
Desde la versión 1.8 de SharePoint Framework se pueden implementar pestañas para Teams con SPFx. Con esto podemos conseguir 2 cosas:

- Vamos a poder reutilizar nuestros desarrollos SPFx para utilizar tanto en SharePoint Online como en Teams
- Los desarrollos que hagamos con SharePoint Framework se hospedan automáticamente en SharePoint y no necesitaremos de servicios externos para hospedar nuestra aplicación.

## Creando nuestro primer proyecto
El proceso de creación del proyecto de nuestra primera pestaña con SPFx es igual que si hiciésemos un webpart SPFx para SharePoint Online.

Dentro del directorio de nuestra solución, ejecutamos el generador de Yeoman para Sharepoint.

```powershell
yo @microsoft/sharepoint
```

Cuando configuramos el proyecto hay algunas cosas que tenemos que tener en cuenta:

1. Debemos seleccionar que nuestra solución la queremos implementar en “Solo SharePoint Online (más reciente)”
1. Cuando nos pregunte si queremos desplegar automáticamente en todo el tenant le diremos que si, de esta forma no será necesario instalar la aplicación en la colección de sitios antes de utilizarla.
1. Indicaremos que queremos crear un WebPart

![Opciones yo teams](/images/creando-tabs-para-teams-con-spfx-01.png)

## ¿Que nos encontramos cuando se crea el proyecto?
Una vez terminado el proceso de creación de la solución, veremos que se nos ha creado una estructura de carpetas muy parecida a la que tenemos cuando creamos cualquier webpart SPFx.

![Estructura de carpetas](/images/creando-tabs-para-teams-con-spfx-02.png)

Si nos fijamos, verenemos que dentro del directorio de la solución se ha creado una carpeta Teams que incluye 2 imagenes con iconos, estos iconos son los que se incluirán del paquete de la aplicación y son los mismos de los que hablamos en el post Un vistazo al Teams app package

## Actualiza el manifiesto del WebPart para que esté disponible para Microsoft Teams
Cuando creamos el webpart, únicamente estará disponible como un webpart de SharePoint Online. Para que nuestra solución esté disponible en Teams tenemos que modificar el manifiesto del webpart que se encuentra dentro de la carpeta con el código de nuestro webpart. Dentro del manifiesto debemos modificar el nodo supportedHosts para indicar que también queremos utilizar nuestro webpart como una pestaña de Teams.

```json
"supportedHosts": ["SharePointWebPart", "TeamsTab"],
```

## ¿Estoy en SharePoint o en Teams?
En ocasiones tendremos que identificar si nuestro webpart SPFx se está ejecutando dentro de Teams o en SharePoint. Para poder hacer esto vamos a modificar nuestro webpart para que obtenga el contexto de Teams.

1. Abrimos el fichero src\webparts\helloWorld\HelloWorldWebPart.ts e incluimos la instrucción

```javascript
import * as microsoftTeams from '@microsoft/teams-js';
```

1. Creamos una propiedad en el webpart que contenga el contexto de Teams

```javascript
private _teamsContext: microsoftTeams.Context;
```

3. Añadimos un metodo onInit en nuestro webpart para obtener el contexto

```javascript
protected onInit(): Promise<any> {
   let retVal: Promise<any> = Promise.resolve();
   if (this.context.microsoftTeams) {
      retVal = new Promise((resolve, reject) => {
         this.context.microsoftTeams.getContext(context => {
            this._teamsContext = context;
            resolve();
         });
      });
   }
   return retVal;
}
```

4. Como hemos creado nuestro webpart utilizando React, vamos a pasar el contexto como una propiedad del componente. Incluimos la propiedad dentro del fichero src\webparts\helloWorld\components\IHelloWorldProps.ts

```javascript
import * as microsoftTeams from '@microsoft/teams-js';
export interface IHelloWorldProps {
    description: string;
    teamsContext: microsoftTeams.Context
}
```
5. Dentro del método Render de nuestro webpart, le pasamos la propiedad

```javascript
public render(): void {
const element: React.ReactElement<IHelloWorldProps > = React.createElement(
HelloWorld,
{
description: this.properties.description,
teamsContext: this._teamsContext
}
);
ReactDom.render(element, this.domElement);
}
```

6. Ahora dentro de nuestro componente podemos utilizar el contexto que recibimos para identificar si estamos en Teams o no

```javascript
var title, message:string = '';
if (this.props.teamsContext) {
title = 'Welcome to Teams!';
message = 'Customize Teams experiences using Tabs.';
}
else {
title = 'Welcome to SharePoint!';
message = 'Customize SharePoint experiences using Web Parts.';
}
```

## Desplegando la aplicación en SharePoint
Para desplegar nuestra aplicación en SharePoint lo haremos exactamente igual que con cualquier webpart que hayamos creado con SPFx.

Generamos el paquete de la solución:

```powershell
gulp build
gulp bundle –ship
gulp package-solution --ship
```

Desplegamos el paquete en el catálogo de aplicaciones, marcando que la solución estará disponible en todo el tenant.

![Desplegando webparta en el catálogo de aplicaciones](/images/creando-tabs-para-teams-con-spfx-03.png)

Una vez desplegado la solución, insertamos el webpart en una página y vemos el resultado.

![Imagen del webpart](/images/creando-tabs-para-teams-con-spfx-04.png)

Como podemos ver, ha detectado que está en el contexto de SharePoint y nos muestra el mensaje que hemos añadido cuando se encuentra en SharePoint.

## Añade la pestaña en Teams
Ahora toca la parte más manual, pero por ahora. Microsoft está trabajando para que podamos sincronizar las aplicaciones que despleguemos en el catálogo de aplicaciones de SharePoint con Teams, de hecho, si vamos al catálogo de aplicaciones podemos ver el botón para hacerlo, pero por ahora está inactivo.

![Sincronizar aplicacion con Teams](/images/creando-tabs-para-teams-con-spfx-05.png)

Hasta que no esté activo nos tocará hacerlo “a mano”.

Lo primero que tenemos que hacer es crear un archivo con el manifiesto de la aplicación de Teams, para ello vete a la carpeta de teams y crea un fichero manifest.json. Dentro de ese fichero puedes utilizar esta plantilla:

```json

{
   "$schema": "https://developer.microsoft.com/en-us/json-schemas/teams/v1.2/MicrosoftTeams.schema.json",
   "manifestVersion": "1.2",
   "packageName": "{{SPFX_COMPONENT_ALIAS}}",
   "id": "aa3fecf0-1fd0-4751-aba1-12314dc3a22f",
   "version": "0.1",
   "developer": {
     "name": "Super Developer",
     "websiteUrl": "https://products.office.com/en-us/sharepoint/collaboration",
     "privacyUrl": "https://privacy.microsoft.com/en-us/privacystatement",
     "termsOfUseUrl": "https://www.microsoft.com/en-us/servicesagreement"
   },
   "name": {
     "short": "{{SPFX_COMPONENT_NAME}}"
   },
   "description": {
     "short": "{{SPFX_COMPONENT_SHORT_DESCRIPTION}}",
     "full": "{{SPFX_COMPONENT_LONG_DESCRIPTION}}"
   },
   "icons": {
     "outline": "{{SPFX_COMPONENT_ID}}_outline.png",
     "color": "{{SPFX_COMPONENT_ID}}_color.png"
   },
   "accentColor": "#004578",
   "configurableTabs": [
     {
       "configurationUrl": "https://{teamSiteDomain}{teamSitePath}/_layouts/15/TeamsLogon.aspx?SPFX=true&dest={teamSitePath}/_layouts/15/teamshostedapp.aspx%3FopenPropertyPane=true%26teams%26componentId={{SPFX_COMPONENT_ID}}%26forceLocale={locale}",
       "canUpdateConfiguration": true,
       "scopes": [
         "team"
       ]
     }
   ],
   "validDomains": [
     "*.login.microsoftonline.com",
     "*.sharepoint.com",
     "*.sharepoint-df.com",
     "spoppe-a.akamaihd.net",
     "spoprod-a.akamaihd.net",
     "resourceseng.blob.core.windows.net",
     "msft.spoppe.com"
   ],
   "webApplicationInfo": {
     "resource": "https://{teamSiteDomain}",
     "id": "00000003-0000-0ff1-ce00-000000000000"
   }
}

```
Ahora tienes que actualizar los siguientes valores en el manifiesto con los valores de tu webpart:

manifest.json	| webpart SPFx
{{SPFX_COMPONENT_ALIAS}}	| alias
{{SPFX_COMPONENT_NAME}}	| preconfiguredEntries[0].title
{{SPFX_COMPONENT_SHORT_DESCRIPTION}}	| preconfiguredEntries[0].description
{{SPFX_COMPONENT_LONG_DESCRIPTION}}	| preconfiguredEntries[0].description
{{SPFX_COMPONENT_ID}}	| id

Una vez creado el manifiesto tenemos que crear un zip con el contenido de la carpeta teams, es decir, el zip contendrá el fichero manifest.json que acabamos de crear y las dos imagenes con los iconos de nuestra app.

Para subir la app a Teams haremos igual que cuando creamos una pestaña con NodeJS, en el post anterior.

Dentro de las opciones del team vamos a Administrar equipo

![Administrar equipo en Teams](/images/creando-tabs-para-teams-con-spfx-06.png)

Dentro de la pestaña de Aplicaciones pulsamos el enlace Cargar una aplicación personalizada que se encuentra en la zona inferior derecha y seleccionamos el zip que hemos generado dentro de la carpeta package de nuestra solución

![Cargar aplicación personalizada](/images/creando-tabs-para-teams-con-spfx-07.png)

Con esto ya tendríamos disponible nuestra app, pero aún nos queda añadir la ficha en nuestro team, como si fuese cualquier otra ficha. Vamos al canal general –> Añadir ficha

![Añadir pestaña en Teams](/images/creando-tabs-para-teams-con-spfx-08.png)

Buscamos la ficha que hemos creado y la seleccionamos y al guardar veremos nuestro webpart SPFx en Teams y vemos que nos ha detectado el contexto de Teams

![Resultado del webpart en Teams](/images/creando-tabs-para-teams-con-spfx-09.png)