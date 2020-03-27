---
title: Accesso remoto Always On pianificazione della migrazione VPN
description: La migrazione da DirectAccess a Always On VPN richiede una pianificazione corretta per determinare le fasi di migrazione, che consente di identificare eventuali problemi prima che influiscano sull'intera organizzazione.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 05/29/2018
ms.openlocfilehash: 80a7a8b3ee13a9d9cc99b81ab917f6443147565f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314946"
---
# <a name="plan-the-directaccess-to-always-on-vpn-migration"></a>Pianificare la migrazione di DirectAccess a VPN Always On

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows 10

&#171;[ **Precedente:** Panoramica di DirectAccess per la migrazione VPN always on](da-always-on-migration-overview.md)<br>
&#187;Passaggio [ **successivo:** eseguire la migrazione a always on VPN e rimuovere le autorizzazioni DirectAccess](da-always-on-migration-deploy.md)


La migrazione da DirectAccess a Always On VPN richiede una pianificazione corretta per determinare le fasi di migrazione, che consente di identificare eventuali problemi prima che influiscano sull'intera organizzazione. L'obiettivo principale della migrazione è consentire agli utenti di mantenere la connettività remota all'ufficio durante tutto il processo. Se le attività vengono eseguite in modo non corretto, è possibile che si verifichi un race condition, lasciando gli utenti remoti senza alcun modo per accedere alle risorse aziendali. È pertanto consigliabile eseguire una migrazione side-by-side pianificata da DirectAccess a Always On VPN. Per informazioni dettagliate, vedere la sezione [Always on distribuzione della migrazione VPN](da-always-on-migration-deploy.md) .

La sezione descrive i vantaggi della separazione degli utenti per la migrazione, le considerazioni sulla configurazione standard e Always On miglioramenti delle funzionalità VPN. La fase di pianificazione della migrazione include:

1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

## <a name="build-migration-rings"></a>Anelli di migrazione compilazione
Gli anelli di migrazione vengono usati per dividere l'attività di migrazione del client VPN Always On in più fasi. Quando si arriva all'ultima fase, il processo dovrebbe essere ben testato e coerente.

In questa sezione viene fornito un approccio per separare gli utenti nelle fasi di migrazione e quindi gestire tali fasi. Indipendentemente dal metodo di separazione della fase utente scelto, mantenere un gruppo di utenti VPN singolo per una gestione più semplice al termine della migrazione.

>[!NOTE] 
>La _fase_ della parola non è destinata a indicare che si tratta di un processo lungo. Che si tratti di ogni fase in un paio di giorni o di un paio di mesi, Microsoft consiglia di sfruttare la migrazione side-by-side e di usare un approccio graduale.

### <a name="benefits-of-dividing-the-migration-effort-into-multiple-phases"></a>Vantaggi della suddivisione del lavoro di migrazione in più fasi

-   **Protezione da interruzioni di massa.** Suddividendo una migrazione in fasi, il numero di persone che un problema generato dalla migrazione può interessare è molto più piccolo.

-   **Miglioramento del processo o della comunicazione dai commenti.** Idealmente, gli utenti non hanno neanche notato che si è verificata la migrazione. Tuttavia, se la loro esperienza non è ottimale, il feedback di questi usi ti offre la possibilità di migliorare la pianificazione ed evitare problemi in futuro.

### <a name="tips-for-building-your-migration-ring"></a>Suggerimenti per la creazione di un anello di migrazione

-   **Identificare gli utenti remoti.** Per iniziare, separare gli utenti in due bucket: quelli che spesso entrano in ufficio e quelli che non lo hanno fatto. Il processo di migrazione è identico per entrambi i gruppi, ma è probabile che la ricezione dell'aggiornamento da parte dei client remoti sia maggiore rispetto a quelle che si connettono con maggiore frequenza. Ogni fase di migrazione, idealmente, deve includere i membri di ogni bucket.

-  **Assegnare priorità agli utenti.** La leadership e altri utenti a elevato effetto sono in genere tra gli ultimi utenti migrati. Quando si classificano in ordine di priorità gli utenti, tuttavia, considerare l'effetto della produttività aziendale se la migrazione del computer client ha avuto esito negativo. Se, ad esempio, si dispone di un rating da 1 a 3, con 1 significa che il dipendente non è in grado di funzionare e 3 significa che non si è verificata un'interruzione immediata del lavoro, un analista aziendale che usa solo app line-of-business (LOB) interne in modalità remota sarebbe 1, mentre un venditore usa un cloud l'app sarà un 3.

-   **Eseguire la migrazione di ogni reparto o business unit in più fasi.** Microsoft consiglia vivamente di non eseguire la migrazione di un intero reparto nello stesso momento. Se si verifica un problema, non è necessario che venga ostacolato il lavoro remoto per l'intero reparto. Migrare invece ogni reparto o business unit in almeno due fasi.

-   **Aumentare gradualmente i conteggi degli utenti.** La maggior parte degli scenari di migrazione tipici inizia con i membri dell'organizzazione IT e quindi passa agli utenti aziendali seguiti dalla leadership e da altri utenti a elevato effetto. Ogni fase di migrazione prevede in genere progressivamente più persone. Ad esempio, la prima fase può includere dieci utenti e il gruppo finale può includere 5.000 utenti. Per semplificare la distribuzione, creare un singolo gruppo di sicurezza utenti VPN e aggiungervi gli utenti all'arrivo della fase. In questo modo, si finisce con un singolo gruppo di utenti VPN a cui è possibile aggiungere membri in futuro.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="next-step"></a>Passaggio successivo

|Se vuoi...  |Vedere...  |
|---------|---------|
|Avviare la migrazione alla VPN Always On     |[Eseguire la migrazione a always on VPN e rimuovere le autorizzazioni di DirectAccess](da-always-on-migration-deploy.md). La migrazione da DirectAccess a Always On VPN richiede un processo specifico per la migrazione dei client, che consente di ridurre al minimo le condizioni di Race che derivano dall'esecuzione di passaggi di migrazione non in ordine.         |
|Informazioni sulle funzionalità di Always On VPN e DirectAccess    |[Confronto tra le funzionalità di always on VPN e DirectAccess](../vpn/vpn-map-da.md). Nelle versioni precedenti dell'architettura VPN di Windows, le limitazioni della piattaforma rendevano difficile fornire le funzionalità critiche necessarie per sostituire DirectAccess, ad esempio le connessioni automatiche avviate prima dell'accesso degli utenti. Always On VPN, tuttavia, ha attenuato la maggior parte di tali limitazioni o ha espanso la funzionalità VPN oltre le funzionalità di DirectAccess.         |



---