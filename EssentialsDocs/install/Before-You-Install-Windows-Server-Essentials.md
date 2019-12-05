---
title: Prima di installare Windows Server Essentials
description: Describes how to use Windows Server Essentials
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
ms.openlocfilehash: 4629c0ba04cc7ee617a2fc6b6a73a19b9e45ada8
ms.sourcegitcommit: 3d76683718ec6f38613f552f518ebfc6a5db5401
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/05/2019
ms.locfileid: "74829553"
---
# <a name="before-you-install-windows-server-essentials"></a>Prima di installare Windows Server Essentials

>Applies To: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_BeforeYouBegin"></a> Before you begin your installation of  Windows Server Essentials, perform the following tasks:  

-   **Verificare che il computer soddisfi i requisiti hardware minimi**. This includes determining if you need additional hardware and verifying that the drivers for your hardware are supported by  Windows Server Essentials. For more information, see [System Requirements for Windows Server Essentials](../get-started/system-requirements.md).   

> [!IMPORTANT]
> Before you install  Windows Server Essentials on a pre-existing computer, we recommend that you fully format and then repartition the hard disks of the pre-existing computer. La formattazione e la ripartizione delle unità disco rigido evitano che sui dischi rigidi restino delle partizioni nascoste.  

- **Prepare your network** To prepare your network to install  Windows Server Essentials, do the following:  


  - **Upgrade operating system on your client computers**  Windows Server Essentials supports the following operating systems:  Windows 8, Windows 7, Windows 10, and Macintosh OS X Lion or greater. Questi sistemi operativi forniscono le funzionalità di sicurezza e i livelli di affidabilità, prestazioni e funzionalità necessari per la rete locale.  

  - **Configurare il router** Verificare che il router sia configurato nel seguente modo:  

    -   Sul router è abilitato il framework UPnP.  

    -   Il servizio server DHCP (Dynamic Host Configuration Protocol) per la rete LAN può essere abilitato o disabilitato.  Windows Server Essentials ensures that DHCP is not running on both the server and the router ? when DHCP is enabled on the router, DHCP is not enabled on the server during installation.  

    -   Si dispone di un indirizzo IP per l'interfaccia esterna del router fornito dal provider di servizi Internet. L'indirizzo IP può essere assegnato dinamicamente dal servizio server DHCP presso il provider di servizi Internet oppure è necessario configurare manualmente un indirizzo IP statico usando la console di gestione del router.  

    -   Se la connessione Internet richiede un nome utente e una password, collettivamente indicati come Point-to-Point Protocol over Ethernet (PPPoE), queste impostazioni vengono configurate sul router, anche se il dispositivo supporta il framework UPnP.  

    -   Il router è connesso a una rete LAN e Internet, è accesso e funzionante.  

    Se il router non supporta il framework UPnP o se non può essere configurato durante l'installazione, è necessario configurarlo manualmente con le impostazioni della rete. Assicurarsi che le porte seguenti siano aperte e che facciano riferimento all'indirizzo IP del server di destinazione:  

  |Numero porta|Applicazione|  
  |-----------------|-----------------|  
  |Porta 80|Traffico Web HTTP|  
  |Porta 443|Traffico Web HTTPS|  


- **Read the  Windows Server Essentials release documentation**. The release documentation contains the latest information that may be critical to properly installing and configuring  Windows Server Essentials. To view or print release documentation, see [Release Documentation for Windows Server Essentials](../get-started/release-notes.md).  

## <a name="see-also"></a>Vedi anche  

-   [Installare Windows Server Essentials](Install-Windows-Server-Essentials.md)

