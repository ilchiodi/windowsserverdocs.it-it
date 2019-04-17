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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="before-you-install-windows-server-essentials"></a>Prima di installare Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_BeforeYouBegin"></a>Prima di iniziare l'installazione di Windows Server Essentials, eseguire le attività seguenti:  

-   **Assicurarsi che il computer soddisfi i requisiti hardware minimi**. Ciò include stabilire se sono necessari ulteriori componenti hardware e verificare che i driver per l'hardware siano supportati da Windows Server Essentials. Per ulteriori informazioni, vedere [requisiti di sistema per Windows Server Essentials](../get-started/system-requirements.md).   

  
    > [!IMPORTANT]
    >  Prima di installare Windows Server Essentials in un computer preesistente, è consigliabile formattare completamente e ripartizionare i dischi rigidi del computer esistenti. La formattazione e ripartizionare i dischi rigidi, si rimuove la possibilità che i dischi rigidi restano partizioni nascoste.  
  
-   **Preparare la rete** per preparare la rete per installare Windows Server Essentials, eseguire le operazioni seguenti:  
    
  
    -   **Aggiornare il sistema operativo nei computer client** Windows Server Essentials supporta i seguenti sistemi operativi: Windows 8, Windows 7, Windows 10 e Macintosh OS X Lion o versioni successive. Questi sistemi operativi forniscono le funzionalità di sicurezza necessarie, affidabilità, prestazioni e funzionalità per la rete locale.  
  
    -   **Configurare il router** verificare che il router sia configurato come segue:  
  
        -   Il framework UPnP è abilitato nel router.  
  
        -   Il servizio Server Dynamic Host Configuration Protocol (DHCP) per la rete LAN può essere attivato o disattivato.  Windows Server Essentials garantisce che DHCP non è in esecuzione nel router sia il server? Quando DHCP è abilitato sul router, DHCP non è abilitato sul server durante l'installazione.  
  
        -   Hai un indirizzo IP per l'interfaccia esterna del router fornito dal provider di servizi Internet (ISP). È necessario configurare manualmente un indirizzo IP statico tramite la console di gestione del router o l'indirizzo IP può essere assegnato dinamicamente dal servizio Server DHCP dell'ISP.  
  
        -   Se la connessione Internet richiede un nome utente e password, chiamata anche-to-Point Protocol over Ethernet (PPPoE), queste impostazioni vengono configurate sul router, anche se il dispositivo supporta il framework UPnP.  
  
        -   Il router è connesso alla LAN e a Internet, sia acceso e funzioni correttamente.  
  
     Se il router non supporta il framework UPnP o se il router non può essere configurato durante l'installazione, è necessario configurarlo manualmente con le impostazioni per la rete. Assicurarsi che le porte seguenti siano aperte e che facciano riferimento all'indirizzo IP del Server di destinazione:  
  
    |Numero di porta|Applicazione|  
    |-----------------|-----------------|  
    |Porta 80|Traffico HTTP Web|  
    |Porta 443|Traffico Web HTTPS|  
  

-   **Leggere documentazione sulla versione di Windows Server Essentials**. Documentazione sulla versione contiene le informazioni più aggiornate che potrebbero essere fondamentali per installare e configurare Windows Server Essentials correttamente. Per visualizzare e stampare la documentazione relativa alla versione, vedere [rilasciare la documentazione per Windows Server Essentials](../get-started/release-notes.md).  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Installare Windows Server Essentials](Install-Windows-Server-Essentials.md)

