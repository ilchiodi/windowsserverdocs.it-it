---
title: Requisiti di sistema per Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/31/2013
ms.prod: windows-server
ms.topic: article
ms.assetid: 0951a67d-492f-41ad-9ae5-8e4cd25e3041
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 72a02084afc78fc12d32ddd32971798a93db14d3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817804"
---
# <a name="system-requirements-for-windows-server-essentials"></a>Requisiti di sistema per Windows Server Essentials

>Si applica a: Windows Server 2019 Essentials, Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
  Il software server di Windows Server Essentials è un sistema operativo solo a 64 bit. La tabella 1 definisce i requisiti hardware minimi consigliati per Windows Server Essentials. La Tabella 2 indica gli altri requisiti hardware e software per il server.  
    
  
## <a name="table-1-system-requirements-for-windows-server-essentials"></a>Tabella 1. Requisiti di sistema per Windows Server Essentials  
  
|Component|Minimo|Consigliato*|Massimo|  
|---------------|-------------|-------------------|-------------|  
|Socket CPU|1,4 GHz (processore a 64 bit) o superiore per singolo core<br /><br /> 1,3 GHz (processore a 64 bit) o superiore per multi-core|3,1 GHz (processore a 64 bit) o superiore per multi-core|2 socket|  
|Memoria (RAM)|2 GB<br /><br /> 4 GB se Windows Server Essentials viene distribuito come macchina virtuale|16 GB|64 GB|  
|Dischi rigidi e spazio di archiviazione disponibile|Disco rigido da 160 GB con una partizione di sistema da 60 GB||Nessun limite|  
  
 *Requisiti hardware consigliati per supportare il numero massimo di utenti e dispositivi.  
  
## <a name="table-2-additional-hardware-and-software-requirements-for-windows-server-essentials"></a>Tabella 2. Requisiti hardware e software aggiuntivi per Windows Server Essentials  
  
|Component|Descrizione|  
|---------------|-----------------|  
|Scheda di rete|Scheda Gigabit Ethernet (10/100/1000baseT PHY/MAC)|  
|Internet|Alcune funzionalità potrebbero richiedere l'accesso a Internet (potrebbe essere a pagamento) o un account Microsoft.|  
|Sistemi operativi client supportati|Windows 8.1, Windows 8, Windows 7, Macintosh OS X dalla versione 10.5 alla versione 10.8.<br /><br /> **Nota:** Alcune funzionalità richiedono edizioni Professional o versioni successive.<br /><br /> 1 GB di spazio disponibile su unità disco rigido (una porzione di questo disco verrà resa disponibile in seguito all'installazione)|  
|Router|Router o firewall che supporti IPv4 NAT o IPv6|  
|Requisiti aggiuntivi|Unità DVD-ROM|  
  
 I requisiti effettivi possono variare in base alla configurazione del sistema, nonché in base alle applicazioni e funzionalità selezionate per l'installazione. Le prestazioni del processore dipendono non solo dalla frequenza di clock del processore, ma anche dal numero di core e dalle dimensioni della cache del processore. I requisiti di spazio di archiviazione per la partizione del sistema sono approssimativi. Potrebbe essere necessario spazio di archiviazione aggiuntivo se l'installazione viene effettuata su una rete.  
  
 Per altre informazioni sui requisiti hardware, vedere il [Catalogo di Windows Server](https://www.windowsservercatalog.com/).  
  
 Tutti i componenti hardware del server devono soddisfare i requisiti stabiliti per il programma logo Windows Server 2012 R2 per i sistemi. Per altre informazioni, vedere [Windows Logo Program](https://msdn.microsoft.com/windows/hardware/gg487403.aspx).  

> [!IMPORTANT]
> I dischi dinamici non sono supportati in Windows Server Essentials.

## <a name="see-also"></a>Vedere anche  
 
-   [Installare Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)  
  
-   [Requisiti di sistema per Windows Server Essentials](system-requirements.md)


