---
author: Ruben Ramos
title: "Como Trabajar Con Graph Api Desde Powershell"
description: "Como Trabajar Con Graph Api Desde Powershell"
date: 2019-07-10T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: true
---

Como hemos visto Graph API ofrece más posibilidades que el módulo de Teams para PowerShell a la hora de administrar Teams. Ahora bien, desde PowerShell también podemos hacer llamadas a Graph API y en este post vamos a ver cómo podemos hacerlo.

Para conectar con Graph desde PowerShell vamos a utilizar el módulo PowerShell de PnP y para ello utilizaremos el commando [Connect-PnPOnline(https://docs.microsoft.com/en-us/powershell/module/sharepoint-pnp/connect-pnponline?view=sharepoint-ps)].

Para conectarnos tenemos 2 opciones, utilizar los permisos de una aplicación registrada en AzureAD, o bien, incluir directamente los scopes que vamos a necesitar en nuestro script.

```powershell
Connect-PnPOnline -AppId $appId -AppSecret $appSecret -AADDomain $appDomain
Connect-PnPOnline -Scopes $scopes
```

Si necesitas ver cómo registrar una aplicación en Azure AD sigue estos [pasos](https://docs.microsoft.com/es-es/azure/active-directory/develop/quickstart-register-app).

Para consultar el listado de permisos de Graph, puedes consultarlos en la siguiente [referencia](https://docs.microsoft.com/es-es/graph/permissions-reference)

Una vez realizada la conexión necesitamos obtener el token que luego incluiremos en la llamada a Graph, para esto utilizaremos el comando [Get-PnPAccessToken](https://docs.microsoft.com/en-us/powershell/module/sharepoint-pnp/get-pnpaccesstoken?view=sharepoint-ps)

```powershell
$accesstoken = Get-PnPAccessToken
```

Una vez obtenido el token lo único que tenemos que hacer es realizar la llamada a Graph, incluyendo el token de autorización, de la siguiente manera

```powershell
Invoke-RestMethod -Uri $url -Method Get -Headers @{Authorization = "Bearer $accesstoken"}
```

Por ejemplo, si necesitamos obtener los Teams en los que estamos unidos ejecutaremos el siguiente script

```powershell
#Conectar usando AppId y AppSecret
#$appId = ''
#$appSecret = ''
#$appDomain = ''
#Connect-PnPOnline -AppId $appId -AppSecret $appSecret -AADDomain $appDomain
$scopes = 'Group.Read.All'
Connect-PnPOnline -Scopes $scopes
$accesstoken = Get-PnPAccessToken
$url = "https://graph.microsoft.com/v1.0/me/joinedTeams"
$myTeams = (Invoke-RestMethod -Uri $url -Method Get -Headers @{Authorization = "Bearer $accesstoken"}).value
$myTeams
```
Y obtendremos nuestro Teams

![Resultado ejecución script](/images/como-trabajar-con-graph-api-desde-powershell-01.png)