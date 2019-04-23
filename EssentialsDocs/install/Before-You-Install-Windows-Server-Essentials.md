---
title: Prima di installare Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d0893bd-e2b7-4494-9537-02b1cbbcd57a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e7eb1b7bed780b41f1a87589add4ab015f41624a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828722"
---
# <a name="before-you-install-windows-server-essentials"></a>Prima di installare Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_BeforeYouBegin"></a> Prima di iniziare l'installazione di Windows Server Essentials, eseguire le attività seguenti:  

-   **Verificare che il computer soddisfi i requisiti hardware minimi**. Ciò include stabilire se sono necessari ulteriori componenti hardware e verificare che i driver per l'hardware siano supportati da Windows Server Essentials. Per altre informazioni, vedere [requisiti di sistema per Windows Server Essentials](../get-started/system-requirements.md).   

  
    > [!IMPORTANT]
    >  Prima di installare Windows Server Essentials in un computer preesistente, è consigliabile di formattare completamente e ripartizionare i dischi rigidi del computer pre-esistenti. La formattazione e la ripartizione delle unità disco rigido evitano che sui dischi rigidi restino delle partizioni nascoste.  
  
-   **Preparare la rete** per preparare la rete per installare Windows Server Essentials, eseguire le operazioni seguenti:  
    
  
    -   **Eseguire l'aggiornamento del sistema operativo nei computer client** Windows Server Essentials supporta i sistemi operativi seguenti:  Windows 8, Windows 7, Windows 10 e Macintosh OS X Lion o versioni successive. Questi sistemi operativi forniscono le funzionalità di sicurezza e i livelli di affidabilità, prestazioni e funzionalità necessari per la rete locale.  
  
    -   **Configurare il router** Verificare che il router sia configurato nel seguente modo:  
  
        -   Sul router è abilitato il framework UPnP.  
  
        -   Il servizio server DHCP (Dynamic Host Configuration Protocol) per la rete LAN può essere abilitato o disabilitato.  Windows Server Essentials assicura che DHCP non è in esecuzione nel router sia nel server? Quando DHCP è abilitato sul router, non è abilitato nel server durante l'installazione.  
  
        -   Si dispone di un indirizzo IP per l'interfaccia esterna del router fornito dal provider di servizi Internet. L'indirizzo IP può essere assegnato dinamicamente dal servizio server DHCP presso il provider di servizi Internet oppure è necessario configurare manualmente un indirizzo IP statico usando la console di gestione del router.  
  
        -   Se la connessione Internet richiede un nome utente e una password, collettivamente indicati come Point-to-Point Protocol over Ethernet (PPPoE), queste impostazioni vengono configurate sul router, anche se il dispositivo supporta il framework UPnP.  
  
        -   Il router è connesso a una rete LAN e Internet, è accesso e funzionante.  
  
     Se il router non supporta il framework UPnP o se non può essere configurato durante l'installazione, è necessario configurarlo manualmente con le impostazioni della rete. Assicurarsi che le porte seguenti siano aperte e che facciano riferimento all'indirizzo IP del server di destinazione:  
  
    |Numero di porta|Applicazione|  
    |-----------------|-----------------|  
    |Porta 80|Traffico Web HTTP|  
    |Porta 443|Traffico Web HTTPS|  
  

-   **Leggere documentazione sulla versione di Windows Server Essentials**. Documentazione sulla versione contiene le informazioni più recenti che potrebbero essere fondamentali per l'installazione e configurazione di Windows Server Essentials in modo corretto. Per visualizzare e stampare la documentazione sulla versione, vedere [Release Documentation for Windows Server Essentials](../get-started/release-notes.md).  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Installare Windows Server Essentials](Install-Windows-Server-Essentials.md)

