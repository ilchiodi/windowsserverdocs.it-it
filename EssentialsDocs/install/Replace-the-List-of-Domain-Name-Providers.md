---
title: Sostituire l'elenco dei provider di nomi di dominio
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="replace-the-list-of-domain-name-providers"></a>Sostituire l'elenco dei provider di nomi di dominio

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile sostituire l'elenco dei provider di nomi di dominio che viene visualizzato nella configurazione guidata nome dominio completando le attività seguenti:  
  

-   [Creare la segnalazione dei file del servizio](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [Aggiungere una voce al Registro di sistema nel computer di riferimento](Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

-   [Creare la segnalazione dei file del servizio](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [Aggiungere una voce al Registro di sistema nel computer di riferimento](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

  
###  <a name="BKMK_ReferralFiles"></a>Creare la segnalazione dei file del servizio  
 Lo strumento di amministrazione di Referral Service crea un set di file che vengono utilizzati per definire l'elenco dei provider di nomi di dominio che vengono visualizzati nella configurazione guidata nome dominio. Un file in formato XML viene creato per ogni regione a livello mondiale e contiene informazioni per i provider di nomi di dominio specificati nello strumento. I file creati dallo strumento devono trovarsi in una cartella che è possibile accedere tramite una connessione protetta (HTTPS) gestita dall'utente in Internet.  
  
##### <a name="to-create-the-referral-files"></a>Per creare i file di Referral Service  
  
1.  Aprire lo strumento di amministrazione di Referral Service.  
  
2.  Fare clic su **aggiungere**.  
  
3.  Nella finestra Aggiungi una finestra di dialogo Provider di nomi di dominio, immettere il nome del provider dei nomi di dominio.  
  
4.  Aggiungere i domini di livello superiore supportati dal provider dei nomi di dominio. A tale scopo, fare clic su **Aggiungi**, immettere l'identificatore di dominio di primo livello e quindi selezionare le regioni supportate. È possibile selezionare **tutte le aree geografiche**.  
  
5.  Immettere la descrizione del provider dei nomi di dominio.  
  
6.  Aggiungere gli URL per tutti i siti Web che sono associati al provider dei nomi di dominio.  
  
7.  Se è disponibile per il provider di nomi di dominio un logo, aggiungere il logo facendo **cambia Logo**.  
  
8.  Fare clic su **salvare**.  
  
9. Ripetere i passaggi da 2 a 8 per ogni provider di nomi di dominio che si desidera elencare nella procedura guidata.  
  
10. Dopo aver aggiunto tutti i provider di nomi di dominio, scegliere la cartella in cui i file di Referral Service utilizzeranno un percorso. Tenere presenti quando si sceglie una cartella che i file di Referral Service devono essere accessibili mediante un collegamento HTTPS.  
  
11. Fare clic su **genera file nel File System**.  
  
###  <a name="BKMK_AddRegistry"></a>Aggiungere una voce al Registro di sistema nel computer di riferimento  
 È necessario aggiungere una voce del Registro di sistema per specificare dove il sistema operativo può trovare la segnalazione dei file del servizio.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Per aggiungere una chiave al Registro di sistema  
  
1.  Nel computer di riferimento, fare clic su **Start**, immettere **regedit**, quindi premere **invio**.  
  
2.  Nel riquadro sinistro, espandere **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, espandere **Microsoft**, espandere **Windows Server**, espandere **Gestione dominio**, quindi espandere **provider**.  
  
3.  Fare doppio clic su di **E423C85D-6B1F-4583-95E0-449D8263BAC4** chiave, quindi fare clic su **valore stringa**.  
  
4.  Tipo **ReferralServerHttpsUri** per il nome della stringa e quindi premere **invio**.  
  
5.  Fare doppio clic su nuovo **ReferralServerHttpsUri** stringa nel riquadro di destra, quindi fare clic su **modifica**.  
  

6.  Digitare l'URL HTTPS utilizzato per accedere ai file di riferimento che hai creato in [creare la segnalazione dei file del servizio](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles), quindi fare clic su **OK**.  

6.  Digitare l'URL HTTPS utilizzato per accedere ai file di riferimento che hai creato in [creare la segnalazione dei file del servizio](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles), quindi fare clic su **OK**.  

  
    > [!IMPORTANT]
    >  Una barra (/) è necessaria alla fine dell'URL.  
  
###  <a name="BKMK_ReplaceDomainNameProviders"></a>Avvisi di stato per nome dominio  
 Se un partner aggiunge dei provider di nomi di dominio e utilizza una application programming interface (API) in Windows Server Essentials SDK per impostare gli stati Unknown, Failed e CertificateRequestNotSubmitted per il certificato, il cliente riceve un messaggio non corretto e il risultato di configurazione. In questo modo i casi sono gestiti da eccezioni e non restituiscono uno stato.  
  
 I seguenti stati di dominio non riuscite e devono essere segnalati come un errore:  
  
-   Non è riuscito  
  
-   PendingCustomerInterventionRequired  
  
-   PurchaseFailed  
  
-   DomainNotFound  
  
-   InRenewalCustomerInterventionRequired  
  
-   RenewalFailed  
  
 I seguenti stati di dominio hanno esito positivo e devono essere segnalati come completati:  
  
-   Pronto  
  
-   In sospeso  
  
-   InRenewal  
  
## <a name="see-also"></a>Vedere anche  

 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)

 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](../install/Testing-the-Customer-Experience.md)

