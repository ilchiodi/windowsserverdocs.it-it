---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: "Pianificazione della capacità del Server federativo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 618dc9419be965dedaaf7dc946da436a5001f121
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-federation-server-capacity"></a>Pianificazione della capacità del Server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pianificazione della capacità per i server federativi consente di stimare:  
  
-   Fattori che aumentare le dimensioni del database di configurazione di ADFS.  
  
-   I requisiti hardware appropriato per ciascun server federativo.  
  
-   Il numero di server federativi per inserire in ogni organizzazione.  
  
I server federativi rilasciare token di sicurezza per gli utenti. I token vengono presentati a una relying party per l'utilizzo. I server federativi rilasciare token di sicurezza dopo l'autenticazione di un utente o dopo la ricezione di un token di sicurezza emesso in precedenza da un servizio federativo del partner. Quando gli utenti eseguire l'accesso alle applicazioni federate o quando i token di sicurezza scadenza mentre accedono alle applicazioni federate, da un servizio federativo è richiesto un token di sicurezza.  
  
I server federativi sono progettati per supportare configurazioni di farm di server a disponibilità elevata che utilizzano la tecnologia di bilanciamento carico di rete Microsoft \(NLB\). I server federativi in una configurazione farm possono soddisfare le richieste in modo indipendente, senza accedere a qualsiasi comuni componenti della farm per ogni richiesta. Pertanto, non esiste un overhead minimo coinvolti nella distribuzione di un server federativo la scalabilità.  
  
**Consigli:**  
  
-   Per mission\ critici o le distribuzioni a disponibilità elevata, ti consigliamo di creare una server farm federativa di piccole dimensioni in ogni organizzazione partner, con almeno due server federativi per farm, per fornire la tolleranza di errore.  
  
-   Con la necessità di elevata disponibilità e la facilità di scalabilità orizzontale server federativi, scalabilità orizzontale è il metodo consigliato per la gestione di un numero elevato di richieste al secondo per un determinato servizio federativo. Scalabilità oltre la configurazione di base in questa guida è improbabile per produrre capacità significative miglioramenti di gestione.  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>Crescita e dimensione del database di configurazione di AD FS  
Le dimensioni del database di configurazione di ADFS sono generalmente considerata piccoli e dimensione del database non tendono a essere fondamentale nelle distribuzioni di AD FS.  Le dimensioni del database di configurazione di ADFS precisa dipende in gran parte il numero di relazioni di trust e i metadati relativi trust\ associati, ad esempio le attestazioni, regole di attestazione, monitoraggio e le impostazioni configurate per ogni trust. Man mano che aumenta il numero di voci di trust nel database di configurazione, quindi non la necessità di più spazio su disco.  
  
Per ulteriori informazioni sulla distribuzione sul database di configurazione di ADFS, vedere [considerazioni sulla topologia di distribuzione ADFS](AD-FS-Deployment-Topology-Considerations.md).  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>Requisiti di spazio di memoria, CPU e disco  
Fortunatamente, requisiti di spazio di memoria, CPU e disco per i server federativi sono minimi e non sono probabilmente un fattore Guida nelle decisioni di hardware. Per ulteriori informazioni sui requisiti hardware, vedere [appendice a: revisione AD requisiti per ADFS](Appendix-A--Reviewing-AD-FS-Requirements.md).  
  
> [!NOTE]  
> Nei test eseguiti dal team del prodotto AD FS utilizzando una server farm federativa configurata con un dedicata di SQL Server per archiviare il database di configurazione di ADFS, il carico complessivo in SQL Server tendevano ad essere insufficiente. In un test di utilizzo di una farm four\-federation\-server che è stata configurata per utilizzare un singolo SQL Server, l'utilizzo della CPU superiore al 10% nonostante test che i server federativi per l'utilizzo di destinazione.  
  
## <a name="bk_estimatefs"></a>Stimare il numero di server federativi per l'organizzazione  
Allo scopo di semplificare il processo per i server federativi di pianificazione di hardware, il team del prodotto AD FS sviluppato di AD FS capacità ridimensionamento foglio di calcolo pianificazione. Questo foglio di calcolo Excel include funzionalità calculator\ che avranno i dati di utilizzo previsto di fornire informazioni sugli utenti nell'organizzazione e restituire un numero ottimale consigliato di server federativi per l'ambiente di produzione ADFS.  
  
> [!NOTE]  
> Il numero di server federativi che consiglia questo foglio di calcolo si basa sulle specifiche di hardware e di rete che il team del prodotto AD FS usato durante i test. Pertanto, è necessario comprendere il numero di server federativi che consiglia il foglio di calcolo in questo contesto.  Per ulteriori informazioni sulle specifiche usato durante i test, vedere l'argomento [pianificazione della capacità del Server AD FS](Planning-for-AD-FS-Server-Capacity.md).  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>Utilizzando la capacità di ADFS pianificazione foglio di calcolo di ridimensionamento  
Quando si utilizza questo foglio di calcolo, è necessario selezionare un valore \ (entrambi **almeno il 40%**, **60%**, o **80%**\) che meglio rappresenta la percentuale del totale degli utenti che si prevede che invia le richieste di autenticazione ai server federativi durante i periodi di utilizzo.  
  
Quindi, sarà necessario selezionare un valore \ (entrambi **1 minuto**, **15 minuti**, o **1 ora**\) che meglio rappresenta l'intervallo di tempo che si prevede che il periodo di utilizzo di punta per ultimo. Ad esempio, si potrebbe prevede almeno il 40% come valore per il numero totale di utenti che verranno accesso entro un periodo di 15 minuti, o che il 60% degli utenti verrà accesso entro un periodo di 1 ora. Questi valori definiscono il profilo di carico massimo mediante il quale verrà calcolato il Consiglio di ridimensionamento.  
  
Successivamente, è necessario specificare il numero totale di utenti che devono singolo accesso di accesso all'applicazione in grado di riconoscere claims\ di destinazione, in base indica se gli utenti sono:  
  
-   Registrazione in Active Directory da un computer locale che è connessa alla rete aziendale \ (tramite authentication\ integrata di Windows)  
  
-   Registrazione in Active Directory in modalità remota da un computer che non è fisicamente connesso alla rete aziendale \ (tramite Windows integrato autenticazione o nome utente e password\)  
  
-   Da un'altra organizzazione e sono tentando di accedere all'applicazione in grado di riconoscere claims\ destinazione da un partner attendibile  
  
-   Da un provider di identità SAML 2.0 e tentare di accedere all'applicazione in grado di riconoscere claims\ destinazione  
  
#### <a name="how-to-use-this-spreadsheet"></a>Come utilizzare questo foglio di calcolo  
È possibile utilizzare i passaggi seguenti per ogni istanza di farm di server federativo che si intende distribuire per determinare il numero consigliato di server federativi.  
  
1.  Scaricare e quindi aprire il [AD FS capacità ridimensionamento foglio di calcolo pianificazione per Windows Server 2012 R2](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx) o [AD FS capacità ridimensionamento foglio di calcolo pianificazione per Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
  
2.  Nella cella a destra del **aspettarmi durante il periodo di utilizzo del sistema di punta, questa percentuale della mia agli utenti di eseguire l'autenticazione** cella, fare clic sulla cella e quindi usare le frecce di elenchi a discesa per selezionare il livello di utilizzo di sistema stimati, tra **almeno il 40%**, **60%** o **80%** per la distribuzione.  
  
3.  Nella cella a destra del **entro il periodo di tempo seguenti** cella, fare clic sulla cella e quindi usare le frecce di elenchi a discesa per selezionare **1 minuto**, **15 minuti**, o **1 ora** per selezionare la durata di punta.  
  
4.  Nella cella a destra del **invio numero stimato di applicazioni interne \ (ad esempio SharePoint \(2007 or 2010\) o applications\ di riconoscere attestazioni)** cella, digitare il numero di applicazioni interne usi nell'organizzazione.  
  
5.  Nella cella a destra del **invio numero stimato di applicazioni online \ (ad esempio Office 365 Exchange Online, SharePoint Online o Lync Online\)** cella, digitare il numero di applicazioni online o servizi che verranno utilizzati nell'organizzazione.  
  
6.  Sotto la cella intitolata **numero di utenti**, digitare un numero di ogni riga che si applica a uno scenario di applicazione di esempio agli utenti sarà necessario accesso-on di accesso singolo per. In questa colonna deve contenere il numero di utenti definiti, non gli utenti di punta al secondo. Se i tentativi di accesso per l'applicazione devono prima essere inviato tramite la pagina di individuazione dell'area di autenticazione, digitare **Y**. Se si è certi di questa selezione, digitare **Y**.  
  
7.  Esaminare i seguenti vengono forniti i valori consigliati:  
  
    1.  Per il numero totale di server federativi consigliate, vedere la cella inferiore destra evidenziato in grigio.  
  
    2.  Per il numero di server consigliato per ogni scenario di applicazione di esempio, vedere la cella nella riga che viene evidenziata in grigio.  
  
> [!NOTE]  
> Il valore che verrà calcolato automaticamente nella cella a destra della cella intitolata **numero totale di server federativi consigliato** nella parte inferiore del foglio di calcolo contiene una formula che aggiungerà un ulteriore 20% di buffer per la somma totale di tutti i valori in ognuna delle singole righe precedente. La formula aggiunta il **numero totale di server federativi consigliato** cella le build in questo buffer a propria disposizione consigliato numero di server federativo distribuito per rendere molto improbabile che il carico complessivo della farm verrà mai raggiunto il punto di saturazione.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
