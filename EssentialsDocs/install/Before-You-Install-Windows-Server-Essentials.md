---
title: Prima di installare Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d0893bd-e2b7-4494-9537-02b1cbbcd57a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7268ebbfffd034780635e693cd6aa6380f30dd91
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310101"
---
# <a name="before-you-install-windows-server-essentials"></a>Prima di installare Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="before-you-begin-your-installation-of--windows-server-essentials-perform-the-following-tasks"></a><a name="BKMK_BeforeYouBegin"></a>Prima di iniziare l'installazione di Windows Server Essentials, eseguire le attività seguenti:  

-   **Verificare che il computer soddisfi i requisiti hardware minimi**. Ciò include determinare se sono necessari altri componenti hardware e verificare che i driver per l'hardware siano supportati da Windows Server Essentials. Per ulteriori informazioni, vedere [requisiti di sistema per Windows Server Essentials](../get-started/system-requirements.md).   

> [!IMPORTANT]
> Prima di installare Windows Server Essentials in un computer preesistente, è consigliabile formattare completamente e quindi ripartizionare i dischi rigidi del computer preesistente. La formattazione e la ripartizione delle unità disco rigido evitano che sui dischi rigidi restino delle partizioni nascoste.  

- **Preparare la rete** Per preparare la rete per l'installazione di Windows Server Essentials, eseguire le operazioni seguenti:  


  - **Aggiornare il sistema operativo nei computer client**  Windows Server Essentials supporta i sistemi operativi seguenti: Windows 8, Windows 7, Windows 10 e Macintosh OS X Lion o versioni successive. Questi sistemi operativi forniscono le funzionalità di sicurezza e i livelli di affidabilità, prestazioni e funzionalità necessari per la rete locale.  

  - **Configurare il router** Verificare che il router sia configurato nel seguente modo:  

    -   Sul router è abilitato il framework UPnP.  

    -   Il servizio server DHCP (Dynamic Host Configuration Protocol) per la rete LAN può essere abilitato o disabilitato.  Windows Server Essentials garantisce che DHCP non sia in esecuzione sul server e sul router? Quando DHCP è abilitato sul router, DHCP non è abilitato sul server durante l'installazione.  

    -   Si dispone di un indirizzo IP per l'interfaccia esterna del router fornito dal provider di servizi Internet. L'indirizzo IP può essere assegnato dinamicamente dal servizio server DHCP presso il provider di servizi Internet oppure è necessario configurare manualmente un indirizzo IP statico usando la console di gestione del router.  

    -   Se la connessione Internet richiede un nome utente e una password, collettivamente indicati come Point-to-Point Protocol over Ethernet (PPPoE), queste impostazioni vengono configurate sul router, anche se il dispositivo supporta il framework UPnP.  

    -   Il router è connesso a una rete LAN e Internet, è accesso e funzionante.  

    Se il router non supporta il framework UPnP o se non può essere configurato durante l'installazione, è necessario configurarlo manualmente con le impostazioni della rete. Assicurarsi che le porte seguenti siano aperte e che facciano riferimento all'indirizzo IP del server di destinazione:  

  |Numero porta|Applicazione|  
  |-----------------|-----------------|  
  |Porta 80|Traffico Web HTTP|  
  |Porta 443|Traffico Web HTTPS|  


- **Leggere la documentazione sulla versione di Windows Server Essentials**. La documentazione relativa alla versione contiene le informazioni più aggiornate che potrebbero essere fondamentali per installare e configurare correttamente Windows Server Essentials. Per visualizzare o stampare la documentazione relativa alla versione, vedere [la documentazione relativa alla versione per Windows Server Essentials](../get-started/release-notes.md).  

## <a name="see-also"></a>Vedere anche  

-   [Installare Windows Server Essentials](Install-Windows-Server-Essentials.md)

