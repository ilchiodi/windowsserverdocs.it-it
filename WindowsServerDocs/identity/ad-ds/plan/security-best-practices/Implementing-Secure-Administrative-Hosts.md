---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: Implementazione di host amministrativi protetti
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ed2ff7bfa0cc3b27506b1ca324e819860eef314c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890422"
---
# <a name="implementing-secure-administrative-hosts"></a>Implementazione di host amministrativi protetti

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Host amministrativi protetti sono workstation o server che sono state configurate in modo specifico ai fini della creazione di piattaforme sicure da cui gli account con privilegi possono eseguire attività amministrative in Active Directory o nei controller di dominio i sistemi appartenenti a un dominio e le applicazioni in esecuzione in sistemi appartenenti a un dominio. In questo caso, "account con privilegi" fa riferimento non solo agli account che sono membri dei gruppi con più privilegi in Active Directory, ma per qualsiasi account che sono stati delegati diritti e autorizzazioni che consentono le attività amministrative da eseguire.  
  
Questi account possono essere account dell'Help Desk che hanno la possibilità di reimpostare le password per la maggior parte degli utenti in un dominio, gli account usati per gestire zone e record DNS o gli account usati per la gestione della configurazione. Host amministrativi protetti sono dedicate alle funzionalità amministrative e non vengono eseguite applicazioni software come applicazioni di posta elettronica, browser web o software di produttività, ad esempio Microsoft Office.  
  
Anche se gli account e gruppi "con più privilegi" di conseguenza devono essere il più rigoroso protetto, ciò non elimina la necessità di proteggere qualsiasi account e gruppi ai quali privilegi superiori a quelli di utente standard gli account sono state concesse.  
  
Host amministrativi sicuro può essere una workstation dedicata che viene usata solo per attività amministrative, un server membro che esegue il ruolo del server Gateway Desktop remoto e IT gli utenti a cui connettersi per eseguire l'amministrazione di host di destinazione o un server che esegue il ruolo Hyper-V e fornisce una macchina virtuale univoca per ogni utente IT da utilizzare per le proprie attività amministrative. In molti ambienti, le combinazioni di tutti e tre gli approcci possono essere implementate.  
  
Implementazione host amministrativi protetti richiede pianificazione e configurazione che è coerente con le dimensioni dell'organizzazione, procedure amministrative, al desiderio di rischio e budget. Considerazioni e le opzioni per l'implementazione host amministrativi protetti sono incluse qui è possibile usare lo sviluppo di una strategia di amministrazione adatta alla propria organizzazione.  
  
## <a name="principles-for-creating-secure-administrative-hosts"></a>Principi per la creazione di host amministrativi protetti  
Per proteggere in modo efficace i sistemi dagli attacchi, alcuni principi generali devono prendere in considerazione:  
  
1.  È non necessario amministrare mai un sistema attendibile (vale a dire, un server protetto, ad esempio un controller di dominio) da un host considerato meno attendibile (vale a dire, una workstation in cui non è protetto per lo stesso grado i sistemi gestiti).  
  
2.  Non basarsi su un singolo fattore di autenticazione durante l'esecuzione di attività con privilegi. vale a dire, combinazioni di nome e una password utente non devono essere considerate accettabile autenticazione perché è rappresentato solo a fattore singolo (un elemento noto). È necessario considerare in cui le credenziali vengono generate e memorizzati nella cache o archiviate negli scenari di amministrazione.  
  
3.  Sebbene la maggior parte degli attacchi nel panorama delle minacce correnti sfruttano hacking dannose e malware, non omettere la protezione fisica durante la progettazione e implementazione host amministrativi protetti.  
  
### <a name="account-configuration"></a>Configurazione dell'account  
Anche se l'organizzazione non usa attualmente le smart card, è consigliabile implementare loro per gli account con privilegi e host amministrativi protetti. Host amministrativi deve essere configurato per richiedere l'accesso con smart card per tutti gli account modificando l'impostazione seguente in un oggetto Criteri di gruppo collegati alle unità organizzative contenenti host amministrativi:  
  
**Accesso al computer Configurazione computer\Criteri\Impostazioni protezione\Criteri locali\Opzioni di protezione\Accesso: Richiedi smart card**  
  
Questa impostazione richiede tutti gli accessi interattivi da usare una smart card, indipendentemente dalla configurazione in un singolo account in Active Directory.  
  
È inoltre opportuno configurare host amministrativi protetti in modo da consentire gli accessi solo da account autorizzati, che può essere configurato in:  
  
**Computer Configurazione computer\Criteri\Impostazioni sicurezza\Criteri locali\Opzioni sicurezza\Criteri Locali\assegnazione diritti utente**  
  
Ciò concede interattivo (e, laddove appropriato, Servizi Desktop remoto) diritti di accesso solo agli utenti autorizzati dell'host di amministrazione sicura.  
  
### <a name="physical-security"></a>Sicurezza fisica  
Per gli host amministrativi essere considerato attendibile, devono essere configurate e protette allo stesso livello come i sistemi che gestiscono. La maggior parte dei consigli forniti [proteggere i controller di dominio contro attacco](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) sono inoltre applicabili agli host che vengono usate per amministrare i controller di dominio e il database di Active Directory Domain Services. Una delle problematiche di implementazione della protezione dei sistemi amministrativi nella maggior parte degli ambienti è che la sicurezza fisica può essere più difficile da implementare perché questi computer spesso si trovano in aree che non siano più protette come server ospitato in Data Center, ad esempio desktop degli utenti amministrativi.  
  
Sicurezza fisica include il controllo di accesso fisico per host amministrativi. Nelle organizzazioni di piccole dimensioni, questo può significare che mantiene bloccata da una workstation amministrativa dedicata in cui viene mantenuto in un ufficio o un cassetto quando non è in uso. O può significare che quando è necessario eseguire l'amministrazione di Active Directory o i controller di dominio, si accede al controller di dominio direttamente.  
  
Nelle organizzazioni di medie dimensioni, è possibile implementare sicuro amministrativi "jump server" che si trovano in un percorso protetto in un ufficio e vengono usate quando è necessaria la gestione di Active Directory oppure controller di dominio. È anche possibile implementare le workstation amministrative che vengono bloccate in percorsi sicuri quando non è in uso, con o senza jump server.  
  
In organizzazioni di grandi dimensioni, è possibile distribuire ospitava datacenter jump server che forniscono un accesso rigidamente controllato a Active Directory. controller di dominio. e file, stampa o applicazione server. Implementazione di un'architettura jump server è più probabile che includano una combinazione di server e workstation sicure in ambienti di grandi dimensioni.  
  
Indipendentemente dalle dimensioni dell'organizzazione e la progettazione di host amministrativi, è necessario proteggere i computer fisici da accessi non autorizzati o furto e deve utilizzare Crittografia unità BitLocker per crittografare e proteggere le unità negli host amministrativi . Implementando BitLocker negli host amministrativi, anche se un host viene prelevato o vengono rimossi i relativi dischi, è possibile assicurarsi che i dati nell'unità sono inaccessibili a utenti non autorizzati.  
  
### <a name="operating-system-versions-and-configuration"></a>Configurazione e le versioni del sistema operativo  
Tutti gli host amministrativi, se i server o workstation, deve essere eseguito il sistema operativo più recente in uso nell'organizzazione per i motivi descritti in precedenza in questo documento. Tramite l'esecuzione di sistemi operativi correnti, il personale amministrativo trae vantaggio dalla nuova funzionalità di sicurezza, supporto del fornitore completo e funzionalità aggiuntive introdotte nel sistema operativo. Inoltre, quando si valuta un nuovo sistema operativo, distribuendola host primo di amministrazione, è necessario acquisire familiarità con le nuove funzionalità, impostazioni e i meccanismi di gestione offre, che successivamente possono essere sfruttate nella pianificazione distribuzione più ampia del sistema operativo. Da allora, agli utenti più esperti nell'organizzazione sarà anche gli utenti che hanno familiarità con il nuovo sistema operativo e la migliore spinta per supportarla.  
  
### <a name="microsoft-security-configuration-wizard"></a>Configurazione guidata impostazioni di sicurezza Microsoft  
Se si implementa jump server come parte della strategia di host amministrativi, utilizzare la configurazione guidata di sicurezza predefiniti per configurare le impostazioni del firewall per ridurre la superficie di attacco del server, del Registro di sistema, controllo e servizio. Quando le impostazioni di configurazione guidata di configurazione di sicurezza sono stati raccolti e configurate, le impostazioni possono essere convertite in un oggetto Criteri di gruppo che consente di applicare una configurazione di base coerente in tutti i server di salto. È possibile modificare ulteriormente l'oggetto Criteri di gruppo per implementare le impostazioni di sicurezza specifiche per jump server ed è possibile combinare tutte le impostazioni con le impostazioni di base aggiuntivi estratte da Microsoft Security Compliance Manager.  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
Il [Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) è uno strumento disponibile gratuitamente che integra le configurazioni di sicurezza consigliati da Microsoft, in base alla configurazione di versione e il ruolo del sistema operativo e le raccoglie in un singolo strumento e l'interfaccia utente che può essere utilizzata per creare e configurare le impostazioni di sicurezza della linea di base per i controller di dominio. I modelli di Microsoft Security Compliance Manager possono essere combinati con le impostazioni di configurazione guidata sicurezza per produrre le linee di base di configurazione completa per jump server distribuiti e applicati da distribuire in corrispondenza le unità organizzative in cui jump server sono oggetti Criteri di gruppo che si trova in Active Directory.  
  
> [!NOTE]  
> Al momento della stesura di questo articolo, Microsoft Security Compliance Manager non include impostazioni specifiche della jump server o altri host amministrativi protetti, ma è ancora utilizzabile Security Compliance Manager (SCM) per creare linee di base iniziale per l'amministrazione host. Per proteggere correttamente l'host, tuttavia, è consigliabile applicare le impostazioni di sicurezza aggiuntiva appropriate per server e workstation a sicurezza elevata.  
  
### <a name="applocker"></a>AppLocker  
Host amministrativi e machinesshould virtuale da configurati con uno script, lo strumento e illustra come consentire applicazioni tramite AppLocker o un software di restrizione dell'applicazione di terze parti. Eventuali applicazioni amministrative o l'utilità non conformi per proteggere le impostazioni devono essere aggiornate o sostituite con gli strumenti che è conforme alle procedure amministrative e sviluppo sicuro. Quando gli strumenti di nuovo o aggiuntivo sono necessaria in un host amministrativo, utilità e applicazioni è consigliabile testare completamente e se gli strumenti sono adatto per la distribuzione negli host amministrativi, può essere aggiunto alla base dei sistemi.  
  
### <a name="rdp-restrictions"></a>Restrizioni di RDP  
Anche se la configurazione specifica varia a seconda dell'architettura dei sistemi amministrativi, è necessario includere restrizioni in cui gli account e computer può essere usato per stabilire le connessioni Remote Desktop Protocol (RDP) per i sistemi gestiti, ad esempio l'uso di Gateway Desktop remoto (Gateway Desktop remoto) jump server per controllare l'accesso ai controller di dominio e altri sistemi gestiti dagli utenti autorizzati e i sistemi.  
  
Dovrebbe consentire gli accessi interattivi dagli utenti autorizzati e devi rimuovere o persino bloccare altri tipi di accesso che non sono necessari per l'accesso al server.  
  
### <a name="patch-and-configuration-management"></a>Gestione di patch e gestione della configurazione  
Le organizzazioni più piccole possono basarsi sulle offerte, ad esempio Windows Update o [Windows Server Update Services](https://technet.microsoft.com/windowsserver/bb332157) (WSUS) per gestire la distribuzione degli aggiornamenti nei sistemi Windows, mentre le organizzazioni più grandi possono implementare delle patch aziendali e software di Gestione configurazione, ad esempio System Center Configuration Manager. Indipendentemente dal fatto i meccanismi che consente di distribuire gli aggiornamenti al server generali e /della workstation, è opportuno considerare distribuzioni distinte per sistemi a protezione elevata, ad esempio i controller di dominio, le autorità di certificazione e host amministrativi. Memorizzando questi sistemi dall'infrastruttura di gestione generale, se gli account di software o servizio di gestione sono compromessi, la compromissione può essere esteso facilmente ai sistemi più sicuri dell'infrastruttura.  
  
Anche se non è necessario implementare i processi di aggiornamento manuale per la protezione dei sistemi, è necessario configurare un'infrastruttura separata per l'aggiornamento di sistemi protetti. Anche in organizzazioni di dimensioni molto grandi, questa infrastruttura può in genere essere implementata tramite il server WSUS dedicato e oggetti Criteri di gruppo per i sistemi protetti.  
  
### <a name="blocking-internet-access"></a>Blocca l'accesso a Internet  
Host amministrativi deve essere consentito l'accesso a Internet, né dovrebbero esserlo saper esplorare intranet dell'organizzazione. Web browser e applicazioni simili devono essere consentite negli host amministrativi. È possibile bloccare l'accesso a Internet per host protetti tramite una combinazione di impostazioni del firewall perimetrale, configurazione WFAS e configurazione del proxy "black hole" su host protetti. È anche possibile usare nell'elenco elementi consentiti dell'applicazione per impedire l'utilizzo negli host amministrativi di web browser.  
  
### <a name="virtualization"></a>Virtualizzazione  
Dove possibile, prendere in considerazione l'implementazione di macchine virtuali come host amministrativi. Mediante la virtualizzazione, è possibile creare per ogni utente amministrativi sistemi archiviati e gestiti centralmente e che possono essere facilmente arrestate quando non è in uso, assicurando che le credenziali non restano attive nei sistemi amministrativi. È anche possibile richiedere che virtuale host amministrativi vengono reimpostati a uno snapshot iniziale dopo ogni uso, assicurando che le macchine virtuali rimangono intatte. Altre informazioni sulle opzioni per la virtualizzazione dell'host amministrativi viene fornite nella sezione seguente.  
  
## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>Gli approcci di esempio per l'implementazione sicura host amministrativi  
Indipendentemente dalla modalità di progettazione e distribuzione dell'infrastruttura di host amministrativi, è necessario tenere presenti le linee guida fornite in "I principi per la creazione di host amministrativi Secure" più indietro in questo argomento. Ogni approccio descritto di seguito vengono fornite informazioni generali sul modo in cui è possibile separare "amministrativo" e i sistemi "productivity" usati dal personale IT. I sistemi di produttività sono computer che gli amministratori IT utilizzano per controllare l'indirizzo di posta elettronica, accedere a Internet e usare il software di produttività generali, ad esempio Microsoft Office. I sistemi amministrativi sono i computer protetti e dedicati per l'uso per l'amministrazione quotidiana di un ambiente IT.  
  
Il modo più semplice per implementare host amministrativi protetti è fornire il personale IT con workstation protette da cui è possibile eseguire attività amministrative. In un'implementazione solo workstation, ogni workstation amministrativa viene usato per avviare gli strumenti di gestione e le connessioni RDP a gestire server e altri elementi dell'infrastruttura. Workstation implementazioni con sola possono risultare efficaci in organizzazioni di piccole dimensioni, anche se infrastrutture più grandi e complesse possono trarre vantaggio da una progettazione distribuita per host amministrativi in cui dedicati server amministrativi e di utilizzo delle workstation, come descritto in "Implementazione Secure amministrativi workstation e Jump server" più avanti in questo argomento.  
  
### <a name="implementing-separate-physical-workstations"></a>Implementazione di workstation fisiche Separate  
Uno che è possibile implementare host amministrativi consiste nell'inviare ogni utente IT due workstation. Una workstation viene usata con un account utente "normale" per eseguire le attività, ad esempio controllare la posta elettronica e tramite applicazioni di produttività, mentre la seconda workstation è dedicata esclusivamente alle funzioni amministrative.  
  
Per la workstation di produttività, è possibile assegnare il personale IT gli account utente normali invece di usare gli account con privilegi per accedere al computer non protetti. La workstation amministrativa deve essere configurata con una configurazione rigorosamente controllata e il personale IT deve usare un account diverso per accedere alla workstation amministrative.  
  
Se è stato implementato le smart card, le workstation amministrative devono essere configurate per richiedere di accessi con smart card e personale IT deve assegnare account distinti destinati all'uso amministrativo, inoltre configurato per richiedere le smart card per l'accesso interattivo. L'host amministrativo deve essere protetti come descritto in precedenza, e solo determinati utenti IT devono essere consentiti di accedere localmente nella workstation amministrativa.  
  
#### <a name="pros"></a>Vantaggi  
Mediante l'implementazione di sistemi fisici separati, è possibile garantire che ogni computer sia configurato in modo appropriato per il proprio ruolo e che gli utenti IT non possono esporre involontariamente sistemi amministrativi a rischi.  
  
#### <a name="cons"></a>Svantaggi  
  
-   Implementazione di computer fisici distinti aumenta i costi hardware.  
  
-   L'accesso a un computer fisico con le credenziali utilizzate per amministrare i sistemi remoti memorizza nella cache le credenziali in memoria.  
  
-   Se non vengono archiviate in modo sicuro le workstation amministrative, potrebbero essere vulnerabile agli attacchi tramite meccanismi, ad esempio keystroke logger hardware fisico o altri attacchi fisici.  
  
### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>Implementazione di una Workstation sicura fisica con una Workstation di produttività virtualizzati  
Questo approccio, agli utenti IT viene fornita una workstation amministrativa protetta da cui si può eseguire funzioni amministrative quotidiane, usando strumenti di amministrazione remota Server (RSAT) o le connessioni RDP ai server nel proprio ambito di responsabilità. Quando gli utenti IT devono eseguire attività di produttività, possano connettersi tramite RDP a una workstation remota per la produttività in esecuzione come macchina virtuale. Usare credenziali separate per ogni workstation e i controlli, ad esempio le smart card devono essere implementati.  
  
#### <a name="pros"></a>Vantaggi  
  
-   Workstation per la produttività e le workstation amministrative sono separate.  
  
-   IL personale IT con workstation sicure per connettersi alla workstation per la produttività possono usare credenziali separate e smart card e credenziali con privilegi non vengono inserite nel computer meno sicuro.  
  
#### <a name="cons"></a>Svantaggi  
  
-   Implementazione della soluzione richiede la progettazione e opzioni di virtualizzazione affidabile e attività di implementazione.  
  
-   Se le workstation fisiche non vengono archiviate in modo sicuro, potrebbero essere vulnerabile agli attacchi fisici che compromettono l'hardware o sistema operativo e li rendono vulnerabili alle intercettazioni le comunicazioni.  
  
### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>Implementazione di una sola Workstation sicure con le connessioni per separare "Productivity" e "Amministrativo" le macchine virtuali  
Questo approccio, è possibile eseguire una sola workstation fisiche che è bloccato come descritto in precedenza, e in cui gli utenti IT non sono necessario l'accesso con privilegi di utenti IT. È possibile fornire le connessioni di Servizi Desktop remoto per macchine virtuali ospitate su server dedicati, fornendo il personale IT con una macchina virtuale che esegue l'indirizzo di posta elettronica e altre applicazioni di produttività e una seconda macchina virtuale configurata come l'utente host di amministrazione dedicato.  
  
È consigliabile richiedere smart card o altro accesso a più fattori per le macchine virtuali, usando account separati diverso dall'account usato per accedere al computer fisico. Dopo che un utente di IT accede a un computer fisico, usano la propria produttività smart card per la connessione al computer remoto per la produttività e un account separato e smart card per connettersi al proprio computer di amministrazione remota.  
  
#### <a name="pros"></a>Vantaggi  
  
-   Gli utenti IT possono usare una sola workstation fisica.  
  
-   Da che richiedono account separati per gli host virtuali e l'utilizzo di connessioni di Servizi Desktop remoto alle macchine virtuali, le credenziali degli utenti IT non vengono memorizzate nella cache nel computer locale.  
  
-   È possibile proteggere l'host fisico per lo stesso livello di host amministrativi, riducendo la probabilità di compromettere il computer locale.  
  
-   Nei casi in cui macchine virtuali per la produttività dell'utente un IT o amministrativi della relativa macchina virtuale possa essere stata compromessa, la macchina virtuale possono essere reimpostata facilmente a uno stato "valida".  
  
-   Se il computer fisico è compromesso, Nessuna credenziale con privilegi verranno memorizzata nella cache e l'uso di smart card può impedire il danneggiamento delle credenziali da keystroke logger.  
  
#### <a name="cons"></a>Svantaggi  
  
-   Implementazione della soluzione richiede la progettazione e opzioni di virtualizzazione affidabile e attività di implementazione.  
  
-   Se le workstation fisiche non vengono archiviate in modo sicuro, potrebbero essere vulnerabile agli attacchi fisici che compromettono l'hardware o sistema operativo e li rendono vulnerabili alle intercettazioni le comunicazioni.  
  
### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>Implementazione di workstation amministrative protette e Jump server  
Come alternativa alla sicurezza delle workstation amministrative o in combinazione con essi, è possibile implementare sicuro jump server, e gli utenti amministratori possono connettersi le jump server tramite RDP e smart card per eseguire attività amministrative.  
  
Jump server deve essere configurato per eseguire il ruolo Gateway Desktop remoto per consentire di implementare limitazioni per le connessioni al server di salto e ai server di destinazione che verranno gestiti da quest'ultimo. Se possibile, è necessario anche installare il ruolo Hyper-V e creare [desktop personali virtuali](https://technet.microsoft.com/library/dd759174.aspx) o altre macchine virtuali per utente per gli utenti amministrativi da utilizzare per le attività nella jump server.  
  
Assegnando gli utenti amministratori a macchine virtuali per utente nel jump server, è fornire la sicurezza fisica per le workstation amministrative e utenti con privilegi amministrativi possono reimpostare o arrestare le macchine virtuali quando non è in uso. Se si preferisce non installare il ruolo Hyper-V e il ruolo Gateway Desktop remoto nello stesso host amministrativi, è possibile installarli in computer separati.  
  
Laddove possibile, usare gli strumenti di amministrazione remota per gestire i server. La funzionalità Strumenti di amministrazione remota Server (RSAT) deve essere installata in macchine virtuali degli utenti (o il jump server se non si siano implementando le macchine virtuali per utente per l'amministrazione), mentre il personale amministrativo deve connettersi tramite RDP al loro macchine virtuali per eseguire attività amministrative.  
  
In casi quando un utente amministratore deve connettersi tramite RDP a un server di destinazione per gestirlo direttamente, Gateway Desktop remoto deve essere configurato per consentire la connessione a essere eseguita solo se l'utente appropriato e il computer vengono utilizzati per stabilire la connessione alla destinazione Server. L'esecuzione di amministrazione remota del server (o simile), ad esempio workstation di uso generale e i server membro che sono non jump server, strumenti nei sistemi che non sono designati sistemi di gestione, dovrebbero essere proibiti.  
  
#### <a name="pros"></a>Vantaggi  
  
-   Creazione jump server consente di eseguire il mapping di determinati server "zones" (raccolte di sistemi con requisiti di configurazione, connessione e sicurezza simili) nella rete e richiedere che l'amministrazione di ogni zona è ottenuto dal personale amministrativo la connessione da host amministrativi protetti a un server designato "zona".  
  
-   Eseguendo il mapping jump server alle aree, è possibile implementare controlli granulari per le proprietà di connessione e i requisiti di configurazione e puoi identificare con facilità i tentativi di connessione dai sistemi non autorizzati.  
  
-   Mediante l'implementazione di macchine virtuali per ogni amministratore nel server di collegamento, si applicano arresto e la reimpostazione delle macchine virtuali in uno stato noto di pulizia al termine delle attività amministrative. Tramite l'applicazione (arresto o riavvio) delle macchine virtuali quando vengono completate le attività amministrative, le macchine virtuali non può essere attaccate da utenti malintenzionati, né di attacchi con furto di credenziali fattibile perché le credenziali memorizzate nella cache di memoria non vengono mantenute di là un riavvio.  
  
#### <a name="cons"></a>Svantaggi  
  
-   Server dedicati sono necessari per jump server, fisica o virtuale.  
  
-   Implementazione designato jump server e workstation amministrative richiede un'attenta pianificazione e la configurazione che esegue il mapping a tutte le aree di sicurezza configurate nell'ambiente.  
  


