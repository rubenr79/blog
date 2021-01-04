---
author: Ruben Ramos
title: "Como Crear Un Team Con Microsoft Graph"
description: "Como Crear Un Team Con Microsoft Graph"
date: 2019-06-02T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: false
---

En el post anterior vimos como podemos crear un Team con Powershell, en este vamos a ver como podemos hacer lo mismo utilizando Microsoft Graph. Por simplicidad, para hacer las llamadas a la API de Graph utilizaremos Graph Explorer.

Lo primero que tenemos que comprobar son los permisos para poder crear Teams, debemos asegurarnos de tener activo el permiso Group.ReadWrite.All

Un Team está asociado a un grupo de Office 365, por lo que lo primero que vamos a hacer es realizar la siguiente llamada para crear el grupo

```json
POST https://graph.microsoft.com/v1.0/groups/
{
  "description": "My first team from Graph",
  "displayName": "Graph Team",
  "groupTypes": [
    "Unified"
  ],
  "mailEnabled": true,
  "mailNickname": "graphTeam",
  "securityEnabled": false
}
```

Esta llamada creará un grupo de Office 365 y nos devolverá la información del grupo que ha creado. De la respuesta vamos a necesitar el ID, lo copiamos y realizamos la siguiente llamada

```json
PUT https://graph.microsoft.com/v1.0/groups/{id}/team
{
  "memberSettings": {
    "allowCreateUpdateChannels": true
  },
  "messagingSettings": {
    "allowUserEditMessages": true,
    "allowUserDeleteMessages": true
  },
  "funSettings": {
    "allowGiphy": true,
    "giphyContentRating": "strict"
  }
}
```

Con esto ya tendríamos creado nuestro Team, ahora para añadir un canal debemos hacer la siguiente llamada

```json
POST https://graph.microsoft.com/v1.0/teams/{id}/channels
{
  "displayName": "Channel 1",
  "description": "this channel was created with Graph"
}
```

Si vamos a la aplicación de Teams podemos ver el resultado

![Resultado en Microsoft Teams](/images/como-crear-un-team-con-microsoft-graph-01.png)