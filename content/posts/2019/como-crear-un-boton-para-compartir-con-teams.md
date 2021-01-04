---
author: Ruben Ramos
title: "Como Crear Un Boton Para Compartir Con Teams"
description: "Como Crear Un Boton Para Compartir Con Teams"
date: 2019-07-03T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: false
---

Recientemente Microsoft ha publicado la nueva funcionalidad Share to Teams, con la que fácilmente podemos añadir un enlace a nuestra página web para compartir una url con nuestro Teams.

Añadiendo el botón para compartir con Teams
Para poder hacer esto, lo primero que tenemos que hacer es incluir un fichero javascript que nos ofrece Teams

```javascript
<script async defer src="https://teams.microsoft.com/share/launcher.js" ></script>
```

Una vez que añadimos lo único que tenemos que hacer es incluir este código html en lugar que queramos de nuestra página incluyendo dentro del atributo data-href la url que queramos compartir

```html
<div
  class="teams-share-button"
  data-href="https://<link-to-be-shared>" >
</div>
```
Ésto hara que se muestre un icono de Teams con el enlace a compartir la url que hayamos indicado

![logo Microsoft Teams](/images/como-crear-un-boton-para-compartir-con-teams-01.png)


Podemos cambiar el tamaño del icono de la siguiente manera

```html
<div
  class="teams-share-button"
  data-href="https://<link-to-be-shared>" 
  data-icon-px-size="64">
</div>
```

Experiencia Share to Teams
Al pulsar sobre el icono nos abrirá una ventana emergente donde podemos seleccionar el canal donde queremos compartir el enlace

![Pantalla compartir en Microsoft Teams](/images/como-crear-un-boton-para-compartir-con-teams-02.png)

Una vez seleccionado un canal y al pulsar en el botón compartir veremos nuestro enlace publicado en el canal que hayamos seleccionado