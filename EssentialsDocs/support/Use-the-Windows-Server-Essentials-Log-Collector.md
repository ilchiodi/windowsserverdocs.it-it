---
title: Usare Windows Server Essentials Log Collector
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c6985518-b42d-4cfb-9761-eaa75306b6d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d003c6a45159548f7e34d86ca242f74868659d2f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="use-the-windows-server-essentials-log-collector"></a>Usare Windows Server Essentials Log Collector

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Durante la risoluzione di problemi dei computer, un rappresentante del cliente servizio supporto tecnico Microsoft potrebbe chiedere di raccogliere i registri dal server, computer in rete o entrambi usando Windows Server Essentials Log Collector.  
  
 Log Collector copia registri di programmi, registri di revisori eventi e informazioni sull'ambiente correlato in un unico file zip in una posizione specificata. È possibile eseguire Log Collector direttamente dal server o da qualsiasi computer della rete o tramite una connessione remota ai computer.  
  
> [!NOTE]
>  -   Log Collector non analizza i problemi di rete o apportare modifiche a qualsiasi server o computer in rete. Per informazioni su come risolvere i problemi di rete, vedere la documentazione della Guida per il prodotto server.  
> -   In questa Guida, i computer in rete, diversi dal server, sono detti computer della rete.  
> -   [Scaricare il pacchetto di installazione di Windows Server Essentials Log Collector](https://go.microsoft.com/fwlink/?LinkID=266341).  
  
 Per installare ed eseguire Log Collector, eseguire i passaggi descritti negli argomenti seguenti:  
  

1.  [Installare Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [Eseguire Log Collector](Run-the-Windows-Server-Essentials-Log-Collector.md)  

1.  [Installare Log Collector](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [Eseguire Log Collector](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)  

  
## <a name="environment-information-collected"></a>Informazioni sull'ambiente raccolte  
 Per ogni computer di rete o server specificato, Log Collector raccoglie le seguenti informazioni sull'ambiente e inserisce nel file della raccolta dei log.  
  
-   Versione del sistema operativo  
  
-   Descrizione e dal produttore della CPU  
  
-   Allocazione e la quantità di memoria  
  
-   Schede di rete associate a TCP/IP  
  
-   Impostazioni locali  
  
-   Processi  
  
-   Configurazione dell'archiviazione  
  
-   Informazioni sul file host  
  
-   Registri eventi inclusi applicazione, sistema, Windows Server e Media Center  
  
-   Messaggi di Gestione controllo servizi  
  
-   Riavviare gli eventi e gli eventi di Windows Update  
  
-   Errori di sistema ed errori dell'applicazione  
  
## <a name="services-information-collected"></a>Informazioni sui servizi raccolte  
  
### <a name="server-services"></a>Servizi server  
  
-   Servizio Backup Computer Client Server Windows  
  
-   Servizio Provider Backup Computer Client di Windows Server  
  
-   Provider dispositivi Windows Server  
  
-   Gestione nome di dominio di Windows Server  
  
-   Registro Provider servizi di Windows Server  
  
-   Provider impostazioni Windows Server  
  
-   Servizio dispositivi UPnP Windows Server  
  
-   Provider amministrazione accesso Web remoto di Server Windows  
  
-   Servizio integrità Windows Server  
  
-   Servizio di archiviazione di Windows Server  
  
-   Servizio SQM Windows Server  
  
### <a name="network-computer-services"></a>Servizi di computer di rete  
  
-   Servizio Provider Backup Computer Client di Windows Server  
  
-   Servizio integrità Windows Server  
  
-   Registro Provider servizi di Windows Server  
  
-   Servizio SQM Windows Server  
  
## <a name="logs-and-registry-information-collected"></a>Informazioni sui registri e del Registro di sistema raccolte  
 Per ogni computer di rete o server specificato, Log Collector raccoglie informazioni di registro e del Registro di sistema dal server e computer di rete come indicato di seguito.  
  
### <a name="server-logs-and-registry-information"></a>Log del server e le informazioni del Registro di sistema  
  
-   Registri del prodotto server, da \Microsoft\Windows Server\Logs < ProgramData\ >  
  
-   Attività pianificate  
  
-   Log di installazione API  
  
-   Registri di Windows Update  
  
-   File di avvisi di integrità  
  
-   File di informazioni sui dispositivi  
  
-   File di Log di Backup server  
  
-   File di Log di Panther  
  
-   Servizi  
  
-   Le chiavi del Registro di sistema, da  
  
    -   \\\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DevicesProviderSvc  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc  
  
### <a name="network-computer-logs-and-registry-information"></a>I registri dei computer di rete e le informazioni del Registro di sistema  
  
-   Registri del prodotto computer di rete in < ProgramData\ > \Microsoft\Windows Server\Logs  
  
-   File di avvisi di integrità in < ProgramData\ > \Microsoft\Windows Server\Data  
  
-   Registri di Windows Update  
  
-   Log di installazione API  
  
-   Informazioni sulle attività pianificate  
  
-   Chiavi del Registro di sistema da \\\HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Server\  
  
## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>Registri per i computer che non eseguono una versione del sistema operativo Windows  
 Log Collector non raccoglie file di log dai computer che non eseguono una versione del sistema operativo Windows. Per i computer non Windows, copiare manualmente i file di log seguenti nella stessa posizione in cui si archiviano i file di Log Collector.  
  
-   System. log  
  
-   Library/registri/Windows Server  
  
-   Library/Logs/CrashReporter/LaunchPad-< nnn\ > (copiare tutti i file LaunchPad-< nnn\ >. crash)  
  
-   Library/Logs/DiagnosticReports/LaunchPad-< nnn\ > (copiare tutti i file LaunchPad-< nnn\ >. crash)  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Risolvere gli errori di Log Collector](Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

-   [Risolvere gli errori di Log Collector](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

