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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836662"
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>Risolvere gli errori di Windows Server Essentials Log Collector

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando si esegue Log Collector, è possibile che vengano visualizzati errori simili ai seguenti. Per risolvere un problema, seguire le informazioni aggiuntive fornite per l'errore associato.  
  
> [!NOTE]
>  In questo documento, i computer in rete, diversi dal server, vengono chiamati i computer di rete.?  
  
###  <a name="BKMK_TheDestinationFolderIsNotValid"></a> La cartella di destinazione non è valida  
 **Causa:** la cartella in cui si sta cercando di copiare i file di log potrebbe non esistere o non avere abbastanza spazio per contenere i file.  
  
 **Soluzione:** controllare che la cartella selezionata esista e che lo spazio disponibile sull'unità sia sufficiente per i file. È anche consigliabile verificare che lo spazio disponibile rimanente nella cartella temp sull'unità sia sufficiente.  
  
###  <a name="BKMK_ANetworkErrorHasOccurred"></a> Si è verificato un errore di rete  
 **Causa:** nel computer di rete o nel server potrebbe esserci un problema relativo alla rete.  
  
 **Soluzione:** verificare che tutti i computer e i dispositivi di rete siano accesi e connessi alla rete. Se non è possibile risolvere il problema, contattare la persona che gestisce la rete per richiedere assistenza.  
  
###  <a name="BKMK_CannotCollectLogFiles"></a> Non è possibile raccogliere i file di log per il computer  
 **Causa:** Log Collector potrebbe non essere installato nel computer perché il computer non si è connesso al server con la procedura guidata Connessione del computer al server.  
  
 **Soluzione:** Per informazioni su come risolvere i problemi relativi alle connessioni al server, vedere [risolvere i problemi di connessione dei computer al server](https://go.microsoft.com/fwlink/p/?LinkID=241492)? (https://go.microsoft.com/fwlink/p/?LinkID=241492) presso il sito Web Microsoft.  
  
 Se non è ancora possibile connettere il computer al server, è possibile copiare i file di log manualmente in un'unità flash USB nel modo seguente:  
  
-   Per i computer client che eseguono Windows 7, Windows 8 o Windows Multipoint Server, è possibile copiare la cartella **Logs** che si trova nel percorso **%sysdir%\programdata\Microsoft\Windows Server**.  
  
###  <a name="BKMK_YouDoNotHavePermission"></a> Non hai le autorizzazioni per salvare i file di log nella cartella selezionata  
 **Causa:** è possibile che non si abbia l'autorizzazione di scrittura per la cartella selezionata per salvare i file di log.  
  
 **Soluzione:** Se si utilizza il percorso predefinito per salvare i file di log, assicurarsi di avere l'autorizzazione di scrittura per la cartella condivisa  **\\ \\< nomeserver\>\Logs**. Se si archiviano i registri in un computer di rete, verificare di avere l'autorizzazione di scrittura per la cartella selezionata per il salvataggio dei file di log.  
  
###  <a name="BKMK_TheComputerIsNotConfiguredProperly"></a> Il computer non è configurato correttamente per raccogliere i file di log  
 **Causa:** il computer non è stato configurato correttamente per Log Collector.  
  
 **Soluzione:** reinstallare Log Collector Vedere [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall).  
  
###  <a name="BKMK_AnUnknownErrorOccurred"></a> Si è verificato un errore sconosciuto  
 **Causa:** sconosciuta.  
  
 **Soluzione 1:** rieseguire Log Collector Se l'errore si ripete, verificare che non ci siano problemi di connettività. È anche possibile provare a reinstallare Log Collector. Vedere [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Se non è possibile risolvere il problema, contattare la persona che gestisce la rete per richiedere assistenza.  
  
 **Soluzione 2:** Provare prima ad aprire la cartella in cui sono stati salvati i file di log. Se il file zip con il nome computer è già stato generato, ignorare questo errore e usare invece i file di log. Se non ci sono file di log generati, rieseguire Log Collector. Se l'errore si ripete, verificare che non ci siano problemi di connettività. È anche possibile provare a reinstallare Log Collector. Vedere [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Se non è possibile risolvere il problema, contattare la persona che gestisce la rete per richiedere assistenza.