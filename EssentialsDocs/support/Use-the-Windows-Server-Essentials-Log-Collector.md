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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877992"
---
# <a name="use-the-windows-server-essentials-log-collector"></a>Usare Windows Server Essentials Log Collector

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando si siano risolvendo i problemi dei computer, un rappresentante del cliente servizio supporto tecnico Microsoft potrebbe chiedere di raccogliere i log dal server, i computer in rete, o entrambi usando Windows Server Essentials Log Collector.  
  
 Log Collector copia registri di programmi, registri di revisori eventi e informazioni sull'ambiente correlato in un unico file zip in un percorso specificato. È possibile eseguire Log Collector direttamente dal server o da uno dei computer in rete oppure usando una connessione remota ai computer.  
  
> [!NOTE]
>  -   Log Collector non analizza i problemi della rete e non apporta modifiche ad alcun server o computer in rete. Per informazioni su come risolvere i problemi relativi alla rete, vedere la documentazione della Guida per il prodotto server.  
> -   In questa Guida, i computer in rete, diversi dal server, vengono chiamati i computer di rete.  
> -   [Scaricare il pacchetto di installazione di Windows Server Essentials Log Collector](https://go.microsoft.com/fwlink/?LinkID=266341).  
  
 Per installare ed eseguire Log Collector, attenersi ai passaggi negli argomenti seguenti:  
  

1.  [Installare l'agente di raccolta Log](Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [Eseguire Log Collector](Run-the-Windows-Server-Essentials-Log-Collector.md)  

1.  [Installare l'agente di raccolta Log](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [Eseguire Log Collector](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)  

  
## <a name="environment-information-collected"></a>Informazioni sull'ambiente raccolte  
 Per ogni computer di rete o server specificato, Log Collector raccoglie le seguenti informazioni sull'ambiente e le inserisce nel file della raccolta dei registri.  
  
-   Versione del sistema operativo  
  
-   Produttore e descrizione della CPU  
  
-   Quantità e allocazione della memoria  
  
-   Schede di rete associate a TCP/IP  
  
-   Locale  
  
-   Processi  
  
-   Configurazione dell'archiviazione  
  
-   Informazioni sul file host  
  
-   Registri eventi, inclusi il registro applicazioni, il registro di sistema, Windows Server e Media Center  
  
-   Messaggi di Gestione controllo servizi  
  
-   Eventi di riavvio ed eventi di Windows Update  
  
-   Errori di sistema ed errori delle applicazioni  
  
## <a name="services-information-collected"></a>Informazioni sui servizi raccolte  
  
### <a name="server-services"></a>Servizi del server  
  
-   Servizio Backup computer client Windows Server  
  
-   Servizio Provider backup computer client Windows Server  
  
-   Provider dispositivi Windows Server  
  
-   Gestione nome dominio Windows Server  
  
-   Registro provider servizi Windows Server  
  
-   Provider impostazioni Windows Server  
  
-   Servizio dispositivi UPnP Windows Server  
  
-   Provider amministrazione Accesso Web remoto Windows Server  
  
-   Servizio integrità Windows Server  
  
-   Servizio di archiviazione server Windows  
  
-   Servizio SQM Windows Server  
  
### <a name="network-computer-services"></a>Servizi dei computer di rete  
  
-   Servizio Provider backup computer client Windows Server  
  
-   Servizio integrità Windows Server  
  
-   Registro provider servizi Windows Server  
  
-   Servizio SQM Windows Server  
  
## <a name="logs-and-registry-information-collected"></a>Informazioni sui registri e sul Registro di sistema raccolte  
 Per ogni computer di rete o server specificato, Log Collector raccoglie informazioni sui registri e sul Registro di sistema dal server e dal computer di rete nel modo seguente.  
  
### <a name="server-logs-and-registry-information"></a>Informazioni sui registri e sul Registro di sistema del server  
  
-   Registri del prodotto server, da < ProgramData\>\Microsoft\Windows Server\Logs  
  
-   Attività pianificate  
  
-   Registri delle API di installazione  
  
-   Registri di Windows Update  
  
-   File di avvisi di integrità  
  
-   File di informazioni sui dispositivi  
  
-   File di log di Backup server  
  
-   File di log di Panther  
  
-   Servizi  
  
-   Chiavi del Registro di sistema, da  
  
    -   \\\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DevicesProviderSvc  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc  
  
### <a name="network-computer-logs-and-registry-information"></a>Informazioni sui registri e sul Registro di sistema dei computer di rete  
  
-   Registri del prodotto computer di rete in < ProgramData\>\Microsoft\Windows Server\Logs  
  
-   File di avvisi di integrità in < ProgramData\>Server\Data  
  
-   Registri di Windows Update  
  
-   Registri delle API di installazione  
  
-   Informazioni sulle attività pianificate  
  
-   Le chiavi del Registro di sistema da \\\HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Server\  
  
## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>Registri per i computer che non eseguono una versione del sistema operativo Windows  
 Log Collector non raccoglie file di log dai computer che non eseguono una versione del sistema operativo Windows. Per i computer non Windows, copiare manualmente i seguenti file di log nella stessa posizione in cui si archiviano i file di Log Collector.  
  
-   System.log  
  
-   Library/Logs/Windows Server.log  
  
-   Library/Logs/CrashReporter/LaunchPad-< nnn\> (copiare tutti LaunchPad-< nnn\>file crash)  
  
-   Library/Logs/DiagnosticReports/LaunchPad-< nnn\> (copiare tutti LaunchPad-< nnn\>file crash)  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Risolvere gli errori dell'agente di raccolta Log](Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

-   [Risolvere gli errori dell'agente di raccolta Log](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

