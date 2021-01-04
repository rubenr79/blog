---
author: Ruben Ramos
title: "Powershell Obtener Todos Los Teams Sin Propietario"
description: "Powershell Obtener Todos Los Teams Sin Propietario"
date: 2019-08-08T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: true
---

Con este script de PowerShell obtendremos todos los Teams que no tiene ningún usuario asignado como propietario.

Puedes verlo en mi repositorio de [GitHub](https://github.com/rubenr79/MSTeams-PS/blob/master/Get-TeamsWithoutOwner.ps1)

```powershell

function Get-AllTeams {
    param([string]$token)
    
    $url = "https://graph.microsoft.com/beta/groups?`$filter=resourceProvisioningOptions/Any(x:x eq 'Team')"
    $response = Invoke-RestMethod -Uri $url -Headers @{Authorization = "Bearer $token"}
    $response.value
}
$scopes = 'Group.Read.All'
Connect-PnPOnline -Scopes $scopes
$token = Get-PnPAccessToken
$teams = Get-AllTeams $token
$result = @()
foreach($team in $teams) {
    $owners = Get-TeamOwners $token $team.id
    if ($owners -eq $null) {
        $members = Get-TeamMembers $token $team.id
        $teamToAdd = "" | Select-Object "TeamId","TeamName"
        $teamToAdd.TeamId = $team.id
        $teamToAdd.TeamName = $team.displayName
        $result += $teamToAdd
    }
}
$result

```