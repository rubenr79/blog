---
author: Ruben Ramos
title: "Creando La Primera Tab Para Teams Con Node Js en Visual Studio Code"
description: "Creando La Primera Tab Para Teams Con Node Js en Visual Studio Code"
date: 2019-04-16T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: false
---

En este post vamos a ver como crear una tab para Teams con una aplicación Node.js desde Visual Studio Code. Para ello vamos a utilizar vamos a utilizar la plantilla de Yeoman para apps de Teams, si aún no la tienes instalado en su página de GitHub explican los pasos que tienes que seguir para instalarla, https://github.com/OfficeDev/generator-teams

Lo primero que haremos es crear una carpeta donde crear nuestra solución, abrimos una ventana de comandos y ejecutamos

```powershell
md teams-simpletab

cd teams-simpletab
```

Para ejecutar el generador de Yeoman, ejecutamos

```powershell
yo teams
```

El asistente nos irá pidiendo una serie de datos para nuestra app:

- El nombre de la solución
- Si queremos utilizar la carpeta en la que nos encontramos o crear una nueva
- Titulo de la app
- El nombre de la compañia
- Que es lo que quieres añadir en el proyecto, en este punto seleccionamos Tab.

![Opciones yo teams](/images/creando-la-primera-tab-para-teams-con-node-js-en-visual-studio-code-01.png)

- La url donde alojaremos nuestra aplicación
- El nombre de la ficha por defecto

Una vez hayamos cumplimentado todos estos datos, el generador comenzará a crear nuestra solución. Una vez terminado abrimos nuestra solución con nuestro editor favorito, por supuesto nosotros utilizaremos Visual Studio Code. Podemos abrirlo directamente ejecutando

```powershell
code .
```

Una vez abierto veremos que nos ha creado una estructura de carpeta como la de la imagen

![Estructura de carpetas](/images/creando-la-primera-tab-para-teams-con-node-js-en-visual-studio-code-02.png)

Dentro de esta estructura podemos ver la carpeta src\app que es donde se encuentra el código de nuestra applicación y la carpeta src\manifest que son los ficheros que se incluirán en el paquete de nuestra app de Teams.

Probando la aplicación
Ahora vamos a probar como quedaría nuestra aplicación dentro de Teams. Para probarla vamos a ejecutar nuestra aplicación en local y añadiremos el paquete en Teams.

Para que desde Teams podamos ver la aplicación que tenemos en local utilizaremos ngrok, en el post xxxxxxx puedes por qué lo utilizamos.

Para ello volvemos a la ventana de comandos y ejecutamos

```powershell
ngrok http 3007
```

3007 es el puerto donde se está ejecutando nuestro servidor web de Express, es el puerto que se pone por defecto cuando creamos la aplicación con el generador de Teams de Yeoman, pero podemos cambiarlo en el fichero src\app\server.ts en la siguiente línea

```powershell
const port = process.env.port || process.env.PORT || 3007;
```

Una vez lo ejecutamos se mostrará información en pantalla, por ahora necesitamos la url https que se muestra en Forwarding, tendrá el formato https://xxxxxxxxx.ngrok.io. La copiamos y dejamos el servicio ejecutándose.

Ahora tenemos que decir en nuestra app dónde se esta ejecutando la aplicación que vamos a mostrar en la ficha, para esto vamos a modificar el manifiesto de nuestra app para indicarlo.

Abrimos el fichero src\manifest\manifest.json y reemplazamos todas las urls de la aplicación que hayamos indicado por la url https que hemos copiado anteriormente. Tenemos que hacer este cambio en los siguientes nodos

- developer/websiteUrl
- developer/privacyUrl
- developer/termsOfUseUrl
- configurableTabs/configurationUrl

En mi ejemplo quedaría así:

```json
…

«developer»: {

«name»: «Ruben»,

«websiteUrl»: «https://1cb22031.ngrok.io»,

«privacyUrl»: «https://1cb22031.ngrok.io/privacy.html»,

«termsOfUseUrl»: https://1cb22031.ngrok.io/tou.html

},

…

«configurableTabs»: [

{

«configurationUrl»: «https://1cb22031.ngrok.io/teamsSimpleTabConfig.html»,

«canUpdateConfiguration»: true,

«scopes»: [

«team»

]

}

],
```

Una vez que hayamos modificado el fichero, lo guardamos.

Ahora tenemos que generar el paquete de la app, para lo que ejecutaremos

```powershell
gulp manifest
```

Esto creara un zip en la carpeta package con el contenido de la carpeta src\manifest. Este zip es el que tendremos que subir a Teams.

También tenemos que hacer un build de la solución y iniciar el servidor web Express ejecutando los siguientes comandos

```powershell
gulp build

gulp serve
```

Para subir la aplicación a Teams, abrimos Teams y vamos al equipo donde vamos a probar la app. También podemos crear un team nuevo para hacer las pruebas.

Dentro de las opciones del team vamos a Administrar equipo

![Administrar equipo en Teams](/images/creando-la-primera-tab-para-teams-con-node-js-en-visual-studio-code-03.png)

Dentro de la pestaña de Aplicaciones pulsamos el enlace Cargar una aplicación personalizada que se encuentra en la zona inferior derecha y seleccionamos el zip que hemos generado dentro de la carpeta package de nuestra solución

![Cargar una aplicación personalizada en Teams](/images/creando-la-primera-tab-para-teams-con-node-js-en-visual-studio-code-04.png)

Con esto ya tendríamos disponible nuestra app, pero aún nos queda añadir la ficha en nuestro team, como si fuese cualquier otra ficha. Vamos al canal general –> Añadir ficha

![Añadir pestaña en Teams](/images/creando-la-primera-tab-para-teams-con-node-js-en-visual-studio-code-05.png)

Buscamos la ficha que hemos creado y la seleccionamos, si todo esta correcto nos mostrará la página de configuración que hayamos indicado en nuestro manifiesto

![Configuración de la pestaña en Teams](/images/creando-la-primera-tab-para-teams-con-node-js-en-visual-studio-code-06.png)

En este ejemplo nos solicita un valor que luego mostrará en la ficha

![Resultado](/images/creando-la-primera-tab-para-teams-con-node-js-en-visual-studio-code-07.png)

Las fichas personalizadas que creemos están disponibles tanto en web como en la aplicación de escritorio de Teams.