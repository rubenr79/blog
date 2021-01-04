---
author: Ruben Ramos
title: "Exponiendo Tu Localhost a Internet Con Ngrok"
description: "Exponiendo Tu Localhost a Internet Con Ngrok"
date: 2019-02-07T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: false
---

ngrok permite exponer a Internet un servidor web que se ejecuta en su equipo local. Para ello establece túneles seguros desde un endpoint público, como Internet, hasta un servicio de red que funciona localmente.

En este post no pretendo hacer un análisis intensivo de la herramienta y todas sus utilidades, que son muchas. Si quieres investigar más sobre ngrok puedes ver su documentación que está bastante bien explicado.

## ¿Por qué lo necesito cuando desarrollo para Teams?
Como os he dicho en el post de Introducción al desarrollo para Microsoft Teams una app de Teams es una aplicación web y por supuesto esta aplicación web debe estar alojada en algún sitio. Cuando estamos desarrollando la app, muchas veces queremos probarla sin necesidad de desplegarla en ningún hosting, pues en estos casos es cuando podemos utilizar ngrok.

Lo que vamos a hacer es que nuestra aplicación esté temporalmente publicada a internet, con lo que podremos desplegar nuestro paquete en Teams y utilizar nuestra aplicación que tenemos alojada en nuestro servidor.

# Instalación
ngrok no requiere instalación, es un ejecutable único. Lo único que debemos hacer es descargarlo desde https://ngrok.com/download. ngrok es multiplataforma, así que al descargar tendrás que seleccionar la descarga adecuada a tu sistema operativo. Una vez realizada la descarga descomprimiremos el fichero ngrok.exe en una carpeta de nuestro ordenador.

![Descarga ngrok](/images/exponiendo-tu-localhost-a-internet-con-ngrok-01.png)

Con esto ya bastaría para poder utilizarlo, pero os aconsejo que añadáis la ruta donde se encuentra el ejecutable a vuestro path, para ello podéis añadirlo manualmente desde las variables de entorno, o bien abriendo una ventana de comandos como administrador y ejecutando el siguiente comando sustituyendo <path> por la ruta donde se encuentra el ejecutable

```powershell
setx path «%path%;<path>»
```

¿Como usar ngrok?
Obviamente, lo primero que tenemos que tener en cuenta es que nuestro servidor web esté levantado. En este ejemplo vamos a suponer que tenemos una aplicación en Node.js publicada a través del puerto 3007. En una ventana de comandos ejecutamos

```powershell
ngrok http 3007
```

Al ejecutarlo levantará un túnel hasta nuestro localhost. Cuando lo ejecutemos mostrará una ventana como esta

![Ejecución ngrok](/images/exponiendo-tu-localhost-a-internet-con-ngrok-02.png)

En esta ventana nos mostrará información, lo más importante es la url por donde el servicio escucha y la url a la que redirecciona. En nuestro ejemplo sería  https://d1e17d2e.ngrok.io -> localhost:3007

## ¿Puede ver que llamadas se están haciendo?
Si, en la propia ventana de comandos se muestran una información básica de las llamadas que se están haciendo.

![Información de peticiones en ngrok](/images/exponiendo-tu-localhost-a-internet-con-ngrok-03.png)

La verdad es que información muy básica, sólo petición y resultado, pero si abrimos el navegador y accedemos a http://127.0.0.1:4040 podemos ver una información más completa de cada petición. Esta url puede cambiar si tenemos varias ejecuciones de ngrok simultanea. Cuando ejecutamos ngrok muestra esta url como Web interface

![Información avanzada de peticiones en ngrok](/images/exponiendo-tu-localhost-a-internet-con-ngrok-04.png)

Referencias
https://ngrok.com