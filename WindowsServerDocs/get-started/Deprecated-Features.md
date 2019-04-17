---
title: Funzionalità rimosse o deprecate in Windows Server 2016
description: Caratteristiche e funzionalità rimosse o di cui è pianificata la rimozione nelle versioni.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 05/02/2017
ms.assetid: 5d10c5f9-ebac-49a0-b808-c0b1702e0437
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 20178a3be14c076623f647fa139e013528de9a69
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133887"
---
# Funzionalità rimosse o deprecate in Windows Server 2016

>Si applica a: Windows Server 2016

Di seguito è riportato un elenco di caratteristiche e funzionalità di Windows Server 2016 che sono state rimosse dal prodotto nella versione corrente o di cui è pianificata la potenziale rimozione nelle versioni successive (deprecate). Questo elenco è destinato ai professionisti IT responsabili dell'aggiornamento dei sistemi operativi in un ambiente commerciale. L'elenco è soggetto a modifiche nelle versioni successive e potrebbe non includere tutte le funzioni o funzionalità deprecate. Per ulteriori dettagli su una particolare caratteristica o funzionalità e la relativa sostituzione, vedere la documentazione relativa a tale funzionalità.  

## Funzionalità rimosse da Windows Server 2016 
Le caratteristiche e le funzionalità seguenti sono state rimosse da questa versione di Windows Server 2016. Utilizzo,codice o applicazioni che dipendono da queste funzionalità non saranno disponibili in questa versione, a meno di utilizzare un metodo alternativo.  

> [!NOTE]  
> Se si passa a Windows Server 2016 da una versione server precedente a Windows Server 2012 R2 o windows Server 2012, è consigliabile consultare anche [Funzionalità rimosse o deprecate in Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx) e [Funzionalità rimosse o deprecate in Windows Server 2012](https://technet.microsoft.com/library/hh831568.aspx).  


### File server  
Lo snap-in Gestione condivisione e archiviazione per Microsoft Management Console è stato rimosso. Effettuare una delle operazioni riportate di seguito:  

-   Se il computer che si vuole gestire esegue un sistema operativo precedente a Windows Server 2016, connettersi a tale computer con Desktop remoto e usare la versione locale dello snap-in Gestione condivisione e archiviazione.  

-   In un computer che esegue Windows 8.1 o versioni precedenti, usare lo snap-in Gestione condivisione e archiviazione da Strumenti di amministrazione remota del server per visualizzare il computer che si desidera gestire.  

-   Usare Hyper-V in un computer client per l'esecuzione di una macchina virtuale con Windows 7, Windows 8 o Windows 8.1 e che include lo snap-in Gestione condivisione e archiviazione remota in Strumenti di amministrazione remota del server.  

### Journal.dll  
Journal.dll è stato rimosso da Windows Server 2016. Non è prevista alcuna sostituzione.  

### Configurazione guidata impostazioni di sicurezza  
La Configurazione guidata impostazioni di sicurezza è stata rimossa. Le funzionalità sono protette per impostazione predefinita. Per controllare impostazioni di protezione specifiche, è possibile usare Criteri di gruppo o [Microsoft Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx).  

### SQM  
I componenti di consenso esplicito che gestiscono la partecipazione al programma Analisi utilizzo software sono stati rimossi. 

### Windows Update
Il comando **wuauclt.exe /detectnow** è stato rimosso e non è più supportato. Per attivare un'analisi per gli aggiornamenti, effettuare una delle operazioni seguenti:

- Eseguire questi comandi di PowerShell:
    ````powershell
    $AutoUpdates = New-Object -ComObject "Microsoft.Update.AutoUpdate"`
    $AutoUpdates.DetectNow()` 
    ````

- In alternativa, utilizzare questo VBScript:
    ````vb
    Set automaticUpdates = CreateObject("Microsoft.Update.AutoUpdate")
    automaticUpdates.DetectNow()
    ````

## Funzionalità deprecate a partire da Windows Server 2016 
Le seguenti caratteristiche e funzionalità sono deprecate a partire da questa versione. Saranno successivamente rimosse completamente da questo prodotto, ma sono ancora disponibili in questa versione, a volte con alcune funzionalità rimosse. È consigliabile iniziare ora a pianificare l'utilizzo di metodi alternativi per l'utilizzo, il codice o le applicazioni che dipendono da queste funzionalità.  

### Strumenti di configurazione  

-   **Scregedit.exe** è deprecato. Se si dispone di script che dipendono da Scregedit.exe, modificarli in modo da usare i metodi Reg.exe o Windows PowerShell.  

-   **Sconfig.exe** è deprecato. Usare invece Windows PowerShell.  

### API personalizzate di NetCfg  
L'installazione di PrintProvider, NetClient e ISDN usando le API personalizzate di NetCfg è deprecata.  

### Gestione remota  
WinRM.vbs è deprecato. Usare invece la funzionalità del provider WinRM di Windows PowerShell.  

### SMB  
SMB 2 + su NetBT è deprecato. Implementare SMB su TCP o RDMA. 
