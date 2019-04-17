---
title: "Risolvere i problemi di connettività di accesso Web remoto in Windows Server Essentials"
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3642575-b3ee-4488-b654-5bf9d3b8c935
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: af4725fd3b1861c847434e3ed62c3da030689fb5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-remote-web-access-connectivity-in-windows-server-essentials"></a>Risolvere i problemi di connettività di accesso Web remoto in Windows Server Essentials
 
>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 In genere, Windows Server Essentials possono configurare automaticamente un router a banda larga, se il router è un dispositivo certificato UPnP e se l'impostazione UPnP è abilitata sul router.  
  
## <a name="possible-issues"></a>Possibili problemi  
 I seguenti problemi possono verificarsi con connettività di accesso Web remoto:  
  
-   Il router non è acceso o non è connesso alla rete.  
  
-   L'impostazione UPnP del router è disattivata.  
  
-   Il router potrebbe non supporta completamente lo standard UPnP. Microsoft gestisce un elenco dei router che funzionano con sistemi operativi Windows. Per visualizzare l'elenco dei router (compresi i router wireless) compatibili con Windows Server Essentials, visitare il [Windows Compatibility Center](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
## <a name="possible-fixes"></a>Possibili correzioni  
 Le azioni seguenti possono risolvere questi problemi:  
  
-   Verificare che il router sia acceso e funzioni correttamente.  
  
-   Assicurarsi che il server è connesso direttamente al router o che sia collegato a un commutatore connesso al router.  
  
-   Verificare che il dispositivo a banda larga che si connette al provider di servizi Internet (ISP) sia acceso, funzioni correttamente e che il router sia connesso al dispositivo a banda larga.  
  
-   Attivare l'impostazione UPnP del router. Connettersi alla pagina web di configurazione del router attivare l'impostazione UPnP. Per informazioni su come accedere al router e come attivare l'impostazione UPnP, vedere la documentazione del router. Dopo avere attivato l'impostazione UPnP, eseguire l'attiva nel Web guidata accesso remoto per configurare il router.  
  
-   Se il router non supporta completamente lo standard UPnP, non può essere configurato automaticamente. È necessario configurare manualmente il router o acquistare un router che supporta lo standard UPnP.  
  
     Per configurare manualmente il router, completare le attività seguenti:  
  
    -   Creare una prenotazione dell'indirizzo IP per il server Windows Server Essentials.  
  
         Prima di configurare manualmente il router per inoltrare le porte necessarie a Windows Server Essentials, è necessario impostare una prenotazione Dynamic Host Configuration Protocol (DHCP) per il server che esegue Windows Server Essentials sul router. Questo passaggio garantisce che l'indirizzo IP che verranno inoltrate le porte venga modificato.  
  
         Per informazioni su come configurare manualmente una prenotazione DHCP per il server sul router, vedere la documentazione del produttore s per il router.  
  
    -   Configurare il port forwarding sul router per le porte seguenti:  
  
        |Servizio o protocollo|Porta|  
        |-------------------------|----------|  
        |HTTP|TCP 80|  
        |HTTPS|TCP 443|  
  
     Per informazioni su come configurare manualmente il port forwarding sul router, vedere la documentazione del produttore s.  
  
     Una pagina di configurazione tipico router include una tabella simile al seguente.  
  
    > [!NOTE]
    >  In questa tabella, l'indirizzo IP del computer che esegue Windows Server Essentials è 192.168.0.100. È necessario determinare l'indirizzo IP del computer e sostituire tale indirizzo IP per l'indirizzo IP indicato nella tabella.  
  
    |Indirizzo IP|Protocollo (TCP/UDP)|Pianificazione|Filtro in ingresso|  
    |----------------|---------------------------|--------------|--------------------|  
    |192.168.0.100|TCP 80|Sempre|Consenti tutto|  
    |192.168.0.100|TCP 443|Sempre|Consenti tutto|  
  
     Dopo la configurazione manuale del router, eseguire l'attivazione in Web guidata accesso remoto, assicurandosi di selezionare il **Ignora configurazione router** opzione il **Introduzione** pagina.  
  
-   Se il router non supporta completamente lo standard UPnP, acquistare un nuovo router.  
  
> [!TIP]
>  Assicurarsi che nel router sia il firmware BIOS più recente installato. È spesso possibile aggiornare il firmware BIOS per il router dalla pagina web di configurazione per il router. Per ulteriori informazioni, vedere la documentazione del router. Dopo l'aggiornamento del router, eseguire la configurazione ovunque guidata di accesso.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Usare accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire accesso Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire l'accesso remoto via Internet](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Supporto per Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Supporto per Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

