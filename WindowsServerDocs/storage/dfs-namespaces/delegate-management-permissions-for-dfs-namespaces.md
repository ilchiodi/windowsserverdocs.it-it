---
title: Delegare le autorizzazioni di gestione per Spazi dei nomi DFS
description: Questo articolo descrive come delegare le autorizzazioni di gestione per Spazi dei nomi DFS e indica i gruppi che possono eseguire attività di spazio dei nomi per impostazione predefinita
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7895432ca16dd13c6425d966f99104fc03db100d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829492"
---
# <a name="delegate-management-permissions-for-dfs-namespaces"></a>Delegare le autorizzazioni di gestione per Spazi dei nomi DFS

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Nella tabella seguente vengono descritti i gruppi che possono eseguire attività di base dello spazio dei nomi per impostazione predefinita e il metodo per delegare la possibilità di eseguire queste attività:

|Attività | Gruppi che possono eseguire questa attività per impostazione predefinita | Metodo di delega |
|---|---|---|
|Creare un nuovo spazio dei nomi basato su dominio|Gruppo Domain Admins nel dominio in cui viene configurato lo spazio dei nomi|Nell'albero della console fai clic con il pulsante destro del mouse sul nodo **Spazi dei nomi**, quindi scegli **Delega autorizzazioni di gestione**. In alternativa, usa i cmdlet [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) e [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot). Cmdlet Windows PowerShell (introdotti in Windows Server 2012). È inoltre necessario aggiungere l'utente al gruppo Administrators locale del server dello spazio dei nomi.|
|Aggiungere un server dello spazio dei nomi a uno spazio dei nomi basato su dominio|Gruppo Domain Admins nel dominio in cui viene configurato lo spazio dei nomi| Nell'albero della console fai clic con il pulsante destro del mouse sullo spazio dei nomi basato su dominio, quindi scegli **Delega autorizzazioni di gestione**. In alternativa, usa i cmdlet [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) e [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot). Cmdlet Windows PowerShell (introdotti in Windows Server 2012). È inoltre necessario aggiungere l'utente al gruppo Administrators locale del server dello spazio dei nomi da aggiungere.|
|Gestire uno spazio dei nomi basato su dominio|Gruppo Administrators locale su ogni server dello spazio dei nomi| Nell'albero della console fai clic con il pulsante destro del mouse sullo spazio dei nomi basato su dominio, quindi scegli **Delega autorizzazioni di gestione**. |
|Creare uno spazio dei nomi autonomo|Gruppo Administrators locale sul server dello spazio dei nomi| Aggiungere l'utente al gruppo Administrators locale del server dello spazio dei nomi. |
|Gestire uno spazio dei nomi autonomo*|Gruppo Administrators locale sul server dello spazio dei nomi| Nell'albero della console fai clic con il pulsante destro del mouse sullo spazio dei nomi autonomo, quindi scegli **Delega autorizzazioni di gestione**. In alternativa, usa i cmdlet [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) e [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot). Cmdlet Windows PowerShell (introdotti in Windows Server 2012).|
|Creare un gruppo di replica o abilitare la Replica DFS in una cartella|Gruppo Domain Admins nel dominio in cui viene configurato lo spazio dei nomi| Nell'albero della console fai clic con il pulsante destro del mouse sul nodo Replica, quindi scegli **Delega autorizzazioni di gestione**. |

<br />

\*Delega delle autorizzazioni di gestione per gestire uno spazio dei nomi autonomo non concede all'utente la possibilità di visualizzare e gestire la sicurezza tramite il **delega** scheda a meno che l'utente sia un membro del gruppo Administrators locale sul server dello spazio dei nomi. Questo problema si verifica perché lo snap-in Gestione DFS non può recuperare gli elenchi di controllo di accesso discrezionali (DACL) per lo spazio dei nomi autonomo dal Registro di sistema. Per abilitare lo snap-in visualizzare informazioni sulla delega, è necessario seguire i passaggi in Microsoft<sup>®</sup> articolo della Knowledge Base: [KB314837: Come gestire l'accesso remoto al Registro di sistema](https://go.microsoft.com/fwlink?linkid=46803)