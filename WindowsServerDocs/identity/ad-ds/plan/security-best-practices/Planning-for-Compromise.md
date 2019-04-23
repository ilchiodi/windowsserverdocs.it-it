---
ms.assetid: 6f50476c-a1f1-48fb-999b-76c4c3816496
title: Pianificazione del compromesso
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0f6a30c91590667a0894b52ec7c188a43bd5e351
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835572"
---
# <a name="planning-for-compromise"></a>Pianificazione del compromesso

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Legge numero uno: Nessuno pensa che errata potrebbe accadere qualunque cosa, fino a quando non avviene.* - [10 leggi immutabili della protezione amministrazione](https://technet.microsoft.com/library/cc722488.aspx)  
  
I piani di ripristino di emergenza in molte organizzazioni si concentrerà sul recupero da errori che causa la perdita di servizi di calcolo o emergenze locali. Tuttavia, quando si lavora con i clienti manomessi, viene spesso trovare che il ripristino dagli attacchi intenzionali è assente nei relativi piani di ripristino di emergenza. Questo è particolarmente vero quando la compromissione comporta il furto di proprietà intellettuale o la Decostruzione intenzionale che sfrutta i limiti logici (ad esempio, la distruzione di tutti i server o tutti i domini di Active Directory) anziché i confini fisici (ad esempio eliminazione di un Data Center). Anche se un'organizzazione può avere i piani di risposta agli eventi imprevisti che definiscono le attività iniziali da eseguire quando viene individuata una compromissione, questi piani spesso omettono i passaggi di ripristino dalla compromissione di un tipo che interessa l'intera infrastruttura di calcolo.  
  
Poiché Active Directory offre funzionalità avanzate di gestione delle identità e accesso per utenti, i server, le workstation e le applicazioni, si è invariabilmente attaccato da utenti malintenzionati. Se un utente malintenzionato ottiene accesso con privilegiato elevati per un controller di dominio o dominio di Active Directory, che l'accesso può essere sfruttata per accedere, controllare o addirittura distruggere l'intera foresta di Active Directory.  
  
Questo documento ha illustrato alcuni degli attacchi più comuni contro Windows e Active Directory e alle contromisure è possibile implementare in modo da ridurre la superficie di attacco, ma l'unico modo sicuro per il ripristino in caso di compromissione completa di Active Directory deve essere Preparazione per la compromissione prima che venga eseguito. Questa sezione è incentrata minore sui dettagli di implementazione tecnica più sezioni precedenti di questo documento e più sulle indicazioni generali che è possibile usare per creare un approccio olistico e completa per proteggere e gestire l'organizzazione del critico Business e le risorse IT.  
  
Se l'infrastruttura non è mai stato attaccato, ha resisted ha tentato di violazioni della sicurezza, o succumbed agli attacchi e che stata completamente compromessa, è necessario pianificare la realtà inevitabile che sarà sottoposta ad attacchi più volte. Non è possibile impedire gli attacchi, ma è effettivamente possibile impedire violazioni della sicurezza significative o all'ingrosso compromissione. Ogni organizzazione deve strettamente valutare i propri programmi di gestione di rischio esistenti e apportare le modifiche necessarie per aiutare a ridurre il livello complessivo di una vulnerabilità bilanciato investimenti nella prevenzione, rilevamento, contenuto e ripristino.  
  
Per creare difese efficaci pur fornendo servizi per gli utenti e aziende che dipendono l'infrastruttura e applicazioni, potrebbe necessarie da prendere in considerazione nuovi modi per prevenire, rilevare e contengono compromessi nell'ambiente in uso e quindi ripristinare da la compromissione. Gli approcci e i consigli forniti in questo documento non consentono di ripristinare un'installazione di Active Directory compromessa, ma aiuta a proteggere quello successivo.  
  
Raccomandazioni per il ripristino di una foresta di Active Directory vengono presentate in [Windows Server 2012: La pianificazione del ripristino di foreste di Active Directory](https://www.microsoft.com/download/details.aspx?id=16506). Potrebbe essere possibile evitare che il nuovo ambiente da eventuali compromissioni completamente, ma anche se non è possibile, sarà necessario gli strumenti per ripristinare e riprendere il controllo dell'ambiente.  
  
## <a name="rethinking-the-approach"></a>Rethinking l'approccio  
*Legge numero otto: La difficoltà di difesa contro una rete è direttamente proporzionale alla sua complessità.* - [10 leggi immutabili della protezione amministrazione](https://technet.microsoft.com/library/cc722488.aspx)  
  
È generalmente ben accettato che se un utente malintenzionato abbia ottenuto SYSTEM, Administrator, radice o accesso equivalente a un computer, indipendentemente dal sistema operativo, tale computer non possa non è più considerato attendibile, indipendentemente da quante attività sono apportate nel "pulirla" il System. Active Directory non è diverso. Se un utente malintenzionato ha ottenuto accesso con privilegi a un controller di dominio o un account con privilegiato elevati in Active Directory, a meno che non si dispone di un record di tutte le modifiche di cui l'utente malintenzionato effettua o un backup valido noto, è non possibile ripristinare mai la directory da un completamente stato di idoneità.  
  
Quando un server membro o una workstation è compromessa e modificata da un utente malintenzionato, il computer non è più attendibile, ma le workstation e server non compromesso adiacenti possono essere considerato attendibile. Compromissione di un computer non implica che tutti i computer vengano compromesse.  
  
Tuttavia, in un dominio di Active Directory, tutti i controller di dominio ospitano repliche dello stesso database di Active Directory Domain Services. Se un controller di dominio singolo è compromesso e un utente malintenzionato modifica il database di Active Directory Domain Services, eseguire la replica tali modifiche in tutti gli altri controller di dominio nel dominio e a seconda della partizione in cui vengono apportate le modifiche, l'insieme di strutture. Anche se si reinstalla ogni controller di dominio nella foresta, si siano reinstallando semplicemente gli host in cui risiede il database di Active Directory Domain Services. Modifiche non autorizzate ad Active Directory replicherà i controller di dominio appena installata la stessa facilità con cui replicherà i controller di dominio siano in esecuzione da anni.  
  
Ambienti di compromessi per la valutazione, comunemente Riteniamo che ciò che è stata che si ritiene essere la prima violazione "event" è stata effettivamente attivata dopo settimane, mesi o addirittura anni dopo che gli utenti malintenzionati inizialmente erano compromesso l'ambiente. Gli utenti malintenzionati in genere ottenuto le credenziali per gli account con privilegi elevati di tempo prima è stata rilevata una violazione, essi sfruttato questi account per compromettere la directory, i controller di dominio, server membri, le workstation e anche connesse non-Windows sistemi.  
  
Questi risultati siano coerenti con i risultati diversi nel Verizon 2012 dati violazione indagini Report, che indica che:  
  
-   98% delle violazioni dei dati morfologica da agenti esterni  
  
-   l'85% delle violazioni dei dati ha impiegato settimane o altre informazioni  
  
-   il 92% degli eventi imprevisti sono stati individuati da terze parti, e  
  
-   il 97% di violazioni della sicurezza sono state evitabili anche se semplice o controlli a livello intermedio.  
  
È irreversibile in modo efficace, questo compromesso al grado descritto in precedenza e l'avviso di standard per "appiattire e ricompilare" tutti i sistemi compromessi semplicemente non sono fattibile o persino possibile se Active Directory è stato compromesso o eliminato definitivamente. Anche il ripristino in uno stato noto soddisfacente non elimina i difetti che l'ambiente sia compromessa in primo luogo è consentita.  
  
Anche se è necessario difendersi ogni aspetto dell'infrastruttura, un utente malintenzionato deve solo trovare sufficiente difetti in difese per ottenere l'obiettivo desiderato. Se l'ambiente è relativamente semplice e originario ed è da sempre ben gestito, implementando le raccomandazioni fornite precedentemente in questo documento può essere una soluzione semplice e immediata.  
  
Tuttavia, abbiamo scoperto che meno recenti, più grandi e più complessa l'ambiente, più è probabile è che i consigli riportati in questo documento saranno computazionale, o addirittura impossibili da implementare. È molto più difficile proteggere un'infrastruttura al termine dell'attività anziché ricominciare dall'inizio e per creare un ambiente che sia resistente agli attacchi e compromettere. Ma come indicato in precedenza, non è nessuna piccola impresa ricompilare un'intera foresta di Active Directory. Per questi motivi, è consigliabile una più specifico, un approccio mirato per proteggere le foreste di Active Directory.  
  
Invece di concentrarsi sull'e tentare di risolvere tutte le operazioni che sono "interrotte", è consigliabile un approccio in cui è la priorità basato sugli aspetti più importanti per l'azienda e dell'infrastruttura. Anziché tentare di correggere un ambiente riempito con applicazioni e sistemi non aggiornati, non configurato correttamente, è consigliabile creare un nuovo ambiente di piccole dimensioni e sicuro in cui è possibile trasferire in modo sicuro, i sistemi gli utenti le informazioni più importanti per il Azienda.  
  
In questa sezione viene descritto un approccio che è possibile creare una foresta di Active Directory Domain Services originario che viene usato come un "barca vita" o "sicuro cella" per l'infrastruttura di business di base. Un insieme di strutture è semplicemente un'appena installata foresta di Active Directory che è in genere limitato in dimensioni e l'ambito e che viene compilato utilizzando corrente i sistemi operativi, applicazioni e con i principi descritti in [riduzione attiva Superficie di attacco directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md).  
  
Implementando le impostazioni di configurazione consigliata in una foresta appena creata, è possibile creare un'installazione di Active Directory Domain Services che viene compilata da zero con le impostazioni di sicurezza e procedure consigliate, ed è possibile ridurre i problemi relativi a supporto dei sistemi legacy e applicazioni. Mentre le istruzioni dettagliate per la progettazione e implementazione di un'installazione di Active Directory Domain Services originario esulano dall'ambito di questo documento, è necessario seguire alcuni principi generali e linee guida per creare una "cella sicura" in cui è possibile ospitare i più importanti asset. Queste linee guida sono i seguenti:  
  
1.  Identificare i principi per adeguatamente e protezione di asset critici.  
  
2.  Definire un piano di migrazione limitato, basati sui rischi.  
  
3.  Usare le migrazioni "nonmigratory" dove necessario.  
  
4.  Implementare "creativa distruzione di $."  
  
5.  Isolare le applicazioni e sistemi legacy.  
  
6.  Semplifica la sicurezza per gli utenti finali.  
  
### <a name="identifying-principles-for-segregating-and-securing-critical-assets"></a>Identificazione dei principi per adeguatamente e la protezione delle risorse critiche  

Le caratteristiche dell'ambiente originario che viene creata a asset critici house possono variare notevolmente. Ad esempio, è possibile scegliere di creare un insieme di strutture in cui si esegue la migrazione solo degli utenti VIP e dati sensibili che solo tali utenti possono accedere. È possibile creare un insieme di strutture in cui si esegue la migrazione non solo gli utenti VIP, ma che viene implementato come una foresta amministrativa, che implementa i principi descritti in [riducendo la superficie di attacco Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md) creare sicura gli account amministrativi e gli host che possono essere utilizzati per gestire le foreste legacy dalla foresta originario. È possibile implementare una "progettati su misura" foresta che ospita gli account VIP, gli account con privilegi e sistemi che richiedono protezione aggiuntiva, ad esempio i server che esegue Servizi certificati Active Directory (AD CS) con l'unico obiettivo di separandoli dagli meno sicure insiemi di strutture. Infine, è possibile implementare un insieme di strutture che diventa il percorso standard de facto per tutti i nuovi utenti, i sistemi, applicazioni e dati, che consente di ritirare infine la foresta tramite attrito legacy.  
  
Indipendentemente se l'insieme di strutture contiene un numero limitato di utenti e i sistemi o costituisce la base per una migrazione più aggressiva, si devono seguire questi principi durante la pianificazione:  
  
1.  Si supponga che le foreste legacy essere stata compromessa.  
  
2.  Non si configura un ambiente incontaminato e per rendere attendibile un insieme di strutture legacy, sebbene sia possibile configurare un ambiente legacy per considerare attendibile un insieme di strutture.  
  
3.  Non eseguire la migrazione degli account utente o i gruppi da una foresta legacy a un ambiente incontaminato e se esiste una possibilità che appartenenze ai gruppi degli account, la cronologia SID o altri attributi potrebbero essere state intenzionalmente modificati. Usare invece gli approcci "nonmigratory" per popolare un insieme di strutture. (Nonmigratory approcci sono descritti più avanti in questa sezione.)  
  
4.  Non eseguire la migrazione di computer nelle foreste legacy alle foreste originario. Implementano i server appena installati nell'insieme di strutture originario, installare applicazioni nei server appena installato e migrare i dati dell'applicazione per i sistemi appena installati. Per file server, copiare i dati per i server appena installati, impostare gli ACL tramite utenti e gruppi nella nuova foresta e quindi creare i server di stampa in modo analogo.  
  
5.  Non consentono l'installazione di sistemi operativi legacy o applicazioni nella foresta originario. Se un'applicazione non possa essere aggiornata e appena installata, lasciarla nella foresta legacy e prendere in considerazione la distruzione creativa per sostituire le funzionalità dell'applicazione.  
  
### <a name="defining-a-limited-risk-based-migration-plan"></a>Definizione di un piano di migrazione limitata, in base al rischio  
Creazione di una limitata, piano di migrazione basati sul rischio indica semplicemente che quando si decide quali utenti, applicazioni e i dati per eseguire la migrazione nella foresta originario, è necessario identificare le destinazioni di migrazione in base al grado di rischio a cui viene esposto l'organizzazione se uno di gli utenti o sistemi è compromessa. Gli utenti di VIP il cui account sono più probabilmente essere indirizzati da utenti malintenzionati devono trovarsi nella foresta originario. Le applicazioni che forniscono funzioni aziendali fondamentali devono essere installate nei server appena compilato nella foresta originario, e devono essere spostati dati altamente sensibili protetti server nella foresta originario.  
  
Se non hai già un quadro preciso degli utenti aziendali più importanti, i sistemi, applicazioni e i dati nell'ambiente Active Directory, utilizzano le business unit di identificarli. Tutte le applicazioni necessarie per l'azienda operi devono essere identificate come tutti i server in cui eseguire le applicazioni critiche o dati critici sono archiviati. Identificando gli utenti e le risorse necessarie per l'organizzazione continua a funzionare, è creare una raccolta naturalmente in ordine di priorità degli asset in cui si desidera concentrare l'attenzione.  
  
### <a name="leveraging-nonmigratory-migrations"></a>Sfruttando le migrazioni "Nonmigratory"  
Se si sa che l'ambiente sia stata compromessa, si sospetta che sia stata compromessa, o semplicemente preferisce non eseguire la migrazione di oggetti e dati legacy da un'installazione di Active Directory legacy a uno nuovo, prendere in considerazione gli approcci di migrazione che non si tratti tecnicamente oggetti "migrazione".  
  
### <a name="user-accounts"></a>User Accounts  
In una tradizionale Active Directory migrazione da una foresta a altra, l'attributo SIDHistory (cronologia SID) degli oggetti utente consente di archiviare i SID degli utenti e i SID dei gruppi che gli utenti sono membri nella foresta legacy. Se gli account utente vengono eseguita la migrazione a una nuova foresta e accedono alle risorse nella foresta legacy, i SID nella cronologia di SID vengono usate per creare un token di accesso che consente agli utenti di accedere alle risorse a cui che avevano accesso prima che gli account sono stati migrati.  
  
Gestione della cronologia SID, tuttavia, si è dimostrata difficili in alcuni ambienti perché popolare i token di accesso degli utenti con i SID attuali e cronologiche può comportare token bloat. Token bloat costituisce un problema in cui viene utilizzato il numero di SID che devono essere archiviati in un token di accesso o supera la quantità di spazio disponibile nel token.  
  
Anche se le dimensioni di token possono essere aumentate in modo limitato, la soluzione definitiva di token bloat consiste nel ridurre il numero di SID associati agli account utente, sia tramite la razionalizzazione appartenenze, eliminando la cronologia SID o una combinazione di entrambi. Per altre informazioni sui token bloat, vedere [MaxTokenSize e Bloat Token Kerberos](http://blogs.technet.com/b/shanecothran/archive/2010/07/16/maxtokensize-and-kerberos-token-bloat.aspx).  
  
Piuttosto che la migrazione degli utenti da un ambiente legacy (in particolare una in cui le appartenenze di gruppo e SID potrebbero risultare compromessa cronologie) con cronologia SID, si consideri di sfruttare metadirectory alle applicazioni di "migrazione" gli utenti, senza che trasportano le cronologie di SID nella nuova foresta. Quando gli account utente vengono creati nella nuova foresta, è possibile utilizzare un application metadirectory per mappare gli account per i propri account corrispondente nella foresta legacy.  
  
Per fornire il nuovo utente gli account l'accesso alle risorse nella foresta legacy, è possibile usare gli strumenti metadirectory per identificare i gruppi di risorse in cui gli account degli utenti legacy sono state concesso l'accesso e quindi aggiungere nuovi account degli utenti a tali gruppi. A seconda della strategia di gruppo nella foresta legacy, potrebbe essere necessario creare gruppi locali di dominio per l'accesso alle risorse o convertire gruppi esistenti ai gruppi locali di dominio per consentire i nuovi account da aggiungere ai gruppi di risorse. Primis nelle applicazioni più importanti e di dati ed eseguirne la migrazione nel nuovo ambiente (con o senza cronologia SID), è possibile limitare la quantità di impegno consumata nell'ambiente legacy.  
  

  
### <a name="servers-and-workstations"></a>Server e workstation  
In una migrazione tradizionale da un servizio Active Directory dell'insieme di strutture per la migrazione, un altro computer è spesso relativamente semplice rispetto alla migrazione di utenti, gruppi e applicazioni. A seconda del ruolo di computer, eseguire la migrazione a una nuova foresta può essere semplice come separare un dominio precedente e sull'aggiunta di uno nuovo. Tuttavia, migrando gli account computer intatti in compromette un insieme di strutture lo scopo della creazione di un nuovo ambiente. Invece di account di migrazione dei computer (potenzialmente compromesso, non configurato correttamente o non aggiornati) a una nuova foresta, è necessario installare appena server e workstation nel nuovo ambiente. È possibile migrare dati da sistemi nella foresta legacy per i sistemi di foreste, ma non i sistemi che ospitano i dati.  
  
### <a name="applications"></a>Applicazioni  

Le applicazioni possono presentare la sfida più significativa una migrazione da una foresta a altra, ma nel caso di una migrazione "nonmigratory", uno dei principi di base che è consigliabile applicare è che le applicazioni nella foresta originario devono essere aggiornate, supportato e appena installato. I dati possono essere migrati dalle istanze dell'applicazione nella foresta precedente laddove possibile. In situazioni in cui un'applicazione non può essere "ricreata" nell'insieme di strutture originario, è necessario considerare gli approcci, ad esempio creativa distruzione o l'isolamento delle applicazioni legacy, come descritto nella sezione seguente.  
  
### <a name="implementing-creative-destruction"></a>Implementazione di distruzione di $ creativa  
Distruzione di $ creativa è un termine di vantaggi economici che descrive lo sviluppo economico creato per l'eliminazione di un ordine precedente. Negli ultimi anni, il termine è stato applicato alle risorse informatiche. In genere fa riferimento ai meccanismi da quale infrastruttura vecchio viene eliminato, non eseguendo l'aggiornamento, ma sostituendola con qualcosa di completamente nuovo. Il 2011 [Gartner Symposium ITXPO](http://www.gartner.com/technology/symposium/orlando/) per CIO e responsabili IT senior presentati creativa distruzione come una delle relative chiavi dei temi per aumento e riduzione dei costi dell'efficienza. Miglioramenti di sicurezza possono essere eseguiti come una naturale outgrowth del processo.  

Ad esempio, un'organizzazione può essere il risultato di più business unit che usano un'altra applicazione che esegue una funzionalità simile, con diversi livelli di supporto modernity e fornitore. In passato, IT potrebbe essere responsabile della gestione dell'applicazione ogni business unit separatamente e consolidamento è costituita da tentativi di capire quale applicazione reso disponibile la funzionalità ottimo e quindi la migrazione dei dati in cui applicazione dagli altri.  
  
Distruzione di $ creativa, anziché di gestione di applicazioni obsolete o con ridondanza, implementare applicazioni completamente nuove per sostituire la vecchia, eseguire la migrazione dei dati nelle nuove applicazioni e rimuovere le precedenti applicazioni e i sistemi in cui vengono eseguite. In alcuni casi, è possibile implementare creativa distruzione delle applicazioni legacy tramite la distribuzione di una nuova applicazione nella propria infrastruttura, ma laddove possibile, è necessario considerare il porting invece l'applicazione a una soluzione basata sul cloud.  
  
Tramite la distribuzione di applicazioni basate sul cloud per sostituire le applicazioni interne legacy, si riduce non solo le attività di manutenzione e i costi, ma si riduce la superficie di attacco della propria organizzazione, eliminando i sistemi legacy e le applicazioni che presentano le vulnerabilità per gli utenti malintenzionati di sfruttare. Questo approccio offre un modo più veloce per un'organizzazione ottenere funzionalità desiderate eliminando contemporaneamente legacy destinazioni nell'infrastruttura. Anche se il principio di distruzione creativa non si applica a tutte le risorse IT, fornisce un'opzione praticabile spesso per eliminando i sistemi legacy e le applicazioni durante la distribuzione simultanea delle applicazioni basata sul cloud affidabile, sicure.  
  
### <a name="isolating-legacy-systems-and-applications"></a>Isolamento delle applicazioni e sistemi Legacy  
Un outgrowth naturale della migrazione gli utenti aziendali cruciali e sistemi in un ambiente originario e sicuro è che la foresta legacy conterrà meno utili informazioni e sistemi. Anche se i sistemi legacy e le applicazioni che rimangono nell'ambiente meno sicuro possono presentare con privilegi elevato rischio di compromissione, rappresentano anche un livello di gravità ridotto di compromissione. Grazie allo spostamento e modernizzare i beni aziendali critiche, è possibile concentrarsi sulla distribuzione di un'efficace gestione e monitoraggio durante la non necessità di supportare i protocolli e le impostazioni legacy.  
  
Quando si sono spostati i dati critici in un insieme di strutture, è possibile valutare le opzioni per isolare ulteriormente i sistemi legacy e le applicazioni nella foresta di Active Directory Domain Services "main". Anche se è possibile implementare la distruzione creativa per sostituire un'applicazione e i server in cui viene eseguito, in altri casi è possibile considerare un isolamento aggiuntivo dei sistemi e delle applicazioni meno sicura. Ad esempio, un'applicazione che viene usato da un numero limitato di utenti, ma che richiede credenziali legacy, ad esempio gli hash di LAN Manager possono essere migrate a un dominio di dimensioni ridotte è creare per supportare i sistemi per i quali non si dispone di alcuna opzione di sostituzione.  
  
Rimuovendo questi sistemi da domini in cui vengono forzate implementazione delle impostazioni legacy, è possibile aumentare la sicurezza dei domini successivamente tramite la configurazione in modo da supportare solo i sistemi operativi correnti e le applicazioni. Tuttavia, è preferibile rispetto alla rimozione delle autorizzazioni sistemi legacy e delle applicazioni laddove possibile. Se l'azzeramento semplicemente non è fattibile per un piccolo segmento della popolazione di legacy, adeguatamente, in un dominio separato (o foresta) consente di eseguire miglioramenti incrementali nel resto dell'installazione legacy.  
  
### <a name="simplifying-security-for-end-users"></a>Semplificazione della protezione per gli utenti finali  
Nella maggior parte delle organizzazioni, gli utenti che hanno accesso alle informazioni riservate a causa della natura dei propri ruoli all'interno dell'organizzazione spesso hanno la minima quantità di tempo da destinare all'apprendimento di controlli e le restrizioni di accesso complessi. Anche se si deve avere un programma di formazione complete di sicurezza per tutti gli utenti dell'organizzazione, è anche consigliabile concentrarsi sull'impostazione di sicurezza più semplice da utilizzare come possibili mediante l'implementazione di controlli che sono transparent e semplificando la principi a cui gli utenti rispettare.  
  
Ad esempio, è possibile definire un criterio in cui i dirigenti e altri indirizzi VIP sono necessari per usare workstation sicura per accedere ai dati sensibili e sistemi, consentendo loro di usare i propri altri dispositivi per accedere ai dati meno sensibili. Si tratta di un principio semplice da ricordare, ma è possibile implementare un numero di controlli di back-end per consentire di applicare l'approccio.  

È possibile usare [verifica del meccanismo di autenticazione](https://technet.microsoft.com/library/dd391847(v=WS.10).aspx) per consentire agli utenti di accedere ai dati sensibili solo se sono già eseguito l'accesso ai loro sistemi protetti utilizzando le smart card e utilizzare IPsec e le restrizioni di controllo per i diritti utente di sistemi da cui possano connettersi ai repository di dati sensibili. È possibile usare la [Microsoft Data Classification Toolkit](https://www.microsoft.com/download/details.aspx?id=27123) per compilare un'infrastruttura di classificazione file potente ed è possibile implementare [controllo dinamico degli accessi](http://blogs.technet.com/b/windowsserver/archive/2012/05/22/introduction-to-windows-server-2012-dynamic-access-control.aspx) per limitare l'accesso ai dati di base caratteristiche di un tentativo di accesso, traducendo le regole di business in controlli tecnici.  
  
Dal punto di vista dell'utente, l'accesso ai dati sensibili da un sistema protetto "Just-works" e provare a farlo da un sistema non protetto "semplicemente non". Tuttavia, dal punto di vista di monitoraggio e la gestione dell'ambiente, in che modo aiutano a creare modelli di comportamento identificabili in modo gli utenti accedere dati sensibili e sistemi, rendendo più semplice per il rilevamento di tentativi di accesso anomali.  
  
Negli ambienti in cui l'utente ha comportato resistenza alle password lunghe e complesse come i criteri password insufficienti, in particolare per gli utenti VIP, prendere in considerazione soluzioni alternative per l'autenticazione, se tramite smart card (che sono disponibili in un numero di fattori di forma e con funzionalità aggiuntive di rafforzare l'autenticazione), controlli biometrici, ad esempio i lettori a scorrere verso il dito o i dati di autenticazione protetta da attendibile chip modulo platform module (TPM) nei computer degli utenti. Anche se l'autenticazione a più fattori non impedisce gli attacchi contro il furto di credenziali se un computer è già compromesso, fornendo agli utenti i controlli di autenticazione facile da usare, è possibile assegnare più affidabile delle password per gli account degli utenti per i quali tradizionale utente i controlli di nome e la password sono difficili da gestire.  
