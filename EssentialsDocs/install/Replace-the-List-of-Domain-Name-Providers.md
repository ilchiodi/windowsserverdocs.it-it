---
title: Sostituzione dell'elenco dei provider dei nomi di dominio
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 104d0412-2d77-4cd4-99f7-65a885522850
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ebaef0f88456f61fa229c9a18ee8014987fe7fa7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820022"
---
# <a name="replace-the-list-of-domain-name-providers"></a>Sostituzione dell'elenco dei provider dei nomi di dominio

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile sostituire l'elenco dei provider dei nomi di dominio visualizzato nella procedura guidata per l'impostazione dei nomi di dominio completando le seguenti attività:  
  

-   [Creare il riferimento del servizio file](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [Aggiungere una voce nel Registro di sistema nel computer di riferimento](Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

-   [Creare il riferimento del servizio file](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [Aggiungere una voce nel Registro di sistema nel computer di riferimento](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

  
###  <a name="BKMK_ReferralFiles"></a> Creare il riferimento del servizio file  
 Lo strumento di amministrazione di Referral Service crea un gruppo di file che vengono utilizzati per definire l'elenco dei provider dei nomi di dominio visualizzati nella procedura guidata per l'impostazione dei nomi di dominio. Per ogni regione a livello mondiale, viene creato un file in formato XML contenente le informazioni per i provider dei nomi di dominio specificati nello strumento. I file creati dallo strumento devono essere collocati in una cartella accessibile mediante una connessione protetta (HTTPS) gestita dall'utente in Internet.  
  
##### <a name="to-create-the-referral-files"></a>Per creare i file di Referral Service  
  
1.  Aprire lo strumento di amministrazione di Referral Service.  
  
2.  Fai clic su **Aggiungi**.  
  
3.  Nella finestra di dialogo per l'aggiunta di un provider dei nomi di dominio, immettere il nome del provider.  
  
4.  Aggiungere i domini di livello superiore supportati dal provider dei nomi di dominio. A tale scopo, fare clic su **Aggiungi**, immettere l'identificatore dei domini di livello superiore, quindi selezionare le regioni supportate. È possibile selezionare **Tutte le aree**.  
  
5.  Immettere la descrizione del provider dei nomi di dominio.  
  
6.  Aggiungere gli URL per tutti i siti Web associati al provider dei nomi di dominio.  
  
7.  Se per il provider dei nomi di dominio è disponibile un logo, aggiungere quest'ultimo facendo clic su **Cambia logo**.  
  
8.  Fare clic su **Salva**.  
  
9. Ripetere le operazioni dei passaggi da 2 a 8 per ogni provider dei nomi di dominio che si desidera inserire nell'elenco della procedura guidata.  
  
10. Una volta aggiunti tutti i provider desiderati, scegliere la cartella in cui verranno collocati i file di Referral Service. Quando si sceglie la cartella, tenere presente che i file di Referral Service devono essere accessibili mediante una connessione HTTPS.  
  
11. Fare clic su **Genera file nel File System**.  
  
###  <a name="BKMK_AddRegistry"></a> Aggiungere una voce nel Registro di sistema nel computer di riferimento  
 È necessario aggiungere una voce al Registro di sistema per specificare la posizione in cui il sistema operativo potrà trovare i file di Referral Service.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Per aggiungere una chiave al Registro di sistema  
  
1.  Nel computer di riferimento fare clic su **Start**, immettere **regedit**e quindi premere **INVIO**.  
  
2.  Nel riquadro sinistro, espandere **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, quindi **Windows Server**, **Gestione dominio** e infine **Provider**.  
  
3.  Fare clic con il pulsante destro del mouse sulla chiave **E423C85D-6B1F-4583-95E0-449D8263BAC4**, quindi fare clic su **Valore stringa**.  
  
4.  Digitare **ReferralServerHttpsUri** come nome della stringa e quindi premere **INVIO**.  
  
5.  Fare clic con il pulsante destro del mouse sulla nuova stringa **ReferralServerHttpsUri** nel riquadro destro, quindi fare clic su **Modifica**.  
  

6.  Digitare l'URL HTTPS usato per l'accesso ai file di riferimento creati in [Create the referral service files](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles), quindi fare clic su **OK**.  

6.  Digitare l'URL HTTPS usato per l'accesso ai file di riferimento creati in [Create the referral service files](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles), quindi fare clic su **OK**.  

  
    > [!IMPORTANT]
    >  Una barra (/) è necessaria alla fine dell'URL.  
  
###  <a name="BKMK_ReplaceDomainNameProviders"></a> Problemi di stato del dominio nome  
 Se un partner aggiunge dei provider di nomi di dominio e utilizza una application programming interface (API) in Windows Server Essentials SDK per impostare gli stati Unknown, Failed e CertificateRequestNotSubmitted per il certificato, il cliente riceve un'implementazione non corretta risultato messaggio e la configurazione. Questo perché i casi sono gestiti da eccezioni e non restituiscono uno stato.  
  
 I seguenti stati di dominio si riferiscono a operazioni non riuscite e devono essere segnalati come errori:  
  
-   Failed  
  
-   PendingCustomerInterventionRequired  
  
-   PurchaseFailed  
  
-   DomainNotFound  
  
-   InRenewalCustomerInterventionRequired  
  
-   RenewalFailed  
  
 I seguenti stati di dominio si riferiscono a operazioni riuscite e devono essere segnalati come completati:  
  
-   Pronto  
  
-   Pending  
  
-   InRenewal  
  
## <a name="see-also"></a>Vedere anche  

 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](Testing-the-Customer-Experience.md)

 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](../install/Testing-the-Customer-Experience.md)

