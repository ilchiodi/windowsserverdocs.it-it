---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: Implementazione degli host amministrativi protetti
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 241418b87fd0f1e6fa64c4b1267e6d9fcde6b0d3
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822764"
---
# <a name="implementing-secure-administrative-hosts"></a>Implementazione degli host amministrativi protetti

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gli host amministrativi protetti sono workstation o server configurati in modo specifico allo scopo di creare piattaforme sicure da cui gli account con privilegi possono eseguire attività amministrative in Active Directory o nei controller di dominio, sistemi aggiunti a un dominio e applicazioni in esecuzione su sistemi aggiunti a un dominio. In questo caso, "account con privilegi" si riferisce non solo agli account membri dei gruppi con privilegi più elevati in Active Directory, ma a tutti gli account a cui sono stati delegati diritti e autorizzazioni che consentono l'esecuzione di attività amministrative.  
  
Questi account possono essere account del supporto tecnico che possono reimpostare le password per la maggior parte degli utenti di un dominio, account utilizzati per amministrare i record DNS e le zone o gli account utilizzati per la gestione della configurazione. Gli host amministrativi protetti sono dedicati alle funzionalità amministrative e non eseguono software come applicazioni di posta elettronica, Web browser o software di produttività, ad esempio Microsoft Office.  
  
Sebbene i gruppi e gli account "con privilegi più elevati" siano di conseguenza i più rigorosamente protetti, questo non elimina la necessità di proteggere gli account e i gruppi a cui sono stati concessi i privilegi superiori a quelli degli account utente standard.  
  
Un host amministrativo sicuro può essere una workstation dedicata utilizzata solo per le attività amministrative, un server membro che esegue il ruolo del server Gateway Desktop remoto e a cui gli utenti si connettono per eseguire l'amministrazione degli host di destinazione o un server che esegue il ruolo Hyper-V e fornisce una macchina virtuale univoca per ogni utente IT da usare per le attività amministrative. In molti ambienti è possibile che vengano implementate combinazioni di tutti e tre gli approcci.  
  
L'implementazione di host amministrativi protetti richiede la pianificazione e la configurazione coerenti con le dimensioni dell'organizzazione, le procedure amministrative, l'appetito per il rischio e il budget. Le considerazioni e le opzioni per l'implementazione di host amministrativi sicuri sono disponibili qui per l'utilizzo nello sviluppo di una strategia amministrativa appropriata per l'organizzazione.  
  
## <a name="principles-for-creating-secure-administrative-hosts"></a>Principi per la creazione di host amministrativi protetti  
Per proteggere efficacemente i sistemi dagli attacchi, è necessario tenere presenti alcuni principi generali:  
  
1.  Non è mai consigliabile amministrare un sistema attendibile, ovvero un server protetto, ad esempio un controller di dominio, da un host meno attendibile, ovvero una workstation non protetta allo stesso livello dei sistemi gestiti.  
  
2.  Quando si eseguono attività con privilegi, è consigliabile non basarsi su un singolo fattore di autenticazione. Ciò significa che le combinazioni di nome utente e password non devono essere considerate come autenticazione accettabile perché è rappresentato un solo fattore (un elemento noto). È necessario considerare dove le credenziali vengono generate e memorizzate nella cache o archiviate in scenari amministrativi.  
  
3.  Sebbene la maggior parte degli attacchi nell'attuale panorama delle minacce sfrutti malware e attacchi di pirateria dannosa, non omettere la sicurezza fisica durante la progettazione e l'implementazione di host amministrativi protetti.  
  
### <a name="account-configuration"></a>Configurazione account  
Anche se l'organizzazione attualmente non usa le smart card, è consigliabile implementarle per gli account con privilegi e gli host amministrativi protetti. Gli host amministrativi devono essere configurati in modo da richiedere l'accesso tramite smart card per tutti gli account modificando l'impostazione seguente in un oggetto Criteri di gruppo collegato alle unità organizzative contenenti gli host amministrativi:  
  
**Computer Computer\criteri\impostazioni protezione\Criteri locali\Opzioni Options\Interactive login: Richiedi smart card**  
  
Questa impostazione richiede che tutti gli accessi interattivi usino una smart card, indipendentemente dalla configurazione di un singolo account in Active Directory.  
  
È inoltre necessario configurare gli host amministrativi protetti per consentire gli accessi solo da account autorizzati, che possono essere configurati in:  
  
**Computer Computer\criteri\impostazioni protezione\Criteri locali\Opzioni protezione\Criteri locali\Assegnazione Rights Assignment**  
  
Questo consente di concedere i diritti di accesso interattivi (e, ove appropriato, Servizi Desktop remoto) solo agli utenti autorizzati dell'host amministrativo protetto.  
  
### <a name="physical-security"></a>Sicurezza fisica  
Affinché gli host amministrativi siano considerati attendibili, devono essere configurati e protetti allo stesso grado dei sistemi gestiti. La maggior parte delle raccomandazioni fornite nella [protezione dei controller di dominio dagli attacchi](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) è applicabile anche agli host utilizzati per amministrare i controller di dominio e il database di servizi di dominio Active Directory. Una delle difficoltà legate all'implementazione di sistemi amministrativi protetti nella maggior parte degli ambienti è che la sicurezza fisica può essere più difficile da implementare perché questi computer si trovano spesso in aree non sicure come i server ospitati nei data center, ad esempio desktop degli utenti amministratori.  
  
La sicurezza fisica include il controllo dell'accesso fisico agli host amministrativi. In un'organizzazione di piccole dimensioni questo potrebbe significare che si mantiene una workstation amministrativa dedicata mantenuta bloccata in un ufficio o un cassetto della scrivania quando non è in uso. In alternativa, quando è necessario eseguire l'amministrazione di Active Directory o dei controller di dominio, è possibile accedere direttamente al controller di dominio.  
  
Nelle organizzazioni di medie dimensioni, è possibile prendere in considerazione l'implementazione di "Jump server" amministrativi protetti che si trovano in un percorso protetto in un ufficio e vengono usati quando è necessario gestire Active Directory o controller di dominio. È anche possibile implementare workstation amministrative bloccate in posizioni sicure quando non sono in uso, con o senza Jump server.  
  
Nelle organizzazioni di grandi dimensioni, è possibile distribuire server di salto Data Center che forniscono accesso rigorosamente controllato a Active Directory; controller di dominio; e server di file, di stampa o di applicazioni. L'implementazione di un'architettura di Jump server è più probabile che includa una combinazione di workstation e server sicuri in ambienti di grandi dimensioni.  
  
Indipendentemente dalle dimensioni dell'organizzazione e dalla progettazione degli host amministrativi, è consigliabile proteggere i computer fisici da accessi non autorizzati o furti e utilizzare Crittografia unità BitLocker per crittografare e proteggere le unità negli host amministrativi. . Implementando BitLocker negli host amministrativi, anche in caso di furto di un host o di rimozione dei relativi dischi, è possibile verificare che i dati dell'unità non siano accessibili agli utenti non autorizzati.  
  
### <a name="operating-system-versions-and-configuration"></a>Configurazione e versioni del sistema operativo  
Tutti gli host amministrativi, se server o workstation, devono eseguire il sistema operativo più recente in uso nell'organizzazione per i motivi descritti in precedenza in questo documento. Eseguendo i sistemi operativi correnti, il personale amministrativo trae vantaggio dalle nuove funzionalità di sicurezza, dal supporto completo del fornitore e dalla funzionalità aggiuntiva introdotta nel sistema operativo. Inoltre, quando si sta valutando un nuovo sistema operativo, distribuendo prima di tutto gli host amministrativi, sarà necessario acquisire familiarità con le nuove funzionalità, le impostazioni e i meccanismi di gestione offerti, che possono essere utilizzati in seguito nella pianificazione distribuzione più ampia del sistema operativo. A questo punto, gli utenti più sofisticati dell'organizzazione saranno anche gli utenti che hanno familiarità con il nuovo sistema operativo e con il migliore posizionamento per supportarla.  
  
### <a name="microsoft-security-configuration-wizard"></a>Configurazione guidata impostazioni di sicurezza Microsoft  
Se si implementano Jump server come parte della strategia host di amministrazione, è consigliabile utilizzare la configurazione guidata impostazioni di sicurezza predefinita per configurare le impostazioni di servizio, registro di sistema, controllo e firewall per ridurre la superficie di attacco del server. Quando le impostazioni di configurazione della configurazione guidata impostazioni di sicurezza sono state raccolte e configurate, le impostazioni possono essere convertite in un oggetto Criteri di gruppo usato per applicare una configurazione di base coerente in tutti i server Jump. È possibile modificare ulteriormente l'oggetto Criteri di gruppo per implementare impostazioni di sicurezza specifiche per Jump server e combinare tutte le impostazioni con impostazioni di base aggiuntive estratte da Microsoft Security Compliance Manager.  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
[Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) è uno strumento disponibile gratuitamente che integra le configurazioni di sicurezza consigliate da Microsoft, in base alla versione del sistema operativo e alla configurazione del ruolo, e le raccoglie in un unico strumento e interfaccia utente che può essere usata per creare e configurare le impostazioni di sicurezza di base per i controller di dominio. È possibile combinare i modelli di Microsoft Security Compliance Manager con le impostazioni della configurazione guidata impostazioni di sicurezza per produrre linee di base di configurazione complete per i server Jump distribuiti e applicati da oggetti Criteri di gruppo distribuiti nelle unità organizzative in cui i server Jump sono si trova in Active Directory.  
  
> [!NOTE]  
> Al momento della stesura di questo documento, Microsoft Security Compliance Manager non include impostazioni specifiche per Jump server o altri host amministrativi protetti, ma è comunque possibile usare Security Compliance Manager (SCM) per creare le basi di riferimento iniziali per l'amministratore ospita. Per proteggere correttamente gli host, è tuttavia necessario applicare impostazioni di sicurezza aggiuntive appropriate per le workstation e i server altamente protetti.  
  
### <a name="applocker"></a>AppLocker  
Gli host amministrativi e i machinesshould virtuali sono configurati con script, strumenti e elenchi di elementi consentiti dell'applicazione tramite AppLocker o un software di restrizione delle applicazioni di terze parti Tutte le applicazioni amministrative o le utilità che non rispettano le impostazioni protette devono essere aggiornate o sostituite con gli strumenti conformi allo sviluppo e alle procedure amministrative sicure. Quando sono necessari strumenti nuovi o aggiuntivi in un host amministrativo, le applicazioni e le utilità devono essere testate accuratamente e se gli strumenti sono adatti per la distribuzione in host amministrativi, è possibile aggiungerli agli elenchi di elementi consentiti dei sistemi.  
  
### <a name="rdp-restrictions"></a>Restrizioni RDP  
Sebbene la configurazione specifica possa variare a seconda dell'architettura dei sistemi amministrativi, è necessario includere restrizioni sugli account e sui computer che possono essere utilizzati per stabilire connessioni di Remote Desktop Protocol (RDP) ai sistemi gestiti, come ad esempio l'uso di Desktop remoto Gateway Gateway Desktop remoto per controllare l'accesso ai controller di dominio e ad altri sistemi gestiti da utenti e sistemi autorizzati.  
  
È necessario consentire gli accessi interattivi da parte di utenti autorizzati e rimuovere o persino bloccare altri tipi di accesso non necessari per l'accesso al server.  
  
### <a name="patch-and-configuration-management"></a>Gestione delle patch e della configurazione  
Le organizzazioni più piccole possono basarsi su offerte come Windows Update o [Windows Server Update Services](https://technet.microsoft.com/windowsserver/bb332157) (WSUS) per gestire la distribuzione degli aggiornamenti nei sistemi Windows, mentre le organizzazioni di grandi dimensioni possono implementare software di gestione delle patch e della configurazione aziendale, ad esempio Microsoft endpoint Configuration Manager. Indipendentemente dai meccanismi usati per distribuire gli aggiornamenti al popolamento generale del server e della workstation, è consigliabile prendere in considerazione distribuzioni separate per sistemi a protezione elevata, ad esempio controller di dominio, autorità di certificazione e host amministrativi. Con la separazione di questi sistemi dall'infrastruttura di gestione generale, se il software di gestione o gli account del servizio sono compromessi, la compromissione non può essere facilmente estesa ai sistemi più sicuri nell'infrastruttura.  
  
Sebbene non sia necessario implementare processi di aggiornamento manuali per i sistemi protetti, è necessario configurare un'infrastruttura separata per l'aggiornamento dei sistemi protetti. Anche nelle organizzazioni di grandi dimensioni, questa infrastruttura può in genere essere implementata tramite server WSUS e oggetti Criteri di gruppo dedicati per i sistemi protetti.  
  
### <a name="blocking-internet-access"></a>Blocco dell'accesso a Internet  
Gli host amministrativi non devono essere autorizzati ad accedere a Internet, né devono essere in grado di esplorare la rete Intranet di un'organizzazione. I browser Web e le applicazioni simili non devono essere consentiti negli host amministrativi. È possibile bloccare l'accesso a Internet per gli host protetti tramite una combinazione di impostazioni del firewall perimetrale, configurazione di WFAS e configurazione del proxy "Black Hole" negli host protetti. È inoltre possibile utilizzare l'elenco elementi consentiti dell'applicazione per impedire l'utilizzo di Web browser in host amministrativi.  
  
### <a name="virtualization"></a>Virtualizzazione  
Laddove possibile, è consigliabile implementare macchine virtuali come host amministrativi. Con la virtualizzazione, è possibile creare sistemi amministrativi per singolo utente che vengono archiviati e gestiti a livello centrale e che possono essere facilmente arrestati quando non sono in uso, assicurando che le credenziali non rimangano attive nei sistemi amministrativi. È inoltre possibile richiedere che gli host amministrativi virtuali vengano reimpostati su uno snapshot iniziale dopo ogni uso, assicurando che le macchine virtuali rimangano inalterate. Ulteriori informazioni sulle opzioni per la virtualizzazione degli host amministrativi sono disponibili nella sezione seguente.  
  
## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>Approcci di esempio per l'implementazione di host amministrativi protetti  
Indipendentemente dalla modalità di progettazione e distribuzione dell'infrastruttura host amministrativa, è necessario tenere presenti le linee guida disponibili in "principi per la creazione di host amministrativi protetti" più indietro in questo argomento. Ognuno degli approcci descritti fornisce informazioni generali su come separare i sistemi "amministrativi" e "produttivi" usati dal personale IT. I sistemi di produttività sono computer che gli amministratori IT utilizzano per controllare la posta elettronica, esplorare Internet e utilizzare software di produttività generale, ad esempio Microsoft Office. I sistemi amministrativi sono computer finalizzati e dedicati per l'amministrazione quotidiana di un ambiente IT.  
  
Il modo più semplice per implementare gli host amministrativi protetti consiste nel fornire al personale IT le workstation protette da cui possono eseguire attività amministrative. In un'implementazione di solo workstation, ogni workstation amministrativa viene utilizzata per avviare gli strumenti di gestione e le connessioni RDP per gestire i server e altre infrastrutture. Le implementazioni solo workstation possono essere efficaci nelle organizzazioni più piccole, sebbene le infrastrutture più grandi e più complesse possano trarre vantaggio da una progettazione distribuita per gli host amministrativi in cui vengono usati server amministrativi dedicati e workstation, come descritto in "implementazione di workstation e di Jump server protetti" più avanti in questo argomento.  
  
### <a name="implementing-separate-physical-workstations"></a>Implementazione di workstation fisiche separate  
Un modo per implementare gli host amministrativi consiste nell'emettere ogni utente IT due workstation. Una workstation viene utilizzata con un account utente "regolare" per eseguire attività quali il controllo della posta elettronica e l'utilizzo di applicazioni di produttività, mentre la seconda workstation è dedicata esclusivamente alle funzioni amministrative.  
  
Per la workstation di produttività, al personale IT possono essere assegnati account utente regolari anziché utilizzare account con privilegi per accedere a computer non protetti. La workstation amministrativa deve essere configurata con una configurazione rigorosamente controllata e il personale IT deve usare un account diverso per accedere alla workstation amministrativa.  
  
Se sono state implementate Smart Card, le workstation amministrative devono essere configurate in modo da richiedere gli accessi tramite smart card e il personale IT deve disporre di account distinti per l'uso amministrativo, configurato anche per richiedere smart card per l'accesso interattivo. L'host amministrativo deve essere finalizzato come descritto in precedenza e solo gli utenti IT designati dovrebbero essere autorizzati ad accedere localmente alla workstation amministrativa.  
  
#### <a name="pros"></a>Vantaggi  
Implementando sistemi fisici distinti, è possibile assicurarsi che ogni computer sia configurato in modo appropriato per il proprio ruolo e che gli utenti non possano inavvertitamente esporre i sistemi amministrativi a rischio.  
  
#### <a name="cons"></a>Svantaggi  
  
-   L'implementazione di computer fisici distinti comporta un aumento dei costi hardware.  
  
-   L'accesso a un computer fisico con le credenziali utilizzate per amministrare i sistemi remoti memorizza nella cache le credenziali in memoria.  
  
-   Se le workstation amministrative non vengono archiviate in modo sicuro, potrebbero essere vulnerabili alla compromissione tramite meccanismi quali i logger di chiavi hardware fisico o altri attacchi fisici.  
  
### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>Implementazione di una workstation fisica sicura con una workstation di produttività virtualizzata  
Con questo approccio, agli utenti IT viene assegnata una workstation amministrativa protetta dalla quale è possibile eseguire funzioni amministrative quotidiane, utilizzando Strumenti di amministrazione remota del server (strumenti di amministrazione remota) o connessioni RDP ai server all'interno dell'ambito di responsabilità. Quando gli utenti devono eseguire attività di produttività, possono connettersi tramite RDP a una workstation di produttività remota in esecuzione come macchina virtuale. È necessario usare credenziali separate per ogni workstation e i controlli quali le smart card devono essere implementati.  
  
#### <a name="pros"></a>Vantaggi  
  
-   Le workstation amministrative e le workstation di produttività sono separate.  
  
-   Il personale IT che usa workstation sicure per connettersi alle workstation di produttività può usare credenziali e smart card separate e le credenziali con privilegi non vengono depositate nel computer meno sicuro.  
  
#### <a name="cons"></a>Svantaggi  
  
-   Per implementare la soluzione sono necessari un lavoro di progettazione e implementazione e opzioni di virtualizzazione affidabili.  
  
-   Se le workstation fisiche non vengono archiviate in modo sicuro, potrebbero essere vulnerabili ad attacchi fisici che compromettono l'hardware o il sistema operativo e li rendono suscettibili all'intercettazione delle comunicazioni.  
  
### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>Implementazione di una singola workstation protetta con connessioni a macchine virtuali "produttività" e "amministrative" separate  
Con questo approccio, è possibile inviare agli utenti IT una singola workstation fisica bloccata come descritto in precedenza e in cui gli utenti IT non dispongono di accesso con privilegi. È possibile fornire connessioni Servizi Desktop remoto alle macchine virtuali ospitate su server dedicati, fornendo al personale IT una macchina virtuale che esegue la posta elettronica e altre applicazioni di produttività e una seconda macchina virtuale configurata come host amministrativo dedicato.  
  
È necessario richiedere una smart card o un altro accesso a più fattori per le macchine virtuali, usando account separati diversi dall'account usato per accedere al computer fisico. Dopo che un utente IT ha eseguito l'accesso a un computer fisico, può usare la smart card per la produttività per connettersi al computer di produttività remoto e a un account separato e a una smart card per connettersi al computer amministrativo remoto.  
  
#### <a name="pros"></a>Vantaggi  
  
-   Gli utenti IT possono usare una singola workstation fisica.  
  
-   Richiedendo account distinti per gli host virtuali e usando Servizi Desktop remoto connessioni alle macchine virtuali, le credenziali degli utenti IT non vengono memorizzate nella cache del computer locale.  
  
-   L'host fisico può essere protetto allo stesso grado degli host amministrativi, riducendo la probabilità di compromissione del computer locale.  
  
-   Nei casi in cui è possibile che la macchina virtuale di produttività di un utente IT o la relativa macchina virtuale amministrativa sia stata compromessa, la macchina virtuale può essere facilmente reimpostata su uno stato noto.  
  
-   Se il computer fisico è compromesso, nessuna credenziale con privilegi verrà memorizzata nella cache e l'utilizzo delle smart card può impedire la compromissione delle credenziali da parte dei logger di tasti.  
  
#### <a name="cons"></a>Svantaggi  
  
-   Per implementare la soluzione sono necessari un lavoro di progettazione e implementazione e opzioni di virtualizzazione affidabili.  
  
-   Se le workstation fisiche non vengono archiviate in modo sicuro, potrebbero essere vulnerabili ad attacchi fisici che compromettono l'hardware o il sistema operativo e li rendono suscettibili all'intercettazione delle comunicazioni.  
  
### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>Implementazione di workstation amministrative sicure e Jump server  
In alternativa alle workstation amministrative sicure o in combinazione con esse, è possibile implementare server di salto sicuro e gli utenti amministratori possono connettersi ai server di salto utilizzando RDP e smart card per eseguire attività amministrative.  
  
È necessario configurare i server di salto per eseguire il ruolo Gateway Desktop remoto per consentire l'implementazione di restrizioni sulle connessioni al server di salto e ai server di destinazione che saranno gestiti da quest'operazione. Se possibile, è necessario installare anche il ruolo Hyper-V e creare [desktop virtuali personali](https://technet.microsoft.com/library/dd759174.aspx) o altre macchine virtuali per utente per gli utenti amministratori da usare per le attività nei server di salto.  
  
Fornendo le macchine virtuali per utente amministratore nel server Jump, viene fornita la sicurezza fisica per le workstation amministrative e gli utenti amministratori possono reimpostare o arrestare le macchine virtuali quando non sono in uso. Se si preferisce non installare il ruolo Hyper-V e il ruolo Gateway Desktop remoto nello stesso host amministrativo, è possibile installarli in computer distinti.  
  
Laddove possibile, è consigliabile utilizzare gli strumenti di amministrazione remota per gestire i server. La funzionalità Strumenti di amministrazione remota del server (strumenti di amministrazione remota del server) deve essere installata nelle macchine virtuali degli utenti (o nel server di salto se non si implementano macchine virtuali per utente per l'amministrazione) e il personale amministrativo deve connettersi tramite RDP ai rispettivi macchine virtuali per eseguire attività amministrative.  
  
Nei casi in cui un utente amministratore deve connettersi tramite RDP a un server di destinazione per gestirlo direttamente, Gateway Desktop remoto deve essere configurato in modo da consentire la connessione solo se l'utente e il computer appropriati vengono utilizzati per stabilire la connessione alla destinazione Server. L'esecuzione di strumenti di strumenti di amministrazione remota del server (o simili) dovrebbe non essere consentita nei sistemi che non sono sistemi di gestione designati, ad esempio le workstation con utilizzo generale e i server membri che non sono Jump server.  
  
#### <a name="pros"></a>Vantaggi  
  
-   La creazione di Jump Server consente di eseguire il mapping di server specifici a "zone" (raccolte di sistemi con requisiti di configurazione, connessione e sicurezza simili) nella rete e di richiedere che l'amministrazione di ogni zona venga realizzata dal personale amministrativo. connessione da host amministrativi protetti a un server "zona" designato.  
  
-   Eseguendo il mapping di Jump Servers alle zone, è possibile implementare controlli granulari per le proprietà di connessione e i requisiti di configurazione e identificare facilmente i tentativi di connessione da sistemi non autorizzati.  
  
-   Implementando le macchine virtuali per amministratore in Jump Servers, si applica l'arresto e la reimpostazione delle macchine virtuali a uno stato pulito noto quando vengono completate le attività amministrative. Applicando l'arresto (o il riavvio) delle macchine virtuali quando vengono completate le attività amministrative, le macchine virtuali non possono essere interessate da utenti malintenzionati, né da attacchi di furto delle credenziali, perché le credenziali memorizzate nella cache non vengono mantenute dopo un riavvio.  
  
#### <a name="cons"></a>Svantaggi  
  
-   I server dedicati sono necessari per i server Jump, sia fisici che virtuali.  
  
-   L'implementazione di server di salto e workstation amministrative designati richiede un'attenta pianificazione e una configurazione che esegue il mapping a tutte le aree di sicurezza configurate nell'ambiente.  
  


