---
author: Ruben Ramos
title: "Introducción al desarrollo para Microsoft Teams"
description: "Introducción al desarrollo para Microsoft Teams"
date: 2019-01-07T08:45:48+01:00
featured_image: "/images/logos.png"
categories:
- Personal
tags: [Personal]
---

Para extender Teams debemos crear una app de Teams. Una sola app puede proporcionar una o más funcionalidades dependiendo de nuestras necesidades.

Las apps de Microsoft Teams son aplicaciones web, puedes usar cualquier tecnología de programación web y puedes alojarlas en cualquier plataforma de alojamiento.

Una app de Teams puede contener las siguientes funcionalidades:

- Tabs
- Bots
- Connectors
- Messaging extensions
- Activity feed integrations
- Outgoing web hooks

##¿Qué plan de Office 365 necesito para desarrollar para Teams?
Para desarrollar para Teams necesitas tener uno de estos planes:

- Business Essentials
- Business Premium
- Enterprise E1, E3, and E5
- Developer
- Education, Education Plus, and Education E5

Mi consejo es que te inscribas en el Office 365 Developer program para obtener un tenant de desarrollo por 1 año.

## Preparando el entorno
Teams está activado por defecto en las suscripciones que lo permiten, pero puedes comprobar que esta activo desde el centro de administración de Office 365. Para ello, ve a Configuración –> Servicios y complementos y selecciona Microsoft Teams. Dentro del panel que se abre comprueba que tienes activas las siguientes casillas:

![Configuración para permitir aplicaciones personalizadas](/images/introduccion-al-desarrollo-para-microsoft-teams-01.png)

Si no las tienes activas, actívalas y guarda los cambios.

Con esto ya tendríamos preparado nuestro tenant y podemos empezar a desarrollar.