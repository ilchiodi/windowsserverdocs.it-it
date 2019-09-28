---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: Pianificazione della capacità per i server federativi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 418bc5d53a2bd11afa8563b07bbff76c89495715
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407972"
---
# <a name="planning-for-federation-server-capacity"></a>Pianificazione della capacità per i server federativi

La pianificazione della capacità per i server federativi consente di stimare:  
  
-   Quali fattori aumentano le dimensioni del database di configurazione AD FS.  
  
-   Requisiti hardware appropriati per ogni server federativo.  
  
-   Il numero di server federativi da inserire in ogni organizzazione.  
  
I server federativi rilasciano token di sicurezza agli utenti. Questi token vengono presentati a un relying party per l'utilizzo. I server federativi rilasciano token di sicurezza dopo l'autenticazione di un utente o dopo la ricezione di un token di sicurezza emesso in precedenza da un Servizio federativo partner. Un token di sicurezza viene richiesto da un Servizio federativo quando gli utenti accedono inizialmente alle applicazioni federate o quando i token di sicurezza scadono durante l'accesso alle applicazioni federate.  
  
I server federativi sono progettati per supportare le configurazioni server farm High @ no__t-0availability che usano la tecnologia Microsoft Network Load Balancing \(NLB @ no__t-2. I server federativi in una configurazione di farm possono soddisfare le richieste in modo indipendente, senza accedere a componenti della farm comuni per ogni richiesta. Pertanto, il ridimensionamento di una distribuzione server federativo comporta un minor overhead.  
  
**Raccomandazioni**  
  
-   Per le distribuzioni Mission @ no__t-0critical o High @ no__t-1availability, si consiglia di creare una piccola Federazione server farm in ogni organizzazione partner, con almeno due server federativi per farm, per fornire la tolleranza di errore.  
  
-   Con la necessità di disponibilità elevata e la facilità di scalabilità orizzontale dei server federativi, la scalabilità orizzontale è il metodo consigliato per la gestione di un numero elevato di richieste al secondo per una particolare Servizio federativo. La scalabilità superiore rispetto alla configurazione di base in questa guida è improbabile che si ottenga un miglioramento significativo della gestione della capacità.  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>Dimensioni e crescita del database di configurazione AD FS  
La dimensione del database di configurazione AD FS è generalmente considerata ridotta e le dimensioni del database non tendono a essere una considerazione importante nelle distribuzioni AD FS.  La dimensione precisa del database di configurazione AD FS può dipendere in gran parte dal numero di relazioni di trust e dai metadati associati @ no__t-0related, ad esempio attestazioni, regole attestazioni e impostazioni di monitoraggio configurate per ogni trust. Con l'aumentare del numero di voci di attendibilità nel database di configurazione, è necessario più spazio su disco.  
  
Per ulteriori informazioni sulla distribuzione del database di configurazione di AD FS, vedere [considerazioni sulla topologia di distribuzione di ad FS](AD-FS-Deployment-Topology-Considerations.md).  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>Requisiti di memoria, CPU e spazio su disco  
Fortunatamente, i requisiti di memoria, CPU e spazio su disco per i server federativi sono semplici e non sono probabilmente un fattore determinante per le decisioni relative all'hardware. Per ulteriori informazioni sui requisiti hardware, vedere [Appendix A: Revisione dei requisiti di AD FS @ no__t-0.  
  
> [!NOTE]  
> Nei test eseguiti dal team del prodotto AD FS utilizzando una Federazione server farm configurata con un SQL Server dedicato per archiviare il database di configurazione AD FS, il carico complessivo sul SQL Server si è rivelato basso. In un test che utilizza una farm di quattro @ no__t-0federation @ no__t-1Server configurata per l'utilizzo di una singola SQL Server, l'utilizzo della CPU non supera il 10% nonostante i test che hanno portato i server federativi alla destinazione dell'utilizzo.  
  
## <a name="bk_estimatefs"></a>Stimare il numero di server federativi per l'organizzazione  
Per semplificare il processo di pianificazione dell'hardware per i server federativi, il team del prodotto AD FS ha sviluppato il foglio di calcolo per il dimensionamento della pianificazione della capacità AD FS. Questo foglio di calcolo di Excel include la funzionalità Calculator @ no__t-0like che consentirà di ottenere i dati di utilizzo previsti sugli utenti dell'organizzazione e di restituire un numero ottimale consigliato di server federativi per l'ambiente di produzione AD FS.  
  
> [!NOTE]  
> Il numero di server federativi che questo foglio di calcolo consiglierà si basa sulle specifiche hardware e di rete utilizzate dal team del prodotto AD FS durante i test. Pertanto, il numero di server federativi che verrà consigliato dal foglio di calcolo deve essere compreso in questo contesto.  Per ulteriori informazioni sulle specifiche utilizzate durante i test, vedere l'argomento relativo alla [pianificazione della capacità del Server ad FS](Planning-for-AD-FS-Server-Capacity.md).  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>Uso del foglio di calcolo per la pianificazione della capacità AD FS  
Quando si usa questo foglio di calcolo, è necessario selezionare un valore \(either **40%** , **60%** o **80%** \) che rappresenta meglio la percentuale di utenti totali che si prevede invierà le richieste di autenticazione ai server federativi durante periodi di utilizzo massimi.  
  
Sarà quindi necessario selezionare un valore \(either **1 minuto**, **15 minuti**o **1 ora**\) che rappresenta il periodo di tempo previsto per l'ultimo periodo di utilizzo massimo. È ad esempio possibile stimare il 40% come valore per il numero totale di utenti che effettueranno l'accesso entro un periodo di 15 minuti o che il 60% degli utenti effettuerà l'accesso entro un periodo di 1 ora. Insieme, questi valori definiscono il profilo di carico di picco in base al quale verrà calcolata la raccomandazione di ridimensionamento.  
  
Sarà quindi necessario specificare il numero totale di utenti che richiederanno l'accesso Single Sign @ no__t-0on all'applicazione Claims @ no__t-1aware di destinazione, a seconda che gli utenti siano:  
  
-   Accesso Active Directory da un computer locale connesso fisicamente alla rete aziendale @no__t 0through autenticazione integrata di Windows @ no__t-1  
  
-   Accesso Active Directory remoto da un computer non connesso fisicamente alla rete aziendale @no__t 0through autenticazione integrata di Windows o nome utente e password @ no__t-1  
  
-   Da un'altra organizzazione e tentano di accedere all'applicazione Claims @ no__t-0aware di destinazione da un partner attendibile  
  
-   Da un provider di identità SAML 2,0 e tentano di accedere all'applicazione di destinazione Claims @ no__t-0aware  
  
#### <a name="how-to-use-this-spreadsheet"></a>Come usare il foglio di calcolo  
Per determinare il numero consigliato di server federativi, è possibile utilizzare i passaggi seguenti per ogni istanza di server farm di federazione che si intende distribuire.  
  
1.  Scaricare e aprire il [foglio di calcolo per la pianificazione della capacità di ad FS per Windows server 2012 R2](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx) o il [foglio di calcolo per il dimensionamento della pianificazione della capacità ad FS per Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
  
2.  Nella cella a destra di **durante il periodo di picco di utilizzo del sistema, si prevede che questa percentuale di utenti esegua l'autenticazione** della cella, fare clic sulla cella e quindi utilizzare le frecce drop @ no__t-1down per selezionare il livello stimato di utilizzo del sistema, ovvero **40%** , **60%** o **80%** per la distribuzione.  
  
3.  Nella cella a destra di **entro il periodo di tempo seguente** , fare clic sulla cella e quindi usare le frecce drop @ no__t-1down per selezionare **1 minuto**, **15 minuti**o **1 ora** per selezionare la durata del picco di carico.  
  
4.  Nella cella a destra del **immettere il numero stimato di applicazioni interne \(such come SharePoint \(2007 o 2010 @ no__t-3 o delle applicazioni Web in grado di riconoscere attestazioni @ no__t-4** , digitare il numero di applicazioni interne che si utilizzerà nel organizzazione.  
  
5.  Nella cella a destra del **immettere il numero stimato di applicazioni online @no__t 1such come Office 365 Exchange Online, SharePoint Online o Lync Online @ no__t-2** cella, digitare il numero di applicazioni o servizi in linea che si utilizzeranno nell'organizzazione.  
  
6.  Sotto la cella **numero di utenti**Digitare un numero in ogni riga che si applica a uno scenario di applicazione di esempio a cui gli utenti dovranno accedere Single Sign @ no__t-1Sulla Barra. Questa colonna deve contenere il numero di utenti definiti, non i picchi di utenti al secondo. Se i tentativi di accesso all'applicazione devono essere eseguiti per la prima volta nella pagina di individuazione dell'area di autenticazione principale, digitare **Y**. Se non si è certi di questa selezione, digitare **Y**.  
  
7.  Esaminare i valori consigliati seguenti:  
  
    1.  Per il numero totale di server federativi consigliati, vedere la cella inferiore destra evidenziata in grigio.  
  
    2.  Per il numero di server consigliati per ogni scenario di applicazione di esempio, vedere la cella nella riga evidenziata in grigio.  
  
> [!NOTE]  
> Il valore che verrà calcolato automaticamente nella cella a destra della cella denominata **numero totale di server federativi consigliati** nella parte inferiore del foglio di calcolo contiene una formula che aggiungerà un buffer aggiuntivo del 20% alla somma totale di tutti i valori in ognuna delle singole righe che lo precedono. La formula aggiunta al **numero totale di server federativi consigliati** compilazioni in questo buffer al numero totale consigliato di server federativi distribuiti per rendere molto improbabile che il carico complessivo della farm raggiungerà il punto di saturazione.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
