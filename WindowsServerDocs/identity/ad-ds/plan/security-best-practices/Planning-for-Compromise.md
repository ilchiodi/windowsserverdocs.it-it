---
ms.assetid: 6f50476c-a1f1-48fb-999b-76c4c3816496
title: Pianificazione di violazione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7cb87e6b9d1ace15050dcbff8b3c6d864c2a18f0
ms.sourcegitcommit: f2e98f8b7828730b83a3cc8f0e33d4e4db1b16e1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2018
---
# Pianificazione di violazione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Legge numero uno: Nessuno ritiene che nulla errate può verificarsi ad essi fino a quando non viene eseguita.* - [10 regole sempre valide dell'amministrazione della protezione](https://technet.microsoft.com/library/cc722488.aspx)  
  
Piani di ripristino in molte organizzazioni focalizzare l'attenzione sul ripristino di emergenza internazionali o errori che causano la perdita di servizi di elaborazione. Tuttavia, quando si lavora con i clienti compromessi, è spesso risultare il ripristino da violazione intenzionale assente in piani di ripristino. Questo è particolarmente vero quando il compromesso genera furto di proprietà intellettuale o l'eliminazione intenzionale che consente di sfruttare i confini logici (ad esempio eliminazione di tutti i domini di Active Directory o tutti i server) invece di confini fisici (ad esempio eliminazione di un centro dati). Sebbene un'organizzazione potrebbe avere piani di risposta che definiscono le attività iniziali dovrà eseguire quando viene rilevato un compromesso, questi piani omettono spesso procedura per correggere un problema che interessa l'intera infrastruttura di elaborazione.  
  
Poiché Active Directory fornisce potenti funzionalità di gestione delle identità e accesso per gli utenti, server, workstation e applicazioni, è sempre riservata da utenti malintenzionati. Se un hacker privilegi di accesso a un controller di dominio o il dominio di Active Directory, tale accesso può essere utilizzato per accedere, controllare o persino eliminare l'intera struttura di Active Directory.  
  
In questo documento è stati illustrati alcuni degli attacchi più comuni rispetto a Windows e Active Directory e contromisure è possibile implementare per ridurre i rischi, ma l'unico modo sicuro per recuperare in caso di violazione completa di Active Directory che deve essere Preparazione per la violazione prima che venga eseguito. In questa sezione vengono illustrate minore in dettagli tecnici di implementazione più sezioni precedenti di questo documento e più alto livello suggerimenti che è possibile utilizzare per creare un approccio globale e completo per proteggere e gestire l'organizzazione del critico Business e risorse IT.  
  
Se l'infrastruttura mai dell'attacco, ha resisted tentato violazioni, ha succumbed attacchi e stato completamente compromessa, è necessario pianificare la realtà inevitabile che riceverà un attacco più volte. Non è possibile impedire attacchi, ma effettivamente potrebbe essere possibile evitare che violazioni significative o compromesso ingrosso. Tutte le organizzazioni devono strettamente valutare la gestione dei rischi esistenti e apportare le modifiche necessarie per ridurre il livello complessivo di vulnerabilità effettuando investimenti non bilanciati prevenzione, rilevamento, contenimento e il ripristino.  
  
Per creare difesa efficace offrendo servizi agli utenti e aziende che dipendono dall'infrastruttura e applicazioni, potrebbe necessario da prendere in considerazione nuovi modi per evitare che, rilevare e contengono compromesso nel proprio ambiente e quindi recupera da compromesso. Gli approcci e suggerimenti in questo documento potrebbero non consentono di ripristinare l'installazione di Active Directory compromessa ma proteggere a quello successivo.  
  
Suggerimenti per il ripristino di un insieme di strutture di Active Directory vengono presentate in [Windows Server 2012: strategia di ripristino di strutture di Active Directory](https://www.microsoft.com/download/details.aspx?id=16506). È possibile impedire che il nuovo ambiente da completamente violazione, ma anche se non è possibile avere strumenti per recuperare e riprendere il controllo del proprio ambiente.  
  
## Riflessioni l'approccio  
*Legge numero otto: la difficoltà di difendere una rete è direttamente proporzionale la complessità.* - [10 regole sempre valide dell'amministrazione della protezione](https://technet.microsoft.com/library/cc722488.aspx)  
  
Si tratta in genere ben accettato che se un ottenuto sistema, amministratore, radice o accesso equivalente a un computer, indipendentemente dal sistema operativo, computer può non essere considerato attendibile, indipendentemente da quanti sforzi sono stati progettati per "pulire" il sistema. Active Directory vi è alcuna differenza. Se un ottenuto privilegi di accesso a un controller di dominio o un account con privilegi elevati in Active Directory, a meno che non si dispone di un record di ogni modifica apportata rende il pirata informatico o una copia di backup valida, sarà non possibile ripristinarli mai la directory da un completamente stato di idoneità.  
  
Quando un server membro o una workstation sia stato compromesso e modificata da un utente non autorizzato, il computer non è più attendibile, ma adiacenti server senza compromessi e arecompromise workstation di un computer non implica che tutti i computer vengano compromesse.  
  
Tuttavia, in un dominio di Active Directory, tutti i controller repliche dello stesso database di Active Directory. Se viene compromesso un controller di dominio e un pirata informatico modifica il database di Active Directory, tali modifiche replicano in tutti gli altri controller nel dominio e a seconda della partizione in cui le modifiche apportate, l'insieme di strutture. Anche se si reinstalla ogni controller di dominio dell'insieme di strutture, si sta reinstallando semplicemente hosts in cui si trova il database di Active Directory. Modifiche non autorizzate ad Active Directory verranno replicato facilmente verranno replicate in controller di dominio eseguito per gli anni in controller di dominio appena installate.  
  
Nel valutare ambienti compromessi, è in genere trovare che cosa era ritenuta il primo violazioni "evento" è stata effettivamente attivata dopo settimane, mesi o anni anche dopo che gli utenti malintenzionati avevano inizialmente sia stato compromesso l'ambiente. Gli utenti malintenzionati in genere ottenuto le credenziali per l'account con privilegi elevati tempo prima che è stato rilevato un violazioni e vengono utilizzati gli account per compromettere la directory controller di dominio, server membri, workstation e anche connessi non Windows sistemi.  
  
Questi risultati sono coerenti con diverse conclusioni 2012 dati violazioni indagini Report del Verizon, che indica che:  
  
-   percentuale 98 delle violazioni di dati giustificate dal agenti esterni  
  
-   85% delle violazioni di dati è stato svolto settimane o altri elementi per individuare  
  
-   92% delle interventi sono stati rilevati da terze parti e  
  
-   percentuale 97 violazioni della sono state evitabili se semplice o intermedi controlli.  
  
Un compromesso in misura descritto in precedenza è in modo efficace irreparabile e consigli standard "Unisci e ricreare" ogni sistema compromesso semplicemente non è possibile possibile o anche se Active Directory è stata compromessa o eliminato. Anche il ripristino di stato valido non elimina difetti consentiti ambiente compromessi nella prima posizione.  
  
Anche se è necessario difendere tutti i facet dell'infrastruttura, un pirata informatico è sufficiente trovare abbastanza difetti la protezione per ottenere l'obiettivo desiderato. Se l'ambiente è relativamente semplice e intatta e storicamente ben gestiti, implementazione i suggerimenti forniti in questo documento potrebbero essere in una soluzione semplice e immediata.  
  
Tuttavia, abbiamo scoperto che meno recenti, dimensioni maggiori e più complessi è suggerimenti riportati in questo documento sarà impossibile o addirittura impossibile implementare l'ambiente, più di frequente. È molto più difficile da proteggere un'infrastruttura dopo aver fatto superiore a quella per iniziare e per creare un ambiente agli attacchi e compromettere. Ma come sottolineato in precedenza, non piccola impresa ricostruzione dell'intera struttura di Active Directory. Per questi motivi, è consigliabile più attivo, mirato per proteggere l'insieme di strutture di Active Directory.  
  
Invece di concentrarsi e si tenta di correggere tutti i cambiamenti "interrotti", prendere in considerazione un approccio in cui è la priorità basato sugli elementi più importanti per le aziende e dell'infrastruttura. Anziché si tenta di aggiornare un ambiente piene di applicazioni e sistemi non aggiornati, non configurati correttamente, è consigliabile creare un nuovo ambiente ridotto e sicuro in cui è possibile trasferire in modo sicuro utenti, sistemi e informazioni più importanti per il Business.  
  
In questa sezione vengono descritte un approccio per il quale è possibile creare una strutture di dominio Active Directory che funge da un "barca vita_utile" o "sicura cella" per l'infrastruttura di business core. Un insieme di strutture è sufficiente un'appena installata foresta Active Directory che è in genere limitati in dimensioni e l'ambito e integrato con sistemi operativi attuali, applicazioni e con principi descritti in [riduzione Active Directory attacco Superficie](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md).  
  
Per implementare le impostazioni di configurazione consigliata un insieme di strutture appena creato, è possibile creare un'installazione di Active Directory creata da zero con le impostazioni di protezione e le procedure ed è possibile ridurre i problemi relativi al supporto di sistemi legacy e applicazioni. Mentre istruzioni dettagliate per la progettazione e implementazione di un'installazione precedente di Active Directory non rientrano nell'ambito di questo documento, è necessario seguire alcuni principi generali e linee guida per creare una "cella sicura" in cui è possibile ospitare il più importanti risorse. Alle linee guida seguenti sono i seguenti:  
  
1.  Identificare principi per separare e proteggere attività critiche.  
  
2.  Definire un piano di migrazione limitato, basati sui rischi.  
  
3.  Se necessario, sfruttare le migrazioni "nonmigratory".  
  
4.  Implementare "eliminazione creative."  
  
5.  Isolare applicazioni e sistemi legacy.  
  
6.  Semplificare la protezione per gli utenti finali.  
  
### Identificazione principi per separare e proteggere attività critiche  

Le caratteristiche dell'ambiente intatta creati alle risorse critiche zenzero possono variare molto. Ad esempio, è possibile creare un insieme di strutture in cui si esegue la migrazione solo gli utenti VIP e dati riservati che possono accedere solo gli utenti. È possibile creare un insieme di strutture in cui eseguire la migrazione VIP non solo gli utenti, ma che è implementare come un insieme di strutture amministrative implementazione principi descritti [riduzione della superficie di attacco Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md) per creare gli account amministrativi sicuri e a ospitare che possono essere usati per gestire l'insieme di strutture legacy dalle strutture. È possibile implementare un insieme di strutture "realizzati" in cui si trova VIP account account con privilegi e sistemi che richiedono rafforzare la sicurezza, ad esempio server che esegue Servizi certificati Active Directory (AD CS) con l'obiettivo esclusiva di separare loro da meno sicuro insiemi di strutture. Infine, è possibile implementare un insieme di strutture che diventa il percorso fatto per tutti i nuovi utenti, sistemi, applicazioni e dati, che consente di rimuovere alla fine della foresta legacy tramite attrito.  
  
Indipendentemente da se le strutture contiene un piccolo gruppo di utenti e sistemi oppure dei moduli di base per una migrazione più aggressiva, è necessario seguire questi principi della pianificazione:  
  
1.  Si supponga che l'insieme di strutture legacy sono stati compromessi.  
  
2.  Non si configura un ambiente privo di per considerare attendibile un insieme di strutture legacy, sebbene sia possibile configurare un ambiente legacy per considerare attendibile un insieme di strutture.  
  
3.  Non eseguire la migrazione di account utente o gruppi da una foresta legacy per un ambiente privo di se è possibile che le appartenenze degli account, cronologia SID o altri attributi potrebbero essere state da utenti malintenzionati modificate. Utilizzare invece "nonmigratory" approcci per popolare un insieme di strutture. (Nonmigratory approcci sono descritti più avanti in questa sezione.)  
  
4.  Non eseguire la migrazione computer da insiemi legacy per gli insiemi di strutture intatte. Implementare server appena installate nelle strutture, installare le applicazioni nel server appena installate ed eseguire la migrazione dei dati dell'applicazione in sistemi appena installati. Per i file server, copiare i dati in server appena installate, impostare ACL utilizzando utenti e gruppi nel nuovo insieme di strutture e quindi creare server di stampa in modo simile.  
  
5.  Non consentire l'installazione di sistemi operativi o applicazioni delle strutture. Se un'applicazione non è aggiornata e appena installata, lasciarla nella foresta legacy e valutare la possibilità di eliminazione creative per sostituire le funzionalità dell'applicazione.  
  
### Definizione di un piano di migrazione limitato, basati sui rischi  
Creare un'offerta, piano di migrazione basati sui rischi significa semplicemente che per decidere quali utenti, applicazioni e dati per eseguire la migrazione nelle strutture, è necessario identificare i siti di destinazione di migrazione in grado di rischio a cui l'organizzazione viene visualizzato se una base di gli utenti o i sistemi è compromessa. VIP gli utenti con account probabilmente da impostare come destinazione da utenti malintenzionati devono trovarsi nelle strutture. Applicazioni che offrono funzioni di business fondamentale devono essere installate nei server appena compilato le strutture e dati riservati devono essere spostati protetti server le strutture.  
  
Se non si dispone già di un'immagine Cancella degli utenti aziendali importanti, sistemi, applicazioni e dati nel proprio ambiente di Active Directory, utilizzano le business unit per l'identificazione. Qualsiasi applicazione necessari per le aziende a funzionare deve essere identificati, nonché eventuali server su cui eseguire applicazioni critiche o dati di importanza cruciale sono archiviati. Identificando gli utenti e risorse necessarie per l'organizzazione continuare a funzionare, si crea una raccolta naturalmente priorità dei beni su cui concentrare l'attenzione.  
  
### Utilizzo delle migrazioni "Nonmigratory"  
Se si conosce che l'ambiente sia stata compromessa, si ritiene che sia stata compromessa o semplicemente si preferisce non eseguire la migrazione di dati legacy e gli oggetti da un'installazione di Active Directory legacy a uno nuovo, è consigliabile approcci alla migrazione che non tecnicamente "eseguire la migrazione" oggetti.  
  
### Account utenti  
In una tradizionale Active Directory migrazione da un insieme di strutture a un altro, l'attributo SIDHistory (cronologia SID) degli oggetti utente viene usata per archiviare SID degli utenti e i SID di gruppi che gli utenti sono membri della foresta legacy. Se gli account utente vengono eseguiti in una nuova foresta e accedere alle risorse della foresta legacy, SID nella cronologia SID vengono utilizzati per creare un token di accesso che consente agli utenti di accedere alle risorse a cui che avevano accesso prima che l'account migrati.  
  
Mantenere la cronologia SID, tuttavia, ha dato prova problematica in alcuni ambienti perché la compilazione di token di accesso degli utenti con corrente e nella cronologia SID causando gonfiamento token. Gonfiamento token è un problema in cui viene utilizzato il numero inclusi devono essere archiviati in token di accesso dell'utente o supera la quantità di spazio disponibile nel token.  
  
Sebbene sia possibile aumentare dimensioni dei token, nei limiti, la soluzione ideale per gonfiamento token consiste nel ridurre il numero di SID associati agli account utente, se la razionalizzazione appartenenze ai gruppi, eliminare la cronologia SID o una combinazione di entrambi. Per ulteriori informazioni su gonfiamento token, vedere [MaxTokenSize e gonfiamento Token Kerberos](http://blogs.technet.com/b/shanecothran/archive/2010/07/16/maxtokensize-and-kerberos-token-bloat.aspx).  
  
Invece di migrazione degli utenti da un ambiente legacy (particolarmente uno appartenenze e SID possono essere compromessa cronologie) utilizzando la cronologia SID, valutare la possibilità di utilizzo metadirectory convertire le applicazioni di "" utenti, senza eseguire le cronologie SID in nuovo insieme di strutture. Quando vengono creati account utente nel nuovo insieme di strutture, è possibile utilizzare un'applicazione di metadirectory per mappare gli account per il proprio account corrispondente in foresta legacy.  
  
Per fornire il nuovo account l'accesso alle risorse foresta legacy, è possibile utilizzare gli utensili metadirectory per identificare i gruppi di risorse in cui gli account degli utenti legacy sono concesso l'accesso e quindi aggiungere nuovi account degli utenti ai gruppi. A seconda la strategia di gruppo della foresta legacy, potrebbe essere necessario creare gruppi locali per accedere alle risorse o convertire gruppi esistenti in gruppi locali per consentire i nuovi account da aggiungere a gruppi di risorse. Concentrarsi innanzitutto le applicazioni più importanti e i dati e la loro migrazione nel nuovo ambiente (con o senza cronologia SID), è possibile limitare la quantità di lavoro svolte in ambiente precedente.  
  

  
### Server e workstation  
In una tradizionale migrazione da un servizio Active Directory foresta nel computer di un'altra, eseguire la migrazione è spesso relativamente semplice confrontate con quelle di migrazione di utenti, gruppi e le applicazioni. In base al ruolo del computer, eseguire la migrazione di una nuova foresta può essere semplice come separare un dominio precedente e partecipare a una nuova. Tuttavia, la migrazione di account computer intatto in un vanifica strutture al fine di creare un ambiente aggiornato. Invece di eseguire la migrazione (potenzialmente compromesso non configurati correttamente computer o non aggiornate) di account in una nuova foresta, è necessario installare appena server e workstation nel nuovo ambiente. È possibile migrare dati da sistemi di foresta legacy per i sistemi di strutture, ma non i sistemi di tale zenzero i dati.  
  
### Applicazioni  

Applicazioni possono presentare il problema più significativo in qualsiasi migrazione da un insieme di strutture a un altro, ma in caso di una migrazione "nonmigratory", uno dei principi di base, è necessario applicare è che devono essere corrente, le applicazioni nelle strutture supportati e appena installati. Se possibile, è possono migrare dati da istanze dell'applicazione della foresta precedente. In situazioni in cui un'applicazione non è possibile "ricreare" nelle strutture, è necessario considerare approcci, ad esempio eliminazione creative o isolamento di applicazioni legacy, come descritto nella sezione seguente.  
  
### Eliminazione di spazio alla creatività implementazione  
Eliminazione Creative è un termine economico che descriva sviluppo economico autore l'eliminazione di un ordine precedente. Negli ultimi anni, il termine è stato applicato alla tecnologia di informazioni. In genere fa riferimento a meccanismi per il vecchio infrastruttura viene eliminato, non eseguendo l'aggiornamento ma sostituendolo con qualcosa di completamente nuovo. Il 2011 [Gartner simposio ITXPO](http://www.gartner.com/technology/symposium/orlando/) per CIO e dirigenti IT presentati creative eliminazione in uno dei relativi temi chiavi per la riduzione dei costi e aumenta l'efficienza. Miglioramenti della protezione sono possibili come un outgrowth naturale del processo.  

Ad esempio un'organizzazione può essere composto diverse business unit con un'altra applicazione che esegue una funzionalità simile, con diversi livelli di supporto modernity e fornitore. Storicamente, IT potrebbe essere responsabile della gestione dell'applicazione di ogni business unit separatamente e consolidamento è costituita da si tenta di stabilire quale applicazione offerto funzionalità più e quindi la migrazione dei dati in cui applicazione dagli altri.  
  
Eliminazione creative, anziché la gestione delle applicazioni non aggiornate o ridondanti, implementare applicazioni completamente nuove per sostituire il vecchio, eseguire la migrazione dei dati in nuove applicazioni e rimuovere le applicazioni precedente e i sistemi in cui vengono eseguite. In alcuni casi, è possibile implementare creative eliminazione di applicazioni legacy distribuendo una nuova applicazione di un'infrastruttura, ma se possibile, è necessario prendere in considerazione portabilità invece di un'applicazione di una soluzione basata su cloud.  
  
Per la distribuzione delle applicazioni basate su cloud per sostituire legacy applicazioni interne, si riduce non solo attività di manutenzione dei servizi e dei costi, ma ridurre i rischi di sicurezza dell'organizzazione eliminando sistemi e applicazioni che presentano vulnerabilità per gli utenti malintenzionati per utilizzare al meglio. Questo approccio offre un modo più veloce per un'organizzazione ottenere funzionalità desiderate eliminando contemporaneamente legacy destinazioni dell'infrastruttura. Sebbene seguire il principio dei privilegi eliminazione creative non si applica a tutte le risorse IT, offre un'opzione spesso valida per l'eliminazione di sistemi e applicazioni durante contemporaneamente la distribuzione di applicazioni affidabili, sicure, basato sul cloud.  
  
### Isolamento sistemi Legacy e applicazioni  
Un outgrowth naturale di migrazione degli utenti aziendali più importanti e sistemi a un ambiente protetto intatta è che la foresta legacy conterrà informazioni meno importanti e sistemi. Sebbene i sistemi legacy e le applicazioni che rimangono nell'ambiente meno sicuro possono presentare rischi elevati di violazione, rappresentano compromesso ridotte gravità. Da spostare e modernizzare i propri beni aziendali importanti, è possibile concentrarsi sulla distribuzione gestione efficace e il monitoraggio mentre non che sia necessario specificarlo adattato protocolli e le impostazioni precedenti.  
  
Quando si sono spostate dati critici a un insieme di strutture, è possibile valutare le opzioni per isolare ulteriormente i sistemi legacy e applicazioni nell'insieme di strutture di Active Directory "principale". Anche se è possibile implementare creative eliminazione per sostituire un'applicazione e i server in cui è in esecuzione, in altri casi è consigliabile isolamento aggiuntivi dei sistemi e applicazioni meno sicuro. Ad esempio, un'applicazione che viene utilizzato un piccolo gruppo di utenti, ma che richiede le credenziali legacy come hash LAN Manager possono eseguire la migrazione di un dominio small create per il supporto di sistemi per cui non si dispone le opzioni di sostituzione.  
  
Rimuovendo i sistemi di domini in cui sono forzata implementazione delle impostazioni precedenti, è possibile aumentare la sicurezza dei domini successivamente configurandole per supportare solo correnti sistemi operativi e applicazioni. Tuttavia, è preferibile rimuovere le autorizzazioni di sistemi legacy e le applicazioni quando possibile. Se disattivazione semplicemente non è possibile per un piccolo segmento la popolazione legacy, separare in un dominio distinto (o foresta) consente di eseguire miglioramenti incrementali il resto dell'installazione legacy.  
  
### Semplificazione di sicurezza per gli utenti finali  
Nella maggior parte delle organizzazioni, gli utenti autorizzati ad accedere a informazioni riservate per le caratteristiche dei loro ruoli nell'organizzazione spesso dispongono almeno quantità di tempo da destinare a familiarizzare con i controlli e le limitazioni di accesso complessi. Anche se non si un programma per l'istruzione di sicurezza completo per tutti gli utenti dell'organizzazione, è necessario anche lo stato attivo sulla protezione semplice da utilizzare come possibile mediante l'implementazione di controlli che fanno trasparenti e semplificando principi a cui gli utenti aderire.  
  
Ad esempio, è possibile definire un criterio in cui dirigenti e altri VIP devono utilizzare workstation protette per accedere ai dati riservati e sistemi, in modo da usare altri dispositivi per accedere ai dati meno sensibili. Si tratta di un principio semplice da ricordare, ma è possibile implementare un numero di controlli di back-end utili per applicare l'approccio.  

È possibile utilizzare [La funzionalità di verifica](https://technet.microsoft.com/library/dd391847(v=WS.10).aspx) per consentire agli utenti di accedere ai dati sensibili solo se è connesso a sistemi protetti utilizzando le smart card e possibile IPsec e utente restrizioni di diritti d'uso per controllare i sistemi da cui possono connettersi a archivi dati riservati. È possibile utilizzare il [Toolkit di classificazione dati Microsoft](https://www.microsoft.com/download/details.aspx?id=27123) per creare un'infrastruttura di classificazione file efficace ed è possibile implementare [Controllo dinamico degli accessi](http://blogs.technet.com/b/windowsserver/archive/2012/05/22/introduction-to-windows-server-2012-dynamic-access-control.aspx) per limitare l'accesso ai dati in base alle caratteristiche di un tentativo di accesso, la traduzione regole di business in controlli tecnici.  
  
Dal punto di vista dell'utente, accesso ai dati sensibili da un sistema protetto "funziona solo" e si tenta di farlo anche da un sistema non protetto "non appena viene." Tuttavia, dal punto di vista di monitoraggio e la gestione dell'ambiente, si offre assistenza per creare modelli identificabili come utenti accede dati riservati e sistemi, semplificando la rilevare tentativi di accesso anomala.  
  
Negli ambienti in cui l'utente ha generato vulnerabilità password lungo e complesso criteri password insufficiente, in particolare per gli utenti VIP, prendere in considerazione le soluzioni alternative per l'autenticazione, tramite smart card (che possono essere un numero di fattori di forma e con funzionalità aggiuntive per rafforzare autenticazione), biometrici controlli, ad esempio i lettori di scorrere il dito o dati di autenticazione pari protetto da attendibili modulo piattaforma (TPM) chip nei computer degli utenti. Anche se l'autenticazione non attacchi credenziali furto se un computer è già compromessa, assegnando agli utenti i controlli di autenticazione da usare, è possibile assegnare password più efficaci per gli account degli utenti per i quali tradizionale utente controlli nome e la password sono difficile da gestire.  
