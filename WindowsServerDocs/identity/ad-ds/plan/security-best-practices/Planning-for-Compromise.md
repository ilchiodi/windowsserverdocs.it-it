---
ms.assetid: 6f50476c-a1f1-48fb-999b-76c4c3816496
title: Pianificazione del compromesso
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d3d08e954b7a2a9ce58eb61dec54f2848ab68c12
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949166"
---
# <a name="planning-for-compromise"></a>Pianificazione del compromesso

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Legge numero uno: nessuno ritiene che possa verificarsi un problema, fino a quando non lo fa.* - [10 leggi non modificabili dell'amministrazione della sicurezza](https://technet.microsoft.com/library/cc722488.aspx)  
  
I piani di ripristino di emergenza in molte organizzazioni si concentrano sul ripristino da emergenze a livello di area o da errori che comportano la perdita di servizi di elaborazione. Tuttavia, quando si lavora con clienti compromessi, spesso si scopre che il ripristino da un compromesso intenzionale è assente nei piani di ripristino di emergenza. Ciò è particolarmente vero quando la compromissione causa un furto di proprietà intellettuale o di distruzione intenzionale che sfrutta i limiti logici (ad esempio, la distruzione di tutti i domini Active Directory o tutti i server) anziché i limiti fisici, ad esempio distruzione di un Data Center. Anche se un'organizzazione può avere piani di risposta agli eventi imprevisti che definiscono le attività iniziali da intraprendere quando viene individuato un compromesso, questi piani spesso omettono i passaggi per risolvere un compromesso che influiscono sull'intera infrastruttura di elaborazione.  
  
Poiché Active Directory fornisce funzionalità avanzate di gestione delle identità e degli accessi per utenti, server, workstation e applicazioni, è invariabilmente indirizzato da utenti malintenzionati. Se un utente malintenzionato ottiene l'accesso con privilegi elevati a un dominio Active Directory o a un controller di dominio, l'accesso può essere utilizzato per accedere, controllare o persino distruggere l'intera foresta Active Directory.  
  
In questo documento sono stati illustrati alcuni degli attacchi più comuni contro Windows e Active Directory e le contromisure che è possibile implementare per ridurre la superficie di attacco, ma l'unico modo sicuro per recuperare in caso di compromissione completa di Active Directory è preparato per la compromissione prima che si verifichi. Questa sezione è incentrata sui dettagli di implementazione tecnica rispetto alle sezioni precedenti di questo documento e su raccomandazioni di alto livello che è possibile usare per creare un approccio olistico e completo per proteggere e gestire le esigenze aziendali più importanti. risorse aziendali e IT.  
  
Se l'infrastruttura non è mai stata aggredita, ha resistito a tentativi di violazione o ha ceduto ad attacchi ed è stata completamente compromessa, è consigliabile pianificare la realtà inevitabile che verrà nuovamente aggredita. Non è possibile impedire gli attacchi, ma potrebbe essere effettivamente possibile prevenire violazioni significative o compromessi all'ingrosso. Ogni organizzazione deve valutare attentamente i programmi di gestione dei rischi esistenti e apportare le modifiche necessarie per ridurre il livello complessivo di vulnerabilità, grazie a investimenti bilanciati in prevenzione, rilevamento, contenimento e ripristino.  
  
Per creare difese efficaci offrendo al tempo stesso servizi per gli utenti e le aziende che dipendono dall'infrastruttura e dalle applicazioni, potrebbe essere necessario prendere in considerazione nuovi modi per prevenire, rilevare e contenere compromessi nell'ambiente in uso e quindi eseguire il ripristino da compromissione. Gli approcci e le raccomandazioni di questo documento potrebbero non aiutare a riparare un'installazione di Active Directory compromessa, ma possono aiutare a proteggere la prossima.  
  
Le raccomandazioni per il ripristino di una foresta di Active Directory vengono presentate in [Windows Server 2012: pianificazione del ripristino della foresta Active Directory](https://www.microsoft.com/download/details.aspx?id=16506). Potrebbe essere possibile impedire che il nuovo ambiente venga completamente compromesso, ma anche se non è possibile, saranno disponibili strumenti per il ripristino e il recupero del controllo dell'ambiente.  
  
## <a name="rethinking-the-approach"></a>Ripensare l'approccio  
*Legge numero 8: la difficoltà di difesa di una rete è direttamente proporzionale alla complessità.* - [10 leggi non modificabili dell'amministrazione della sicurezza](https://technet.microsoft.com/library/cc722488.aspx)  
  
In generale, se un utente malintenzionato ha ottenuto un accesso di sistema, amministratore, radice o equivalente a un computer, indipendentemente dal sistema operativo, tale computer non può più essere considerato attendibile, indipendentemente dal numero di tentativi effettuati per "pulire" il sistema. Active Directory non è diverso. Se un utente malintenzionato ha ottenuto l'accesso con privilegi a un controller di dominio o a un account con privilegi elevati in Active Directory, a meno che non si disponga di un record di ogni modifica apportata da un utente malintenzionato o di un backup valido noto, non è possibile ripristinare completamente la directory stato attendibile.  
  
Quando un server membro o una workstation viene compromesso e modificato da un utente malintenzionato, il computer non è più attendibile, ma i server e le workstation non compromessi adiacenti possono comunque essere considerati attendibili. La compromissione di un computer non implica che tutti i computer siano compromessi.  
  
Tuttavia, in un dominio Active Directory, tutti i controller di dominio ospitano le repliche dello stesso database di servizi di dominio Active Directory. Se un singolo controller di dominio viene compromesso e un utente malintenzionato modifica il database di servizi di dominio Active Directory, queste modifiche vengono replicate in ogni altro controller di dominio nel dominio e, a seconda della partizione in cui vengono apportate le modifiche, la foresta. Anche se si reinstalla tutti i controller di dominio nella foresta, si stanno semplicemente reinstallando gli host in cui risiede il database di servizi di dominio Active Directory. Le modifiche dannose a Active Directory verranno replicate nei controller di dominio appena installati con la stessa facilità con cui verranno replicati nei controller di dominio che sono stati in esecuzione per anni.  
  
Quando si valutano gli ambienti compromessi, in genere si scopre che la prima violazione dell'evento è stata effettivamente attivata dopo settimane, mesi o addirittura anni dopo che gli utenti malintenzionati hanno inizialmente compromesso l'ambiente. Gli utenti malintenzionati ottengono in genere le credenziali per gli account con privilegi elevati fino a quando è stata rilevata una violazione e sfruttano tali account per compromettere la directory, i controller di dominio, i server membri, le workstation e persino le connessioni non Windows sistemi.  
  
Questi risultati sono coerenti con diversi risultati nel report di analisi delle violazioni dei dati 2012 di Verizon, che indica che:  
  
-   98 percentuale di violazioni dei dati derivanti da agenti esterni  
  
-   il 85% delle violazioni dei dati richiede settimane o più per l'individuazione  
  
-   92 percentuale di eventi imprevisti individuati da terze parti e  
  
-   il 97 percento delle violazioni è stato evitabile anche se controlli semplici o intermedi.  
  
Una compromissione del grado descritto in precedenza è effettivamente irreparabile e il Consiglio standard di "flat and Rebuild" ogni sistema compromesso è semplicemente non fattibile o addirittura possibile se Active Directory è stato compromesso o distrutto. Anche il ripristino a uno stato valido noto non elimina i difetti che consentivano di compromettere l'ambiente in primo luogo.  
  
Sebbene sia necessario difendere ogni facet dell'infrastruttura, un utente malintenzionato deve solo individuare un numero sufficiente di difetti nelle difese per raggiungere l'obiettivo desiderato. Se l'ambiente in uso è relativamente semplice e incontaminato e in passato gestito correttamente, l'implementazione delle indicazioni fornite in precedenza in questo documento può essere una proposta semplice.  
  
Tuttavia, è stato rilevato che l'ambiente più vecchio, più grande e più complesso, più probabilmente è che i consigli in questo documento saranno inutilizzabili o addirittura impossibile da implementare. È molto più difficile proteggere un'infrastruttura dopo che è stata avviata una nuova e costruire un ambiente resistente ad attacchi e compromessi. Tuttavia, come indicato in precedenza, non è sufficiente ricompilare un'intera foresta Active Directory. Per questi motivi, è consigliabile un approccio più mirato e mirato per proteggere le foreste Active Directory.  
  
Invece di concentrarsi e provare a correggere tutti gli elementi che sono "interrotti", si consideri un approccio in cui la priorità si basa su ciò che è più importante per l'azienda e nell'infrastruttura. Anziché provare a correggere un ambiente riempito con sistemi e applicazioni obsoleti e non configurati in modo corretto, è consigliabile creare un nuovo ambiente sicuro e di piccole dimensioni in cui è possibile trasferire in modo sicuro gli utenti, i sistemi e le informazioni più importanti per il business.  
  
In questa sezione viene descritto un approccio tramite il quale è possibile creare una foresta di Active Directory Domain Services che funge da "vita" o "sicura" per l'infrastruttura aziendale principale. Una foresta incontaminata è semplicemente una foresta Active Directory appena installata, in genere limitata a dimensione e ambito, e compilata usando i sistemi operativi correnti, le applicazioni e i principi descritti in [riduzione della superficie di attacco Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md).  
  
Implementando le impostazioni di configurazione consigliate in una foresta appena compilata, è possibile creare un'installazione di servizi di dominio Active Directory creata da zero con impostazioni e procedure sicure ed è possibile ridurre le richieste che accompagnano il supporto dei sistemi legacy e applicazioni. Sebbene le istruzioni dettagliate per la progettazione e l'implementazione di un'installazione incontaminata di servizi di dominio Active Directory non rientrano nell'ambito di questo documento, è necessario seguire alcuni principi e linee guida generali per creare una "cella sicura" in cui sia possibile ospitare la più critica Asset. Queste linee guida sono le seguenti:  
  
1.  Identificare i principi per la separazione e la protezione degli asset critici.  
  
2.  Definire un piano di migrazione limitato e basato sul rischio.  
  
3.  Se necessario, sfruttare le migrazioni "non migratorie".  
  
4.  Implementare la "distruzione creativa".  
  
5.  Isolare i sistemi e le applicazioni legacy.  
  
6.  Semplifica la sicurezza per gli utenti finali.  
  
### <a name="identifying-principles-for-segregating-and-securing-critical-assets"></a>Identificazione dei principi per la separazione e la protezione di asset critici  

Le caratteristiche dell'ambiente incontaminato creato per ospitare asset critici possono variare notevolmente. Ad esempio, è possibile scegliere di creare una foresta incontaminata in cui si esegue la migrazione solo di utenti VIP e dati sensibili a cui solo gli utenti possono accedere. È possibile creare una foresta incontaminata in cui si esegue la migrazione non solo degli utenti VIP, ma implementata come foresta amministrativa, implementando i principi descritti in [riduzione della superficie di attacco Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md) per creare account amministrativi e host protetti che possono essere usati per gestire le foreste legacy dalla foresta incontaminata. È possibile implementare una foresta "finalizzata" che ospita account VIP, account con privilegi e sistemi che richiedono sicurezza aggiuntiva, ad esempio i server che eseguono Servizi certificati Active Directory (AD CS) con l'unico obiettivo di separarli da meno sicuri foreste. Infine, è possibile implementare una foresta incontaminata che diventa la posizione de facto per tutti i nuovi utenti, sistemi, applicazioni e dati, consentendo alla fine di rimuovere le autorizzazioni della foresta legacy tramite attrito.  
  
Indipendentemente dal fatto che la foresta incontaminata contenga un numero limitato di utenti e sistemi o costituisce la base per una migrazione più aggressiva, è necessario attenersi a questi principi nella pianificazione:  
  
1.  Si supponga che le foreste legacy siano state compromesse.  
  
2.  Non configurare un ambiente incontaminato per considerare attendibile una foresta legacy, sebbene sia possibile configurare un ambiente legacy per considerare attendibile una foresta incontaminata.  
  
3.  Non eseguire la migrazione di account utente o gruppi da una foresta legacy a un ambiente incontaminato se è possibile che l'appartenenza a gruppi degli account, la cronologia SID o altri attributi siano stati modificati in modo dannoso. Usare invece approcci "non migratori" per popolare una foresta incontaminata. Gli approcci non migratori sono descritti più avanti in questa sezione.  
  
4.  Non eseguire la migrazione di computer da foreste legacy a foreste incontaminate. Implementare i server appena installati nella foresta incontaminata, installare le applicazioni nei server appena installati e migrare i dati dell'applicazione nei sistemi appena installati. Per i file server, copiare i dati nei server appena installati, impostare gli ACL usando utenti e gruppi nella nuova foresta, quindi creare server di stampa in modo analogo.  
  
5.  Non consentire l'installazione di applicazioni o sistemi operativi legacy nella foresta incontaminata. Se un'applicazione non può essere aggiornata e installata di recente, lasciarla nella foresta legacy e prendere in considerazione la distruzione creativa per sostituire la funzionalità dell'applicazione.  
  
### <a name="defining-a-limited-risk-based-migration-plan"></a>Definizione di un piano di migrazione limitato e basato sul rischio  
La creazione di un piano di migrazione limitato e basato sul rischio significa semplicemente che quando si decidono gli utenti, le applicazioni e i dati di cui eseguire la migrazione nella foresta incontaminata, è necessario identificare le destinazioni di migrazione in base al livello di rischio a cui l'organizzazione viene esposta se uno dei Gli utenti o i sistemi sono compromessi. Gli utenti VIP i cui account hanno più probabilità di essere assegnati agli utenti malintenzionati devono essere ospitati nella foresta incontaminata. Le applicazioni che forniscono funzioni aziendali essenziali devono essere installate in server appena creati nella foresta incontaminata e i dati estremamente sensibili devono essere spostati in server protetti nella foresta incontaminata.  
  
Se non si dispone già di un quadro chiaro degli utenti, dei sistemi, delle applicazioni e dei dati cruciali per l'azienda nell'ambiente Active Directory, utilizzare le business unit per identificarli. È necessario identificare tutte le applicazioni necessarie per il funzionamento dell'azienda, in modo che tutti i server in cui vengono eseguite le applicazioni critiche o i dati critici vengano archiviati. Identificando gli utenti e le risorse necessari per la continuazione del funzionamento dell'organizzazione, si crea una raccolta con priorità naturale di asset su cui concentrare il lavoro.  
  
### <a name="leveraging-nonmigratory-migrations"></a>Uso delle migrazioni "non migratorie"  
Che tu sappia che l'ambiente è stato compromesso, sospetta che sia stato compromesso o semplicemente preferisci non eseguire la migrazione di dati e oggetti legacy da un'installazione Active Directory legacy a una nuova, prendere in considerazione approcci di migrazione che non tecnicamente oggetti "migrate".  
  
### <a name="user-accounts"></a>Account utenti  
In una migrazione Active Directory tradizionale da una foresta a un'altra, l'attributo SIDHistory (cronologia SID) sugli oggetti utente viene usato per archiviare il SID degli utenti e i SID dei gruppi a cui gli utenti sono membri nella foresta legacy. Se viene eseguita la migrazione degli account utente a una nuova foresta e si accede alle risorse nella foresta legacy, i SID nella cronologia SID vengono usati per creare un token di accesso che consente agli utenti di accedere alle risorse a cui avevano accesso prima della migrazione degli account.  
  
La gestione della cronologia SID, tuttavia, si è rivelata problematica in alcuni ambienti perché il popolamento dei token di accesso degli utenti con i SID correnti e cronologici può causare un aumento del token. L'aumento del numero di token è un problema per cui il numero di SID che devono essere archiviati nel token di accesso di un utente utilizza o supera la quantità di spazio disponibile nel token.  
  
Sebbene le dimensioni dei token possano essere aumentate a un extent limitato, la soluzione definitiva per la riduzione dei token consiste nel ridurre il numero di SID associati agli account utente, se si razionalizzano le appartenenze ai gruppi, si elimina la cronologia SID o una combinazione di entrambi. Per altre informazioni sull'aumento del numero di token, vedere MaxTokenSize e il numero di [token Kerberos](https://blogs.technet.com/b/shanecothran/archive/2010/07/16/maxtokensize-and-kerberos-token-bloat.aspx).  
  
Anziché eseguire la migrazione degli utenti da un ambiente legacy (in particolare, in cui le appartenenze a gruppi e le cronologie SID potrebbero essere compromesse) usando la cronologia SID, provare a sfruttare le applicazioni metadirectory per eseguire la migrazione degli utenti, senza usare le cronologie dei SID nella nuova foresta. Quando gli account utente vengono creati nella nuova foresta, è possibile usare un'applicazione metadirectory per eseguire il mapping degli account agli account corrispondenti nella foresta legacy.  
  
Per consentire ai nuovi account utente di accedere alle risorse nella foresta legacy, è possibile usare gli strumenti metadirectory per identificare i gruppi di risorse in cui è stato concesso l'accesso agli account legacy degli utenti, quindi aggiungere i nuovi account degli utenti a tali gruppi. A seconda della strategia di gruppo nella foresta legacy, potrebbe essere necessario creare gruppi locali di dominio per l'accesso alle risorse o convertire gruppi esistenti in gruppi locali di dominio per consentire l'aggiunta di nuovi account ai gruppi di risorse. Concentrandosi innanzitutto sulle applicazioni e sui dati più critici e sulla relativa migrazione al nuovo ambiente (con o senza la cronologia SID), è possibile limitare la quantità di lavoro richiesto nell'ambiente legacy.  
  

  
### <a name="servers-and-workstations"></a>Server e workstation  
In una migrazione tradizionale da una foresta Active Directory a un'altra, la migrazione dei computer è spesso relativamente semplice rispetto alla migrazione di utenti, gruppi e applicazioni. A seconda del ruolo del computer, la migrazione a una nuova foresta può essere semplice come separare un dominio precedente e aggiungerne uno nuovo. Tuttavia, la migrazione degli account computer intatti in una foresta incontaminata vanifica lo scopo della creazione di un nuovo ambiente. Anziché eseguire la migrazione degli account computer (potenzialmente compromessi, non configurati o obsoleti) a una nuova foresta, è necessario installare i server e le workstation nel nuovo ambiente. È possibile migrare i dati dai sistemi della foresta legacy ai sistemi nella foresta incontaminata, ma non ai sistemi che ospitano i dati.  
  
### <a name="applications"></a>Applicazioni  

Le applicazioni possono presentare la sfida più significativa in ogni migrazione da una foresta a un'altra, ma nel caso di una migrazione "non migrabile", uno dei principi fondamentali da applicare è che le applicazioni nella foresta incontaminata devono essere aggiornate. supportato e installato di recente. È possibile eseguire la migrazione dei dati dalle istanze dell'applicazione nella foresta precedente, ove possibile. Nelle situazioni in cui un'applicazione non può essere "ricreata" nella foresta incontaminata, è consigliabile prendere in considerazione approcci come la distruzione creativa o l'isolamento di applicazioni legacy, come descritto nella sezione seguente.  
  
### <a name="implementing-creative-destruction"></a>Implementazione della distruzione creativa  
La distruzione creativa è un termine economico che descrive lo sviluppo economico creato dalla distruzione di un ordine precedente. Negli ultimi anni, il termine è stato applicato all'Information Technology. Si riferisce in genere ai meccanismi in base ai quali l'infrastruttura precedente viene eliminata, non tramite l'aggiornamento, ma sostituendo questo elemento con qualcosa di completamente nuovo. Il [simposio 2011 Gartner ITxpo](http://www.gartner.com/technology/symposium/orlando/) per cIOS e dirigenti IT Senior ha presentato la distruzione creativa come uno dei temi principali per la riduzione dei costi e l'incremento dell'efficienza. I miglioramenti apportati alla sicurezza sono possibili come una naturale crescita del processo.  

Un'organizzazione, ad esempio, può essere costituita da più business unit che utilizzano un'applicazione diversa che esegue funzionalità simili, con diversi gradi di modernità e supporto del fornitore. In passato, potrebbe essere responsabile della gestione separata dell'applicazione di ogni business unit e l'impegno di consolidamento consiste nel tentare di determinare quale applicazione ha offerto la funzionalità migliore e quindi di eseguire la migrazione dei dati applicazione dagli altri.  
  
Nella distruzione creativa, anziché mantenere le applicazioni obsolete o ridondanti, si implementano applicazioni completamente nuove per sostituire quelle obsolete, migrare i dati nelle nuove applicazioni e rimuovere le autorizzazioni delle applicazioni precedenti e dei sistemi in cui vengono eseguite. In alcuni casi, è possibile implementare la distruzione creativa di applicazioni legacy distribuendo una nuova applicazione nella propria infrastruttura, ma, laddove possibile, è consigliabile eseguire il porting dell'applicazione a una soluzione basata sul cloud.  
  
Grazie alla distribuzione di applicazioni basate su cloud per la sostituzione di applicazioni interne legacy, non solo si riducono i costi e le attività di manutenzione, ma si riduce la superficie di attacco dell'organizzazione eliminando i sistemi legacy e le applicazioni che presentano vulnerabilità per utilizzare gli utenti malintenzionati. Questo approccio consente a un'organizzazione di ottenere in modo più rapido la funzionalità desiderata eliminando contemporaneamente le destinazioni legacy nell'infrastruttura. Sebbene il principio di distruzione creativa non si applichi a tutti gli asset IT, offre un'opzione spesso praticabile per eliminare i sistemi e le applicazioni legacy, distribuendo contemporaneamente applicazioni solide e sicure basate sul cloud.  
  
### <a name="isolating-legacy-systems-and-applications"></a>Isolamento di applicazioni e sistemi legacy  
Una crescita naturale della migrazione degli utenti e dei sistemi cruciali per l'azienda a un ambiente protetto incontaminato è che la foresta legacy conterrà informazioni e sistemi meno importanti. Sebbene i sistemi e le applicazioni legacy che rimangono nell'ambiente meno sicuro possano presentare un rischio elevato di compromissione, rappresentano anche una minore gravità del compromesso. Grazie al riattivazione e alla modernizzazione delle risorse aziendali cruciali, puoi concentrarti sulla distribuzione di gestione e monitoraggio efficaci senza dover gestire le impostazioni e i protocolli legacy.  
  
Quando i dati critici sono stati riospitati in una foresta incontaminata, è possibile valutare le opzioni per isolare ulteriormente i sistemi e le applicazioni legacy nella foresta di servizi di dominio Active Directory "principale". Sebbene sia possibile implementare la distruzione creativa per sostituire un'applicazione e i server in cui è in esecuzione, in altri casi è possibile considerare un isolamento aggiuntivo dei sistemi e delle applicazioni meno sicuri. Ad esempio, un'applicazione utilizzata da un numero limitato di utenti, ma che richiede credenziali legacy come gli hash di LAN Manager, è possibile eseguire la migrazione a un dominio di piccole dimensioni creato per supportare sistemi per i quali non sono disponibili opzioni di sostituzione.  
  
Rimuovendo questi sistemi da domini in cui è stata forzata l'implementazione di impostazioni legacy, è possibile aumentare la sicurezza dei domini configurando tali sistemi per supportare solo le applicazioni e i sistemi operativi correnti. Sebbene sia preferibile rimuovere le autorizzazioni di sistemi e applicazioni legacy, quando possibile. Se la rimozione delle autorizzazioni non è semplicemente fattibile per un piccolo segmento della popolazione legacy, la relativa separazione in un dominio o in una foresta separata consente di eseguire miglioramenti incrementali nel resto dell'installazione legacy.  
  
### <a name="simplifying-security-for-end-users"></a>Semplificazione della sicurezza per gli utenti finali  
Nella maggior parte delle organizzazioni, gli utenti che hanno accesso alle informazioni più sensibili a causa della natura dei propri ruoli nell'organizzazione hanno spesso la quantità minima di tempo da dedicare alla formazione di restrizioni e controlli di accesso complessi. Anche se è necessario disporre di un programma di formazione per la sicurezza completo per tutti gli utenti dell'organizzazione, è inoltre necessario concentrarsi sulla creazione di una sicurezza più semplice da utilizzare, grazie all'implementazione di controlli trasparenti e semplificando i principi a cui gli utenti aderire.  
  
È ad esempio possibile definire un criterio in cui i dirigenti e gli altri indirizzi VIP devono usare workstation sicure per accedere ai dati e ai sistemi sensibili, consentendo loro di usare gli altri dispositivi per accedere ai dati meno sensibili. Si tratta di un principio semplice da ricordare per gli utenti, ma è possibile implementare una serie di controlli back-end che consentono di applicare l'approccio.  

È possibile usare la [garanzia del meccanismo di autenticazione](https://technet.microsoft.com/library/dd391847(v=WS.10).aspx) per consentire agli utenti di accedere ai dati sensibili solo se hanno effettuato l'accesso ai sistemi protetti usando le smart card e possono usare restrizioni relative ai diritti utente e IPSec per controllare i sistemi da cui possono connettersi ai repository di dati sensibili. È possibile utilizzare [Microsoft Data Classification Toolkit](https://www.microsoft.com/download/details.aspx?id=27123) per creare un'infrastruttura di classificazione file affidabile ed è possibile implementare il [controllo dinamico degli accessi](https://blogs.technet.com/b/windowsserver/archive/2012/05/22/introduction-to-windows-server-2012-dynamic-access-control.aspx) per limitare l'accesso ai dati in base alle caratteristiche di un tentativo di accesso, traducendo le regole business in controlli tecnici.  
  
Dal punto di vista dell'utente, l'accesso ai dati sensibili da un sistema protetto "Just Works" e il tentativo di eseguire questa operazione da un sistema non protetto "just not". Tuttavia, dal punto di vista del monitoraggio e della gestione dell'ambiente, si contribuisce a creare modelli identificabili in modo da consentire agli utenti di accedere a dati e sistemi sensibili, semplificando il rilevamento di tentativi di accesso anomali.  
  
Negli ambienti in cui la resistenza degli utenti alle password lunghe e complesse ha comportato un numero insufficiente di criteri per le password, in particolare per gli utenti VIP, prendere in considerazione approcci alternativi all'autenticazione, sia tramite smart card (che si riferiscono a diversi fattori di forma che con le funzionalità aggiuntive per rafforzare l'autenticazione, i controlli biometrici, come i lettori con swipe dito, o anche i dati di autenticazione protetti da chip TPM (Trusted Platform Module) nei computer degli utenti. Anche se l'autenticazione a più fattori non impedisce gli attacchi di furto di credenziali se un computer è già compromesso, offrendo agli utenti controlli di autenticazione facili da usare, è possibile assegnare password più solide agli account degli utenti per i quali l'utente tradizionale i controlli nome e password sono ingombranti.  
