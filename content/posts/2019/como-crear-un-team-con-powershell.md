---
author: Ruben Ramos
title: "Como Crear Un Team Con Powershell"
description: "Como Crear Un Team Con Powershell"
date: 2019-06-02T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: false
---

Hay varias formas de crear Teams, en este post vamos a ver como crear un Team desde Powershell. Para ello vamos a utilizar el módulo Powershell para Teams, si no lo tienes instalado puedes hacerlo ejecutando

```powershell
Install-Module -Name MicrosoftTeams
```

Una vez tengamos el módulo instalado ejecutamos el siguiente script para crear un nuevo Team con dos canales

```powershell
$teamName = "PowershellTeam"
$teamDescription = "My first team from Powershell"
$visibility = "Public" #Public or Private
Connect-MicrosoftTeams
$team = New-Team -DisplayName "$teamName" -Description "$teamDescription" -Visibility $visibility
New-TeamChannel -GroupId $team.GroupId -DisplayName "Channel 1"
New-TeamChannel -GroupId $team.GroupId -DisplayName "Channel 2"
```

El resultado:

![Resultado ejecución script](/images/como-crear-un-team-con-powershell-01.png)

Si quieres saber más sobre los comandos disponibles en el módulo de Teams puedes verlo en la [documentación oficial](https://docs.microsoft.com/es-es/powershell/module/teams/?view=teams-ps).