---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: Pianificazione della capacità per i server federativi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 569bea74fe7750eaf2b410a552876e0862b1e24b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191087"
---
# <a name="planning-for-federation-server-capacity"></a>Pianificazione della capacità per i server federativi

Pianificazione della capacità per i server federativi aiuta a stimare:  
  
-   Quali fattori aumentare le dimensioni del database di configurazione di AD FS.  
  
-   I requisiti di hardware appropriato per ciascun server federativo.  
  
-   Il numero di server federativi da posizionare in ogni organizzazione.  
  
I server di federazione rilasciano token di sicurezza per gli utenti. Questi token vengono presentati a una relying party per l'utilizzo. I server di federazione rilasciano token di sicurezza dopo l'autenticazione di un utente o dopo la ricezione di un token di sicurezza emesso in precedenza da un servizio federativo del partner. Un token di sicurezza è richiesta da un servizio federativo quando gli utenti inizialmente l'accesso alle applicazioni federate o quando i relativi token di sicurezza scadere mentre accedono alle applicazioni federate.  
  
I server federativi sono progettati per contenere elevate\-disponibilità configurazioni di server farm che usano bilanciamento carico di rete Microsoft \(NLB\) tecnologia. I server federativi in una configurazione farm possono servire le richieste in modo indipendente, senza accedere alle eventuali componenti comuni di farm per ogni richiesta. Pertanto, non prevede un sovraccarico minimo in scalabilità orizzontale di una distribuzione di server federativo.  
  
**Indicazioni:**  
  
-   Per la missione\-critici o elevata\-le distribuzioni di disponibilità, si consiglia di creare una server farm federativa di piccole dimensioni in ogni organizzazione partner, con almeno due server federativi per farm, per garantire tolleranza di errore.  
  
-   Con la necessità di disponibilità elevata e la facilità di scalabilità orizzontale di server federativi, scalabilità orizzontale è il metodo consigliato per la gestione di un numero elevato di richieste al secondo per un determinato servizio federativo. Aumento delle prestazioni oltre la configurazione di base in questa guida è molto probabilmente non dà capacità significative miglioramenti di gestione.  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>Crescita e dimensione del database di configurazione di AD FS  
Le dimensioni del database di configurazione di ADFS viene in genere considerata piccoli e dimensioni del database non tendono a essere un fattore di primaria importanza in distribuzioni di AD FS.  Le dimensioni del database di configurazione di AD FS precisa possano dipendono in larga misura il numero di relazioni di trust e la relazione di trust associato\-metadati correlati, come le attestazioni, le regole di attestazione, monitoraggio e impostazioni configurate per ogni relazione di trust. Man mano che aumenta il numero di voci di relazione di trust nel database di configurazione, pertanto, non la necessità di ulteriore spazio su disco.  
  
Per informazioni sulla distribuzione aggiuntive relative al database di configurazione di ADFS, vedere [considerazioni sulla topologia di distribuzione di AD FS](AD-FS-Deployment-Topology-Considerations.md).  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>Requisiti di spazio di memoria, CPU e disco  
Fortunatamente, requisiti di spazio di memoria, CPU e disco per i server federativi sono modesti e non sono probabilmente un fattore determinante nelle decisioni di hardware. Per altre informazioni sui requisiti hardware, vedere [appendice a: Requisiti per ADFS](Appendix-A--Reviewing-AD-FS-Requirements.md).  
  
> [!NOTE]  
> Nei test che sono stati eseguiti dal team del prodotto AD FS usando una server farm federativa configurata con una versione di SQL Server dedicata per archiviare il database di configurazione di ADFS, il carico complessivo nel Server SQL tendenzialmente essere basso. In uno di test usando un quattro\-federazione\-server farm che è stato configurato per usare un singolo SQL Server, l'utilizzo della CPU superiore al 10% nonostante test che lo ha portato i server federativi all'utilizzo della destinazione.  
  
## <a name="bk_estimatefs"></a>Stimare il numero di server federativi per l'organizzazione  
Allo scopo di semplificare il processo per i server federativi di pianificazione dell'hardware, il team del prodotto AD FS sviluppato AD FS capacità pianificazione ridimensionamento foglio di calcolo. Questo foglio di calcolo di Excel include Calcolatrice\-come funzionalità che indirizzano i dati di utilizzo previsto che si forniscono informazioni sugli utenti nell'organizzazione e restituire un numero ottima consigliato di server federativi per l'ambiente di produzione di AD FS .  
  
> [!NOTE]  
> Il numero di server federativi che consiglia questo foglio di calcolo è basato sulle specifiche hardware e di rete che il team del prodotto AD FS usato durante i test. Pertanto, il numero di server federativi che consiglia il foglio di calcolo deve essere compresa all'interno di questo contesto.  Per altre informazioni sulle specifiche del usato durante i test, vedere l'argomento intitolato [pianificazione della capacità dei Server AD FS](Planning-for-AD-FS-Server-Capacity.md).  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>Usando la capacità di ADFS pianificazione foglio di calcolo di ridimensionamento  
Quando si usa questo foglio di calcolo, sarà necessario selezionare un valore \(entrambe **40%** , **60%** , oppure **80%** \) che meglio rappresenta la percentuale di numero totale di utenti che invierà le richieste di autenticazione per i server federativi durante i periodi di utilizzo.  
  
Quindi, sarà necessario selezionare un valore \(entrambe **minuto**, **15 minuti**, oppure **1 ora** \) che meglio rappresenta l'intervallo di tempo previsto il periodo di picco di utilizzo per ultimo. È ad esempio, potrebbe essere stimare 40% come valore per il numero totale di utenti che hanno eseguito l'accesso entro un periodo di 15 minuti, o che il 60% degli utenti l'accesso entro un periodo di 1 ora. Insieme, questi valori definiscono il profilo di carico di picco mediante il quale verranno calcolata le indicazioni di ridimensionamento.  
  
Successivamente, è necessario specificare il numero totale di utenti che richiederanno l'accesso single sign\-sull'accesso alle attestazioni destinazione\-applicazione compatibile con, basata sul fatto che gli utenti sono:  
  
-   La registrazione in Active Directory da un computer locale che è fisicamente connesso alla rete aziendale \(tramite l'autenticazione integrata di Windows\)  
  
-   La registrazione in Active Directory in modalità remota da un computer che non è fisicamente connesso alla rete aziendale \(tramite Windows integrato l'autenticazione o nome utente e password\)  
  
-   Da un'altra organizzazione e sono tentando di accedere alle attestazioni destinazione\-con riconoscimento dell'applicazione da un partner affidabile  
  
-   Da un provider di identità SAML 2.0 e sono tentando di accedere alle attestazioni destinazione\-applicazione compatibile con  
  
#### <a name="how-to-use-this-spreadsheet"></a>Come utilizzare questo foglio di calcolo  
È possibile usare la procedura seguente per ogni istanza di farm di server di federazione che si intende distribuire per determinare il numero consigliato di server federativi.  
  
1.  Scaricare e aprire il [AD FS capacità ridimensionamento foglio di calcolo pianificazione per Windows Server 2012 R2](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx) o nella [AD FS capacità ridimensionamento foglio di calcolo pianificazione per Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
  
2.  Nella cella a destra del **durante il periodo di utilizzo del sistema di picco, prevedere questa percentuale degli utenti per l'autenticazione** cella, fare clic sulla cella e quindi usare il trascinamento\-giù per selezionare la percentuale di utilizzo di sistema stimati il livello, ovvero **40%** , **60%** oppure **80%** per la distribuzione.  
  
3.  Nella cella a destra del **entro il periodo di tempo seguente** cella, fare clic sulla cella e quindi usare il trascinamento\-freccia giù per selezionare uno **1 minuto**, **a15minuti**, oppure **1 ora** per selezionare la durata del carico di picco.  
  
4.  Nella cella a destra del **invio numero stimato di applicazioni interne \(ad esempio SharePoint \(2007 o 2010\) o le applicazioni web compatibili con di attestazioni\)**  cella, digitare il numero di applicazioni interne, si userà nella propria organizzazione.  
  
5.  Nella cella a destra del **immettere il numero di applicazioni online stimato \(, ad esempio Office 365 Exchange Online, SharePoint Online o Lync Online\)**  cella, digitare il numero di applicazioni online o servizi che verranno usati nell'organizzazione.  
  
6.  Sotto la cella intitolata **numero di utenti**, digitare un numero in ogni riga che si applica a uno scenario di applicazione di esempio agli utenti sarà necessario Single sign-\-sull'accesso a. Questa colonna deve contenere il numero di utenti definiti, non gli utenti di picco al secondo. Se i tentativi di accesso all'applicazione devono prima passare attraverso la pagina di individuazione dell'area di autenticazione, digitare **Y**. Se si è certi della selezione, digitare **Y**.  
  
7.  Esaminare le informazioni seguenti valori forniti consigliate:  
  
    1.  Per il numero totale di server federativi consigliati, vedere la cella inferiore destra viene evidenziata in grigio.  
  
    2.  Per il numero di server consigliato per ogni scenario di applicazione di esempio, vedere la cella nella riga che viene evidenziata in grigio.  
  
> [!NOTE]  
> Il valore viene calcolato automaticamente nella cella a destra della cella intitolata **numero totale di server federativi consigliato** nella parte inferiore del foglio di calcolo contiene una formula che verrà aggiunto un ulteriore buffer 20% per il totale Somma di tutti i valori in ognuna delle singole righe che la precede. La formula aggiunta per il **numero totale di server federativi consigliato** cella compilazioni di questo buffer totale numero consigliato di server federativi distribuiti per renderla molto improbabile che raggiunga mai il carico complessivo della farm di intermedie saturazione.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
