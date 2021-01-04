---
author: Ruben Ramos
title: "Como Crear Un Marcador en Chromium Para Compartir Una Url Con Teams"
description: "Como Crear Un Marcador en Chromium Para Compartir Una Url Con Teams"
date: 2019-07-06T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: false
---

Cada vez que veo un enlace interesante lo comparto en un canal de Teams con mi equipo. Lo que hacía es copiar la url, abrir la aplicación de Microsoft Teams y pegar la url en el canal que quería. Lo que vamos a ver en este post es como crear un marcador para compartir la url en la que estemos con un canal de Teams.

Lo que haremos es crear un marcador con el siguiente código:

```Javascript
javascript: (() => {
    javascript: (() => {
        const shareTeamsUrl = new URL('https://teams.microsoft.com/share');
        const url = new URL(window.location.href);
        shareTeamsUrl.searchParams.set("href", url);
        shareTeamsUrl.searchParams.set("preview", "true");
        shareTeamsUrl.searchParams.set("referrer", window.location.host);
        window.open(shareTeamsUrl.href, "ms-teams-share-popup", 'width=700,height=600');
        return false;
    })();
})();
```

Puedes hacerlo fácilmente arrastrando el siguiente enlace a la barra de marcadores: [Share to Teams](javascript: (() => {javascript: (() => {const shareTeamsUrl = new URL('https://teams.microsoft.com/share');const url new URL(window.location.href);shareTeamsUrl.searchParams.set("href", url);shareTeamsUrl.searchParams.set("preview", "true");shareTeamsUrl.searchParams.set("referrer", window.location.host);window.open(shareTeamsUrl.href, "ms-teams-share-popup", 'width=700,height=600');return false;})();})();)

![Pantalla compartir en Microsoft Teams](/images/como-crear-un-marcador-en-chromium-para-compartir-una-url-con-teams-01.gif)