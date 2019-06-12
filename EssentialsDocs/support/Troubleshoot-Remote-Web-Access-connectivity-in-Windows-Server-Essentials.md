---
title: Risolvere i problemi di connettività di Accesso Web remoto in Windows Server Essentials
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
ms.openlocfilehash: fda0b5a227fe25b4e8780915089e97ee48620383
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432433"
---
# <a name="troubleshoot-remote-web-access-connectivity-in-windows-server-essentials"></a>Risolvere i problemi di connettività di Accesso Web remoto in Windows Server Essentials
 
>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 In genere, Windows Server Essentials può configurare automaticamente un router a banda larga, se il router è un dispositivo certificato UPnP e se l'impostazione UPnP è abilitata sul router.  
  
## <a name="possible-issues"></a>Possibili problemi  
 Di seguito sono elencati i problemi che possono verificarsi con la connettività di Accesso Web remoto:  
  
-   Il router non è acceso o non è connesso alla rete.  
  
-   L'impostazione UPnP per il router è disattivata.  
  
-   Il router potrebbe non supportare completamente lo standard UPnP. Microsoft gestisce un elenco dei router che funzionano con i sistemi operativi Windows. Per visualizzare l'elenco dei router (compresi i router wireless) compatibili con Windows Server Essentials, visitare il [Centro compatibilità Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
## <a name="possible-fixes"></a>Possibili correzioni  
 Le azioni seguenti possono risolvere questi problemi:  
  
- Verificare che il router sia acceso e funzioni correttamente.  
  
- Verificare che il server sia connesso direttamente al router o che sia collegato a un commutatore connesso al router.  
  
- Verificare che il dispositivo a banda larga che si connette al provider di servizi Internet (ISP) sia acceso, funzioni correttamente e che il router sia connesso al dispositivo a banda larga.  
  
- Attivare l'impostazione UPnP del router. Connettersi alla pagina web di configurazione del router per attivare l'impostazione UPnP. Per informazioni su come accedere al router e come attivare l'impostazione UPnP, vedere la documentazione del router. Una volta abilitata l'impostazione UPnP, eseguire attiva sul Web guidata accesso remoto per configurare il router.  
  
- Se il router non supporta completamente lo standard UPnP, non può essere configurato automaticamente. È necessario configurare manualmente il router o acquistare un router che supporti lo standard UPnP.  
  
   Per configurare manualmente il router, completare le attività seguenti:  
  
  - Creare una prenotazione dell'indirizzo IP per il server Windows Server Essentials.  
  
     Prima di configurare manualmente il router per inoltrare le porte necessarie a Windows Server Essentials, è necessario configurare una prenotazione DHCP (Dynamic Host Configuration Protocol) per il server che esegue Windows Server Essentials sul router. Questo passaggio garantisce che l'indirizzo IP a cui verranno inoltrate le porte non venga modificato.  
  
     Per informazioni su come configurare manualmente una prenotazione DHCP per il server sul router, vedere la documentazione del produttore del router.  
  
  - Configurare il port forwarding sul router per le porte seguenti:  
  
    |Servizio o protocollo|Port|  
    |-------------------------|----------|  
    |HTTP|TCP 80|  
    |HTTPS|TCP 443|  
  
    Per informazioni su come impostare manualmente il port forwarding sul router, vedere la documentazione del produttore.  
  
    Una tipica pagina di configurazione del router include una tabella simile alla seguente.  
  
  > [!NOTE]
  >  In questa tabella l'indirizzo IP del computer che esegue Windows Server Essentials è 192.168.0.100. È necessario determinare l'indirizzo IP del computer e sostituire tale indirizzo IP con l'indirizzo IP indicato nella tabella.  
  
  |L'indirizzo IP|Protocollo (TCP/UDP)|Pianificazione|Filtro in ingresso|  
  |----------------|---------------------------|--------------|--------------------|  
  |192.168.0.100|TCP 80|Sempre|Consenti tutto|  
  |192.168.0.100|TCP 443|Sempre|Consenti tutto|  
  
   Dopo la configurazione manuale del router, eseguire l'attivazione sul Web guidata accesso remoto, assicurandosi di selezionare il **Ignora configurazione router** opzione il **introduttiva** pagina.  
  
- Acquistare un nuovo router, se il router non supporta completamente lo standard UPnP.  
  
> [!TIP]
>  Verificare che nel router sia installato il firmware BIOS più recente. È spesso possibile aggiornare il firmware BIOS per il router dalla pagina Web di configurazione del router. Per altre informazioni, vedere la documentazione del router. Dopo l'aggiornamento del router, eseguire la Configurazione guidata di Accesso remoto via Internet.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Usare accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire accesso Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire l'accesso remoto via Internet](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Supportare Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Supportare Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

