---
author: Ruben Ramos
title: "Powershell Eliminar Todas Las Pestanas Wiki Vacias"
description: "Powershell Eliminar Todas Las Pestanas Wiki Vacias"
date: 2019-07-14T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: true
---

Lo reconozco, en Teams, soy más partidario de utilizar OneNote que de utilizar wikis. Con este script vamos a poder eliminar todas las pestañas wiki, que no tengan contenido de todo Teams.

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
    
    $url = "https://graph.microsoft.com/v1.0/teams/" + $teamID + "/channels/" + $channelID + "/tabs/"
    $response = Invoke-RestMethod -Uri $url -Headers @{Authorization = "Bearer $token"}
    $response.value
}
function Remove-ChannelTab {
    param([string]$token, [System.Guid] $teamID, [string] $channelID, [System.Guid] $tabID)
    
    $url = "https://graph.microsoft.com/v1.0/teams/" + $teamID + "/channels/" + $channelID + "/tabs/" + $tabID
    Invoke-RestMethod -Uri $url -Headers @{Authorization = "Bearer $token"} -Method Delete
    
}
$scopes = 'Group.ReadWrite.All'
Connect-PnPOnline -Scopes $scopes
$token = Get-PnPAccessToken
$teams = Get-AllTeams $token
foreach($team in $teams) {
    $teamChannels = Get-TeamChannels $token $team.id
    foreach($channel in $teamChannels) {
        
        $channelTabs = Get-ChannelTabs $token $team.id $channel.id
        foreach($tab in $channelTabs) {
            if ([bool]($tab.configuration.wikiTabId) -and -not [bool]($tab.configuration.hasContent)) {
                Remove-ChannelTab $token $team.id $channel.id $tab.id
            }
        }
    }
}

```

Puedes verlo en mi repositorio de [GitHub](https://github.com/rubenr79/MSTeams-PS/blob/master/Remove-EmptyWikiTabs.ps1)