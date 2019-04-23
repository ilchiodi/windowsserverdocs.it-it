---
title: Remote Access VPN Always On la pianificazione della migrazione
description: Migrazione da DirectAccess a VPN Always On richiede una pianificazione appropriata determinare le fasi di migrazione, che consente di identificare eventuali problemi prima che possano danneggiare l'intera organizzazione.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: 494dc7916b505991c22b07bec738c2300d660ec1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835672"
---
# <a name="plan-the-directaccess-to-always-on-vpn-migration"></a>Pianificare la migrazione di DirectAccess a VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

&#171;[ **Precedente:** Panoramica di DirectAccess per la migrazione di VPN Always On](da-always-on-migration-overview.md)<br>
&#187; [**Next:** Eseguire la migrazione a VPN Always On e rimuovere le autorizzazioni di DirectAccess](da-always-on-migration-deploy.md)


Migrazione da DirectAccess a VPN Always On richiede una pianificazione appropriata determinare le fasi di migrazione, che consente di identificare eventuali problemi prima che possano danneggiare l'intera organizzazione. L'obiettivo principale della migrazione è agli utenti di mantenere la connettività remota all'ufficio nel corso del processo. Se si eseguono attività senza rispettare l'ordine, può verificarsi una race condition, lasciando agli utenti remoti con alcun modo per accedere alle risorse aziendali. Di conseguenza, Microsoft consiglia di eseguire una migrazione pianificata, side-by-side da DirectAccess a VPN Always On. Per informazioni dettagliate, vedere la [distribuzione di migrazione VPN Always On](da-always-on-migration-deploy.md) sezione.

La sezione descrive i vantaggi della separazione tra gli utenti per la migrazione, considerazioni sulla configurazione standard e miglioramenti della funzionalità VPN Always On. La fase di pianificazione della migrazione include:

1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

## <a name="build-migration-rings"></a>Creare gli anelli di migrazione
Gli anelli di migrazione vengono utilizzati per suddividere lo sforzo di migrazione di client VPN Always On in più fasi. Una volta che ottenere l'ultima fase, il processo deve essere ben testate e coerente.

Questa sezione presenta un approccio per la separazione degli utenti in fasi di migrazione e quindi la gestione di queste fasi. Indipendentemente dal metodo di separazione fase utente che prescelto, mantenere un singolo gruppo di utenti VPN per semplificare la gestione una volta completata la migrazione.

>[!NOTE] 
>La parola _fase_ non può indicare che si tratta di un processo lungo. Se sposta all'interno di ogni fase in un paio di mesi o un paio di giorni, Microsoft consiglia di sfruttare i vantaggi della migrazione side-by-side e adottare un approccio graduale.

### <a name="benefits-of-dividing-the-migration-effort-into-multiple-phases"></a>Vantaggi di dividere lo sforzo di migrazione in più fasi

-   **Protezione di un'interruzione di massa.** Dividendo una migrazione in fasi, il numero di un problema di migrazione-generato può influire sulle persone è molto più piccolo.

-   **Analisi utilizzo software in corso o le comunicazioni dai commenti e suggerimenti.** In teoria, gli utenti sono stati non anche notare che si è verificato durante la migrazione. Tuttavia, se l'esperienza è stata non ottimale, commenti e suggerimenti da tali usi offre un'opportunità per migliorare la pianificazione della ed evitare i problemi in futuro.

### <a name="tips-for-building-your-migration-ring"></a>Suggerimenti per compilare l'anello di migrazione

-   **Identificare gli utenti remoti.** Per iniziare, separare gli utenti in due bucket: coloro che spesso entrano in ufficio e chi non lo sono. Il processo di migrazione è lo stesso per entrambi i gruppi, ma è probabile che richiedono più tempo per i client remoti ricevere l'aggiornamento più per quelle che si connettono più frequentemente. Ogni fase della migrazione, in teoria, deve includere i membri di ogni bucket.

-  **Definire le priorità degli utenti.** Leadership e ad altri utenti a impatto elevato vengono in genere tra l'ultimo utenti migrati. Quando l'assegnazione delle priorità degli utenti, tuttavia, prendere in considerazione loro impatto sulla produttività aziendale se dovesse avere esito negativo della migrazione dei computer client. Ad esempio, se si disponeva di una classificazione da 1 a 3, con 1 che significa che il dipendente non sarebbe in grado di lavoro e 3 alcuna interruzione di un lavoro immediato, vale a dire usando solo app interne line-of-business (LOB) in modalità remota un business analyst sarebbe a 1, mentre un agente utilizzando un cloud  App sarebbe un 3.

-   **Eseguire la migrazione di ogni reparto o una business unit in più fasi.** Microsoft consiglia vivamente di non migrare un intero reparto nello stesso momento. Se deve verificarsi un problema, non serve a ostacolare lavoro remoti per l'intero reparto. In alternativa, eseguire la migrazione di ogni reparto o una business unit in almeno due fasi.

-   **Aumentare gradualmente il numero degli utenti.** Più comuni scenari di migrazione iniziano con i membri dell'organizzazione IT e quindi passare agli utenti aziendali, seguiti dalla leadership e ad altri utenti a impatto elevato. Ogni fase della migrazione è in genere implica progressivamente più persone. Ad esempio, la prima fase potrebbe includere dieci utenti e il gruppo finale può includere 5.000 utenti. Per semplificare la distribuzione, creare un singolo gruppo di sicurezza agli utenti di VPN e aggiungervi utenti man mano che arrivano le relative fasi. In questo modo, si finisce con un singolo gruppo di utenti VPN a cui è possibile aggiungere membri in futuro.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="next-step"></a>Passaggio successivo

|Se si vuole...  |Vedere quindi...  |
|---------|---------|
|Avviare la migrazione a VPN Always On     |[Eseguire la migrazione a VPN Always On e rimuovere le autorizzazioni DirectAccess](da-always-on-migration-deploy.md). Migrazione da DirectAccess a VPN Always On richiede un processo specifico per la migrazione dei client, che consente di ridurre al minimo le race condition che derivano dall'esecuzione di passaggi della migrazione non in ordine.         |
|Informazioni sulle funzionalità di DirectAccess e VPN Always On    |[Confronto di DirectAccess o VPN Always On delle funzionalità](../vpn/vpn-map-da.md). Nelle versioni precedenti dell'architettura VPN di Windows, delle limitazioni delle piattaforme rendeva difficile fornire la funzionalità critiche necessarie per sostituire DirectAccess (ad esempio, le connessioni avviate prima che gli utenti accedono automatiche). VPN Always On, tuttavia, ha mitigato la maggior parte di queste limitazioni o espandere le funzionalità VPN oltre le funzionalità di DirectAccess.         |



---