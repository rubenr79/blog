---
author: Ruben Ramos
title: "Como Crear Un Boton Para Compartir Con Teams Sin Javascript"
description: "Como Crear Un Boton Para Compartir Con Teams Sin Javascript"
date: 2019-07-05T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
---

En el post anterior vimos como podemos crear un botón para compartir una url con Teams, para hacer esto se cargaba un fichero js que era el encargado de generar el enlace para compartir con Teams. Esta opción es la forma oficial de hacerlo y en principio la más recomendable, pero en ocasiones no podemos cargar ficheros js en la página donde queremos cargar el enlace.

Si te fijaste, cuando creamos el botón para compartir con Teams, lo que se hace es abrir una ventana con una url con el siguiente formato:

```
https://teams.microsoft.com/share?href=<link-to-share>&preview=true&referrer=
```

Por ejemplo, si queremos compartir este post con Teams lo que haremos es crear un enlace a la siguiente url

```
https://teams.microsoft.com/share?href=https://rubenrm.com/como-crear-un-boton-para-compartir-con-teams-sin-javascript&preview=true&referrer=rubenrm.com
```

Así que si quieres compartir este post con alguien de tu equipo puedes hacerlo pulsando aqui