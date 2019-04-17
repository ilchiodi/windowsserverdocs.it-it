---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: Implementazione host amministrativi protetti
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 42be89311f11ecc6a967b8b40600b53311253b89
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="implementing-secure-administrative-hosts"></a>Implementazione host amministrativi protetti

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Host amministrativi protetti sono workstation o server che sono stati configurati in modo specifico per gli scopi di creazione di piattaforme sicure da cui gli account con privilegi possono eseguire attività amministrative in Active Directory o in sistemi appartenenti a un dominio, i controller di dominio e applicazioni in esecuzione su sistemi appartenenti a un dominio. "Account con privilegi" fa riferimento in questo caso, non solo per gli account che sono membri dei gruppi con più privilegi in Active Directory, ma per qualsiasi account che sono stati delegati diritti e autorizzazioni che consentono di eseguire le attività amministrative.  
  
Questi account potrebbero essere l'account di supporto tecnico che hanno la possibilità di reimpostare le password per la maggior parte degli utenti in un dominio, gli account utilizzati per amministrare i record DNS e zone o gli account utilizzati per la gestione della configurazione. Le funzionalità amministrative dedicati host amministrativi sicuri e non eseguono il software come applicazioni di posta elettronica, web browser o software di produttività, ad esempio Microsoft Office.  
  
Sebbene gli account e i gruppi "con più privilegi" conseguenza devono essere il più rigorosamente protetto, questo non elimina la necessità di proteggere qualsiasi account e gruppi di quali privilegi rispetto a quelli di utente standard per l'account è stata concessa.  
  
Un host amministrativo sicuro può essere una workstation dedicata che viene utilizzata solo per le attività amministrative, un server membro che esegue il ruolo del server Gateway Desktop remoto e in cui gli utenti IT connettersi a eseguire l'amministrazione degli host di destinazione o un server che esegue il ruolo Hyper-V e fornisce una macchina virtuale univoca per ogni utente IT da utilizzare per le attività amministrative. In molti ambienti, possono essere implementate combinazioni di tutti i tre approcci.  
  
Implementazione host amministrativi protetti richiede pianificazione e configurazione coerente con le dimensioni dell'organizzazione, procedure amministrative, desiderio di rischio e budget. Considerazioni e le opzioni per l'implementazione host amministrativi protetti fornite di seguito per poter utilizzare nello sviluppo di una strategia di amministrazione della propria organizzazione.  
  
## <a name="principles-for-creating-secure-administrative-hosts"></a>Principi per la creazione degli host amministrativi protetti  
Per proteggere efficacemente sistemi contro gli attacchi, alcuni principi generali devono prendere in considerazione:  
  
1.  È non necessario amministrare mai un sistema attendibile (ovvero, un server protetto, ad esempio un controller di dominio) da un host meno attendibile (vale a dire una workstation in cui non è protetto per lo stesso livello, come i sistemi gestiti).  
  
2.  È necessario non affidarsi a un singolo fattore di autenticazione durante l'esecuzione di attività con privilegi. vale a dire le combinazioni di nome e una password utente non devono essere considerate autenticazione accettabile perché solo un singolo fattore (qualcosa che conosci) è rappresentato. È necessario considerare in cui le credenziali sono generate e memorizzato nella cache o archiviate negli scenari di amministrazione.  
  
3.  Sebbene la maggior parte degli attacchi nel panorama delle minacce corrente sfruttano violazioni dannose e malware, non escludere sicurezza fisica per la progettazione e implementazione host amministrativi protetti.  
  
### <a name="account-configuration"></a>Configurazione dell'account  
Anche se l'organizzazione attualmente Usa smart card, è consigliabile implementare li per gli account con privilegi e degli host amministrativi protetti. Host amministrativi deve essere configurato per richiedere l'accesso con smart card per tutti gli account modificando l'impostazione seguente in un oggetto Criteri di gruppo collegati alle unità organizzative contenenti host amministrativi:  
  
**Computer Configurazione computer\Criteri\Impostazioni sicurezza\Criteri locali\Opzioni protezione\Accesso interattivo: Richiedi smart card**  
  
Questa impostazione richiede tutti gli accessi interattivi per utilizzare una smart card, indipendentemente dalla configurazione di un singolo account in Active Directory.  
  
È inoltre necessario configurare un host amministrativi protetti per consentire l'accesso solo da account autorizzati, che può essere configurato in:  
  
**Computer Configurazione computer\Criteri\Impostazioni sicurezza\Criteri locali\Opzioni sicurezza\Criteri Locali\assegnazione diritti utente**  
  
Questo concede interattivo (e, se appropriato, Servizi Desktop remoto) i diritti di accesso solo agli utenti autorizzati di host amministrativi sicuro.  
  
### <a name="physical-security"></a>Sicurezza fisica  
Per gli host amministrativi essere considerato attendibile, devono essere configurati e proteggere lo stesso livello, come i sistemi che gestiscono. La maggior parte dei consigli forniti [protezione controller di dominio dagli attacchi](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) sono applicabili anche agli host che vengono utilizzati per amministrare i controller di dominio e il database di Active Directory. Uno dei problemi di implementazione di sistemi amministrativi protetti nella maggior parte degli ambienti è che la sicurezza fisica può essere più difficile da implementare perché questi computer spesso si trovano in aree che non sono protette come server ospitato in centri dati, ad esempio desktop degli utenti amministrativi.  
  
Sicurezza fisica include il controllo di accesso fisico a host amministrativi. In un'organizzazione di piccole dimensioni, ciò significa che mantenere una workstation amministrativa dedicata che viene mantenuta bloccato in un ufficio o di un cassetto della scrivania quando non è in uso. In alternativa, questo può significare di quando è necessario eseguire l'amministrazione di Active Directory o controller di dominio, accedere al controller di dominio direttamente.  
  
Nelle organizzazioni di medie dimensioni, è possibile valutare di implementazione sicura amministrativi "jump server" che si trovano in una posizione protetta in un ufficio e vengono utilizzate quando è necessaria la gestione di Active Directory oppure controller di dominio. È anche possibile implementare workstation amministrative che sono bloccate in percorsi sicuri quando non è in uso, con o senza jump server.  
  
In organizzazioni di grandi dimensioni, è possibile distribuire server jump ospitate datacenter che forniscono accesso controllato esclusivamente per Active Directory. controller di dominio. e file, stampa o server applicazioni. Implementazione di un'architettura jump server è più probabile che includono una combinazione di server e workstation protette in ambienti di grandi dimensioni.  
  
Indipendentemente dalle dimensioni dell'organizzazione e la progettazione della tua host amministrativi, si consiglia di proteggere i computer fisici contro accessi non autorizzati o furto e deve utilizzare Crittografia unità BitLocker per crittografare e proteggere l'unità host amministrativi. Implementando BitLocker su host amministrativi, anche se un host viene rubato o i dischi vengono rimossi, è possibile assicurarsi che i dati nell'unità sono inaccessibili a utenti non autorizzati.  
  
### <a name="operating-system-versions-and-configuration"></a>Configurazione e le versioni del sistema operativo  
Tutti gli host amministrativi, se i server o workstation, deve eseguire il sistema operativo più recente in uso nell'organizzazione per i motivi descritti in precedenza in questo documento. Eseguendo i sistemi operativi correnti, il personale amministrativo dei vantaggi di nuove funzionalità di sicurezza, supporto completo del fornitore e funzionalità aggiuntive introdotte nel sistema operativo. Inoltre, quando si sta valutando un nuovo sistema operativo, mediante la distribuzione prima di host amministrativi, è necessario acquisire familiarità con le nuove funzionalità, impostazioni e i meccanismi di gestione offre, che possono essere sfruttate successivamente nella pianificazione della distribuzione più ampia del sistema operativo. Da allora, gli utenti più sofisticati all'interno dell'organizzazione verranno inoltre gli utenti che hanno familiarità con il nuovo sistema operativo e posizionati migliore per supportarla.  
  
### <a name="microsoft-security-configuration-wizard"></a>Configurazione guidata sicurezza Microsoft  
Se si implementano i jump server come parte della strategia di host amministrativi, utilizzare la configurazione guidata di sicurezza predefinite per configurare le impostazioni del firewall per ridurre la superficie di attacco del server, del Registro di sistema, controllo e del servizio. Quando le impostazioni di configurazione guidata impostazioni di configurazione sicurezza sono stati raccolti e configurate, le impostazioni possono essere convertite in un oggetto Criteri di gruppo viene utilizzato per applicare una configurazione di base coerente in tutti i server di collegamento. È possibile modificare ulteriormente l'oggetto Criteri di gruppo per implementare le impostazioni di sicurezza specifiche per jump server e possibile combinare tutte le impostazioni con le impostazioni di base aggiuntive estratte da Microsoft Security Compliance Manager.  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
Il [Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) è uno strumento disponibile gratuitamente che integra le configurazioni di protezione consigliate da Microsoft, in base alla configurazione di ruolo e versione del sistema operativo, e consente di raccogliere in un unico strumento e dell'interfaccia utente che può essere utilizzata per creare e configurare le impostazioni di sicurezza di base per i controller di dominio. Modelli di Microsoft Security Compliance Manager possono essere combinati con le impostazioni di configurazione guidata impostazioni di sicurezza per produrre le linee di base di configurazione completa per jump server distribuito e applicato da oggetti Criteri di gruppo distribuito in unità organizzative in quali jump server si trovano in Active Directory.  
  
> [!NOTE]  
> In questo modo, Microsoft Security Compliance Manager include le impostazioni specifiche per jump server o altri host amministrativi protetti, ma ancora Security Compliance Manager (SCM) consente di creare linee di base iniziale per gli host amministrativi. Tuttavia, per proteggere correttamente gli host, è consigliabile applicare le impostazioni di sicurezza aggiuntive appropriate per server e workstation altamente sicuri.  
  
### <a name="applocker"></a>AppLocker  
Host amministrativi e machinesshould virtuale essere configurati con script e lo strumento whitelists applicazioni tramite AppLocker o una restrizione dell'applicazione di terze parti. Tutte le applicazioni amministrative o l'utilità non conformi per proteggere le impostazioni devono essere aggiornate o sostituite con gli strumenti che è conforme alle procedure amministrative e di sviluppo protetto. Quando sono necessario strumenti nuovo o aggiuntivo in un host amministrativo, utilità e applicazioni devono essere testate attentamente e se gli strumenti sono adatto per la distribuzione di host amministrativi, può essere aggiunto al whitelists dei sistemi.  
  
### <a name="rdp-restrictions"></a>Restrizioni per RDP  
Anche se la configurazione specifica variano in base all'architettura dei sistemi di amministrazione, è necessario includere le restrizioni in cui gli account e i computer possono essere utilizzati per stabilire le connessioni Remote Desktop Protocol (RDP) per i sistemi gestiti, ad esempio tramite Gateway Desktop remoto (Gateway Desktop remoto) jump server per controllare l'accesso ai controller di dominio e gestiti altri sistemi da utenti autorizzati e sistemi.  
  
Si dovrebbe consentire gli accessi interattivi dagli utenti autorizzati e devono rimuovere o bloccare anche altri tipi di accesso che non sono necessarie per accedere al server.  
  
### <a name="patch-and-configuration-management"></a>Patch e la gestione della configurazione  
Le organizzazioni più piccole si basano sulle offerte, ad esempio Windows Update o [Windows Server Update Services](https://technet.microsoft.com/windowsserver/bb332157) (WSUS) per gestire la distribuzione degli aggiornamenti ai sistemi Windows, mentre le organizzazioni più grandi possono implementare patch e la configurazione software di gestione aziendale, ad esempio System Center Configuration Manager. Indipendentemente dai meccanismi di che utilizzare per distribuire gli aggiornamenti a popolazione workstation e server generale, è necessario considerare le distribuzioni separate per sistemi a protezione elevata, ad esempio i controller di dominio, le autorità di certificazione e host amministrativi. Per separare questi sistemi dall'infrastruttura di gestione generali, compromessi gli account di software o servizio di gestione, la compromissione non può essere facilmente esteso per i sistemi più sicuri nell'infrastruttura.  
  
Anche se non è necessario implementare i processi di aggiornamento manuale per i sistemi protetti, è necessario configurare un'infrastruttura separata per l'aggiornamento dei sistemi protetti. Anche in organizzazioni di dimensioni molto grandi, questa infrastruttura in genere implementabili tramite server WSUS dedicato e oggetti Criteri di gruppo per i sistemi protetti.  
  
### <a name="blocking-internet-access"></a>Blocco dell'accesso a Internet  
Host amministrativi deve essere consentito l'accesso a Internet, né dovrebbero avere la possibilità di esplorare intranet dell'organizzazione. Web browser e applicazioni simili devono essere consentite negli host amministrativi. È possibile bloccare l'accesso a Internet per gli host protetti tramite una combinazione di impostazioni del firewall perimetrale, la configurazione WFAS e configurazione del proxy "black hole" negli host protetto. È anche possibile utilizzare applicazione whitelist per impedire che i web browser viene usato su un host amministrativi.  
  
### <a name="virtualization"></a>Virtualizzazione  
Dove possibile, si consiglia di implementare le macchine virtuali come host amministrativi. Utilizza la virtualizzazione, è possibile creare per ogni utente amministrativi sistemi che vengono archiviati e gestiti centralmente e che può essere facilmente arrestato quando non è in uso, assicurandosi che le credenziali vengono rimossi active nei sistemi amministrativi. È anche possibile richiedere che virtuale host amministrativi vengono reimpostate uno snapshot iniziale dopo ogni uso, assicurarsi che le macchine virtuali rimangono invariate. Ulteriori informazioni sulle opzioni per la virtualizzazione di host amministrativi sono disponibili nella sezione seguente.  
  
## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>Esempio approcci all'implementazione di Secure host amministrativi  
Indipendentemente dalla modalità di progettazione e distribuire l'infrastruttura di host amministrativi, è necessario tenere presenti le indicazioni fornite in "Principi per la creazione di host amministrativi Secure" più indietro in questo argomento. Ciascuno dei metodi descritti di seguito vengono fornite informazioni generali su come è possibile separare "amministrazione" e sistemi "produttività" utilizzati dal personale IT. Sistemi di produttività sono computer che si avvalgono degli amministratori IT per controllare la posta elettronica, esplorare Internet e per l'utilizzo di software di produttività, ad esempio Microsoft Office. Sistemi amministrativi sono computer che sono protetti e dedicati per l'utilizzo dell'amministrazione quotidiana di un ambiente IT.  
  
Il modo più semplice per implementare host amministrativi protetti è fornire il personale IT con le workstation protette da cui possano eseguire attività amministrative. In un'implementazione solo workstation, ogni workstation amministrativa viene utilizzato per avviare gli strumenti di gestione e le connessioni RDP per gestire server e un'altra infrastruttura. Solo workstation implementazioni possono essere efficace nelle organizzazioni più piccole, ma più grandi e complesse infrastrutture possono beneficiare di una progettazione distribuita per gli host amministrativi in cui vengono utilizzate le workstation e server amministrativi dedicati, come descritto in "Implementazione Secure amministrativi workstation e Jump server" più avanti in questo argomento.  
  
### <a name="implementing-separate-physical-workstations"></a>Implementazione di workstation fisiche Separate  
Un modo che è possibile implementare host amministrativi consiste nel rilasciare ogni utente IT due workstation. Una workstation viene utilizzata con un account utente "normale" per eseguire attività come controllare la posta elettronica e tramite applicazioni di produttività, mentre la seconda workstation dedicata esclusivamente alle funzioni amministrative.  
  
Per la workstation di produttività, è possibile assegnare il personale IT normali account utente invece di usare gli account con privilegi per accedere al computer non protetti. Workstation amministrativa deve essere configurata con una configurazione rigorosamente controllata e il personale IT deve utilizzare un account diverso per accedere a una workstation amministrativa.  
  
Se è stato implementato smart card, le workstation amministrative devono essere configurate per richiedere accessi con smart card e il personale IT deve assegnare account distinti per l'utilizzo di amministrazione, anche configurato per richiedere smart card per l'accesso interattivo. L'host amministrativo deve essere protetto come descritto in precedenza e solo a determinati utenti IT devono essere consentiti accedere localmente a workstation amministrativa.  
  
#### <a name="pros"></a>Professionisti  
Mediante l'implementazione di sistemi fisici separati, è possibile garantire che ogni computer è configurato in modo appropriato per il proprio ruolo e che gli utenti IT non possono esporre involontariamente sistemi amministrativi a rischi.  
  
#### <a name="cons"></a>Contro  
  
-   Implementazione di computer fisici separati aumentano i costi di hardware.  
  
-   L'accesso a un computer fisico con le credenziali utilizzate per amministrare i sistemi remoti memorizza nella cache le credenziali in memoria.  
  
-   Se non sono archiviate in modo sicuro le workstation amministrative, potrebbero essere vulnerabile agli attacchi tramite meccanismi come keystroke logger hardware fisico o altri attacchi fisici.  
  
### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>Implementazione di una Workstation protetta fisica con una Workstation produttività virtualizzati  
Questo approccio, agli utenti IT viene fornita una workstation amministrativa protetta da cui possano eseguire funzioni amministrative quotidiane, tramite strumenti di amministrazione remota Server (RSAT) o connessioni RDP ai server nel proprio ambito di responsabilità. Quando gli utenti IT devono eseguire attività di produttività, si connettono tramite RDP a una workstation produttività remoto in esecuzione come macchina virtuale. Credenziali separate da utilizzare per ogni workstation e devono essere implementati controlli, ad esempio smart card.  
  
#### <a name="pros"></a>Professionisti  
  
-   Le workstation amministrative e workstation di produzione sono separate.  
  
-   IL personale IT usando workstation protette per connettersi alla workstation di produzione è possibile utilizzare credenziali separate e smart card e credenziali con privilegi non depositate nel computer meno sicuri.  
  
#### <a name="cons"></a>Contro  
  
-   Implementazione della soluzione richiede operazioni di implementazione di progettazione e opzioni di virtualizzazione affidabile.  
  
-   Se non sono archiviate in modo sicuro le workstation fisiche, potrebbero essere vulnerabile agli attacchi fisici che compromettono l'hardware o del sistema operativo e rendono vulnerabile a intercettazione delle comunicazioni.  
  
### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>Implementazione di una singola Workstation protetta con connessioni a separare "Produttività" e "Amministrazione" macchine virtuali  
Questo approccio, è possibile emettere gli utenti IT una singola workstation fisica che è bloccata come descritto in precedenza e in cui gli utenti IT non hanno accesso con privilegiato. È possibile fornire le connessioni di Servizi Desktop remoto per le macchine virtuali ospitate su server dedicati, fornendo il personale IT con una macchina virtuale che esegue l'e-mail e altre applicazioni di produttività e una seconda macchina virtuale configurata come host di amministrazione dedicato dell'utente.  
  
È consigliabile richiedere smart card o altro accesso a più fattori per le macchine virtuali, utilizzando separata di account diverso dall'account viene utilizzato per accedere al computer fisico. Dopo che un utente IT accede a un computer fisico, usano la produttività smart card per connettersi al computer remoto per la produttività e un account separato e smart card per connettersi al computer remoto amministrativa.  
  
#### <a name="pros"></a>Professionisti  
  
-   Gli utenti IT possono usare una singola workstation fisica.  
  
-   Per la richiesta di account separati per gli host virtuali e l'utilizzo di connessioni di Servizi Desktop remoto alle macchine virtuali, le credenziali degli utenti IT non vengono memorizzate nella cache nel computer locale.  
  
-   L'host fisico può essere protetti per lo stesso livello di host amministrativi, riducendo la probabilità di compromissione del computer locale.  
  
-   Nei casi in cui la macchina virtuale amministrativa o macchina virtuale di produttività dell'utente un IT è compromessa, la macchina virtuale possono essere reimpostata facilmente a uno stato "valida".  
  
-   Se il computer fisico è compromesso, credenziali con privilegi non verranno memorizzate nella cache in memoria e l'utilizzo di smart card può impedirne la compromissione delle credenziali da keystroke logger.  
  
#### <a name="cons"></a>Contro  
  
-   Implementazione della soluzione richiede operazioni di implementazione di progettazione e opzioni di virtualizzazione affidabile.  
  
-   Se non sono archiviate in modo sicuro le workstation fisiche, potrebbero essere vulnerabile agli attacchi fisici che compromettono l'hardware o del sistema operativo e rendono vulnerabile a intercettazione delle comunicazioni.  
  
### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>Implementazione di workstation amministrative protette e Jump server  
In alternativa, per proteggere le workstation amministrative oppure in combinazione con loro, è possibile implementare sicuro jump server e gli utenti amministrativi possono connettersi ai server jump tramite RDP e smart card per eseguire attività amministrative.  
  
Jump server deve essere configurato per eseguire il ruolo di Gateway Desktop remoto per consentire di implementare restrizioni per le connessioni per il jump server e ai server di destinazione che verranno gestiti da quest'ultimo. Se possibile, dovrà anche installare il ruolo Hyper-V e creare [desktop personali virtuali](https://technet.microsoft.com/library/dd759174.aspx) o altre macchine virtuali per ogni utente per gli utenti amministrativi da utilizzare per le attività sui jump server.  
  
Assegnando gli utenti amministratori a macchine virtuali per ogni utente nel jump server, è fornire sicurezza fisica per le workstation amministrative e gli utenti amministrativi possono reimpostare o arrestare le macchine virtuali quando non è in uso. Se non si desidera installare il ruolo Hyper-V e il ruolo di Gateway Desktop remoto nello stesso host amministrativi, è possibile installare in computer distinti.  
  
Se possibile, utilizzare strumenti di amministrazione remota per gestire i server. La funzionalità Strumenti di amministrazione remota Server (RSAT) deve essere installata su macchine virtuali degli utenti (o il jump server se non si implementa macchine virtuali per ogni utente per l'amministrazione) e il personale amministrativo deve connettersi tramite RDP per le macchine virtuali per eseguire attività amministrative.  
  
In casi quando un utente amministratore deve connettersi tramite RDP a un server di destinazione per gestirlo direttamente, Gateway Desktop remoto deve essere configurato per consentire la connessione solo se l'utente appropriato e il computer vengono utilizzati per stabilire la connessione al server di destinazione. Esecuzione di amministrazione remota del server (o simile) strumenti dovrebbero essere proibiti nei sistemi che non sono designati sistemi di gestione, ad esempio di uso generale workstation e server membri che sono non jump server.  
  
#### <a name="pros"></a>Professionisti  
  
-   Creazione di jump server consente di eseguire il mapping di server specifici a "zone" (raccolte di sistemi con requisiti di configurazione, connessione e protezione simili) della rete e per richiedere che l'amministrazione di ciascuna zona viene ottenuta dal personale amministrativo connessione da host amministrativi protetti a un server designato "zona".  
  
-   Eseguendo il mapping jump server per le zone, è possibile implementare controlli granulari per le proprietà di connessione e i requisiti di configurazione e identificare facilmente i tentativi di connessione dai sistemi non autorizzati.  
  
-   Mediante l'implementazione di macchine virtuali al privilegi di amministratore nel server di collegamento, applicare arresto e il ripristino delle macchine virtuali in uno stato pulito noto al termine delle attività amministrative. Applicando (arresto o riavvio) delle macchine virtuali al termine delle attività amministrative, le macchine virtuali non possono essere indirizzate da utenti malintenzionati, né sono gli attacchi contro il furto di credenziali fattibile perché le credenziali memorizzate nella cache di memoria non vengono mantenute oltre un riavvio.  
  
#### <a name="cons"></a>Contro  
  
-   Server dedicati sono necessarie per jump server, se fisico o virtuale.  
  
-   Implementazione designato jump server e workstation amministrative richiede un'attenta pianificazione e la configurazione che esegue il mapping per le aree di sicurezza configurate nell'ambiente.  
  


