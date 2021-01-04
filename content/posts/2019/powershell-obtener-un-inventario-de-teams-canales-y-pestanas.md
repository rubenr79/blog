---
author: Ruben Ramos
title: "Powershell Obtener Un Inventario De Teams Canales Y Pestanas"
description: "Powershell Obtener Un Inventario De Teams Canales Y Pestanas"
date: 2019-07-12T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: true
---

Con este script de PowerShell podemos obtener un inventario de todos los Teams, canales y pestañas que tenemos en nuestro tenant.

Puedes verlo en mi repositorio de [GitHub](https://github.com/rubenr79/MSTeams-PS/blob/master/Get-AllTeamsTabs.ps1)

```powershell

function Get-AllTeams {
    param([string]$token)
    
    $url = "https://graph.microsoft.com/beta/groups?`$filter=resourceProvisioningOptions/Any(x:x eq 'Team')"
    $response = Invoke-RestMethod -Uri $url -Headers @{Authorization = "Bearer $token"}
    $response.value
}
function Get-TeamChannels {
    param([string]$token, [System.Guid] $teamID)
    
    $url = "https://graph.microsoft.com/v1.0/teams/" + $teamID + "/channels"
    $response = Invoke-RestMethod -Uri $url -Headers @{Authorization = "Bearer $token"}
    $response.value
}
function Get-ChannelTabs {
    param([string]$token, [System.Guid] $teamID, [string] $channelID)
    
    $url = "https://graph.microsoft.com/v1.0/teams/" + $teamID + "/channels/" + $channelID + "/tabs"
    $response = Invoke-RestMethod -Uri $url -Headers @{Authorization = "Bearer $token"}
    $response.value
}
$scopes = 'Group.Read.All'
Connect-PnPOnline -Scopes $scopes
$token = Get-PnPAccessToken
$teams = Get-AllTeams $token
$channels = @()
foreach($team in $teams) {
    $teamChannels = Get-TeamChannels $token $team.id
    foreach($channel in $teamChannels) {
        $channelToAdd = "" | Select-Object "TeamId","TeamName","ChannelId","ChannelName", "ChannelTabs"
        $channelToAdd.TeamId = $team.id
        $channelToAdd.TeamName = $team.displayName
        $channelToAdd.ChannelId = $channel.id
        $channelToAdd.ChannelName = $channel.displayName
        $channelTabs = Get-ChannelTabs $token $team.id $channel.id
        $channelToAdd.ChannelTabs = $channelTabs
        $channels += $channelToAdd
    }
}
$channels | Format-Table

```
El resultado sería un listado como este

![Resultado ejecución script](/images/powershell-obtener-un-inventario-de-teams-canales-y-pestanas-01.png)