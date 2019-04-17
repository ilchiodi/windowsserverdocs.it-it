---
title: Risolvere gli errori di Windows Server Essentials Log Collector
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa2e1685-31c0-4d4f-a10a-6c8885dfc493
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e92236c8e5d956b50f657ebcbe1a942b5841fded
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>Risolvere gli errori di Windows Server Essentials Log Collector

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando si esegue Log Collector, è possibile riscontrare uno dei seguenti errori. Per risolvere un problema, seguire le indicazioni fornite per l'errore associato.  
  
> [!NOTE]
>  In questo documento, i computer in rete, diversi dal server, sono detti computer della rete.?  
  
###  <a name="BKMK_TheDestinationFolderIsNotValid"></a>La cartella di destinazione non è valida  
 **Causa:** la cartella in cui si sta cercando di copiare i file di log potrebbe non esistere o non dispone di spazio sufficiente per contenere i file.  
  
 **Soluzione:** controllare che la cartella selezionata esista e che è disponibile spazio libero sufficiente nell'unità per i file. È inoltre necessario assicurarsi che vi sia spazio libero rimanente nella cartella temp sull'unità.  
  
###  <a name="BKMK_ANetworkErrorHasOccurred"></a>Si è verificato un errore di rete  
 **Causa:** potrebbe essersi verificato un problema relativo alla rete nel computer di rete o server.  
  
 **Soluzione:** assicurarsi che tutti i computer e dispositivi di rete siano accesi e connessi alla rete. Se non si riesce a risolvere il problema, contattare la persona che gestisce la rete per assistenza.  
  
###  <a name="BKMK_CannotCollectLogFiles"></a>Non è possibile raccogliere i file di registro per il computer  
 **Causa:** Log Collector potrebbe non essere installato nel computer perché il computer non si è connesso al server con connessione del Computer per la creazione guidata Server.  
  
 **Soluzione:** per informazioni su come risolvere i problemi relativi alle connessioni al server, vedere [risoluzione dei problemi di connessione dei computer al server](https://go.microsoft.com/fwlink/p/?LinkID=241492)? (https://go.microsoft.com/fwlink/p/?LinkID=241492) nel sito Web Microsoft.  
  
 Se non si riesce ancora a connettere il computer al server, quindi è possibile copiare i file di log manualmente in un'unità flash USB come indicato di seguito:  
  
-   Per i computer client che eseguono Windows 7, Windows 8 o Windows Multipoint Server, è possibile copiare il **registri** cartella disponibile all'indirizzo **%sysdir%\programdata\Microsoft\Windows Server**.  
  
###  <a name="BKMK_YouDoNotHavePermission"></a>Non si ha l'autorizzazione per salvare i file di log nella cartella selezionata  
 **Causa:** non non dispone dell'autorizzazione di scrittura nella cartella selezionata per salvare i file di registro.  
  
 **Soluzione:** se si utilizza il percorso predefinito per salvare i file di log, verificare di disporre dell'autorizzazione di scrittura per la cartella condivisa **\ \ \ < ServerName\ > \Logs**. Se si archiviano i registri in un computer di rete, assicurarsi di disporre dell'autorizzazione di scrittura per la cartella selezionata per salvare i file di registro.  
  
###  <a name="BKMK_TheComputerIsNotConfiguredProperly"></a>Il computer non è configurato correttamente per raccogliere i file di registro  
 **Causa:** il computer non è stato configurato correttamente per Log Collector.  
  
 **Soluzione:** reinstallare Log Collector. Vedere [reinstallazione di Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall).  
  
###  <a name="BKMK_AnUnknownErrorOccurred"></a>Si è verificato un errore sconosciuto  
 **Causa:** sconosciuto.  
  
 **Soluzione 1:** rieseguire Log Collector. Se l'errore persiste, assicurarsi di che non siano presenti problemi di connettività. È anche possibile provare a reinstallare Log Collector. Vedere [reinstallazione di Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Se non si riesce a risolvere il problema, contattare la persona che gestisce la rete per assistenza.  
  
 **Soluzione 2:** provare prima ad aprire la cartella in cui vengono salvati i file di registro. Se il file zip con nome del computer è già stato generato, ignorare questo errore e usare invece i file di registro. Se sono presenti file di log generati, rieseguire Log Collector. Se l'errore persiste, assicurarsi di che non siano presenti problemi di connettività. È anche possibile provare a reinstallare Log Collector. Vedere [reinstallazione di Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Se non si riesce a risolvere il problema, contattare la persona che gestisce la rete per assistenza.