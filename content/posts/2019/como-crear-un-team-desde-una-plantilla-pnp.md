---
author: Ruben Ramos
title: "Como Crear Un Team Desde Una Plantilla Pnp"
description: "Como Crear Un Team Desde Una Plantilla Pnp"
date: 2019-06-03T00:00:00+01:00
featured_image: "/images/MSTeams.png"
categories:
- 
tags: []
draft: false
---

Desde la versión 2019-03 del esquema de aprovisionamiento de PnP es posible la creación de Teams.

Para ver un ejemplo, creamos un xml llamado msteams-pnptemplate.xml y copiamos lo siguiente

```xml
<?xml version="1.0"?>
<pnp:Provisioning xmlns:pnp="http://schemas.dev.office.com/PnP/2019/03/ProvisioningSchema"
        Author="Ruben Ramos"
        Generator=""
        Version="1.0"
        Description="Provisioning teams from PnP template"
        DisplayName="Teams Provisioning"
        ImagePreviewUrl="https://docs.microsoft.com/es-es/media/logos/logo_MSTeams.svg">
    <pnp:Teams>
        <pnp:Team  DisplayName="PnP Template Team" Description="My first team from PnP template"
              Visibility="Public" MailNickname="pnpteam" Archived="false">
            <pnp:FunSettings AllowGiphy="true" GiphyContentRating="Strict"
                               AllowStickersAndMemes="true" AllowCustomMemes="true"/>
            <pnp:GuestSettings AllowCreateUpdateChannels="true" AllowDeleteChannels="false"/>
            <pnp:MembersSettings AllowCreateUpdateChannels="true" AllowDeleteChannels="false"
                                   AllowAddRemoveApps="true" 
                                   AllowCreateUpdateRemoveConnectors="true" 
                                   AllowCreateUpdateRemoveTabs="false" />
            <pnp:MessagingSettings AllowUserEditMessages="true" AllowUserDeleteMessages="true"
                                     AllowOwnerDeleteMessages="false"
                                     AllowTeamMentions="true"
                                     AllowChannelMentions="true"/>     
            <pnp:Security>
                <pnp:Owners ClearExistingItems="true">
                    <pnp:User UserPrincipalName="<user>" />
                </pnp:Owners>
                <pnp:Members ClearExistingItems="false">
                    <pnp:User UserPrincipalName="<user>" />
                </pnp:Members>
            </pnp:Security>
            <pnp:Channels>
                <pnp:Channel DisplayName="Channel 1"
                                 Description="this channel was created with PnP Template"
                                 IsFavoriteByDefault="true">
                </pnp:Channel>
            </pnp:Channels>
        </pnp:Team>
    </pnp:Teams>
                  
</pnp:Provisioning>
```

Una vez creamos nuestra plantilla, lo único que tendremos que hacer es conectarnos a nuestro tenant y aplicar la plantilla

```powershell
Connect-PnPOnline https://<tenant>.sharepoint.com 
Apply-PnPTenantTemplate -Path .\msteams-pnptemplate.xml
```

Una vez terminada la ejecución, podremos ver nuestro nuevo Team
![Resultado ejecución script](/images/como-crear-un-team-desde-una-plantilla-pnp-01.png)
