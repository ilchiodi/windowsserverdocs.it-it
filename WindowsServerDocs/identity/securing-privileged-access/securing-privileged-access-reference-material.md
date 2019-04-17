---
title: Materiale di riferimento per la protezione dell'accesso con privilegi
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22ee9a77-4872-4c54-82d9-98fc73a378c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ee5769a3ed0225ebdd31645027bace38f0bff1b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="securing-privileged-access-reference-material"></a>Materiale di riferimento per la protezione dell'accesso con privilegi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Questa sezione contiene informazioni di riferimento per l'accesso con privilegi di protezione tra cui:

-   [Modello di livelli dell'amministrazione di Active Directory](#ADATM_BM)

-   [Principio di origine pulita](#CSP_BM)

-   [Approccio di progettazione di foresta amministrativa ESAE](#ESAE_BM)

-   [Equivalenza di livello 0](#T0E_BM)

-   [Strumenti di amministrazione e tipi di accesso](#ATLT_BM)

## <a name="ADATM_BM"></a>Modello di livelli dell'amministrazione di Active Directory
Lo scopo di questo modello a livelli è di proteggere i sistemi di identità con un set di zone buffer tra il controllo completo dell'ambiente (livello 0) e le risorse di workstation ad alto rischio che utenti malintenzionati compromettono frequentemente.

![Diagramma che illustra i tre livelli del modello a livelli](../media/securing-privileged-access-reference-material/PAW_RM_Fig1.JPG)

Il modello a livelli è composto da tre livelli e include solo gli account amministrativi, gli account utente non standard:

-   **Livello 0** -controllo diretto dell'identità dell'organizzazione nell'ambiente. Livello 0 include account, gruppi e altre risorse che hanno il controllo amministrativo diretto o indiretto della foresta di Active Directory, domini o controller di dominio e tutte le risorse in esso. Il livello di riservatezza di tutte le risorse di livello 0 è equivalente in quanto si trovano tutti in modo efficace in controllo di altra.

-   **Livello 1** -controllo dei server aziendali e applicazioni. Le risorse di livello 1 includono i sistemi operativi server, i servizi cloud e le applicazioni aziendali. Gli account di livello 1 amministratore hanno il controllo amministrativo di una percentuale significativa del valore aziendale ospitato su queste risorse. Un ruolo di esempio comune è amministratori del server che gestiscono questi sistemi operativi con la possibilità di influenzare tutti i servizi aziendali.

-   **Livello 2** -controllo di workstation degli utenti e dispositivi. Gli account di amministratore 2 livello hanno il controllo amministrativo di una percentuale significativa del valore aziendale ospitato sulle workstation utente e dispositivi. Esempi includono l'Help Desk e gli amministratori di supporto computer perché possono avere un impatto l'integrità dei dati quasi qualsiasi utente.

> [!NOTE]
> I livelli fungono anche da un meccanismo di base definizione delle priorità per la protezione delle risorse amministrative, ma è importante considerare che un utente malintenzionato con controllo di tutte le risorse a qualsiasi livello può accedere tutti o la maggior parte dei beni aziendali. Il motivo che è utile come meccanismo di priorità di base è utente malintenzionato difficoltà e i costi. È più semplice per un utente malintenzionato di operare con controllo completo di tutte le identità (livello 0) o server e i servizi cloud (livello 1) rispetto a quelle se è necessario accedere a ogni singola workstation o dispositivo dell'utente (livello 2) per ottenere i dati dell'organizzazione.

### <a name="containment-and-security-zones"></a>Aree di contenimento e sicurezza
I livelli si riferiscono a un'area di sicurezza specifici. Sebbene siano state denominate in vari modi, aree di sicurezza sono un approccio consolidato che fornisce il contenimento delle minacce per la sicurezza attraverso l'isolamento del livello di rete tra di essi. Il modello a livelli si integra con l'isolamento, fornendo il contenimento degli avversari all'interno di un'area di sicurezza in cui l'isolamento di rete non è valido. Aree di sicurezza possono estendersi sia in locale e cloud dell'infrastruttura, come nell'esempio in cui i controller di dominio e i membri del dominio nello stesso dominio sono ospitati in locale e in Azure.

![Diagramma che illustra come le aree di sicurezza possono estendersi sia in locale e infrastruttura cloud](../media/securing-privileged-access-reference-material/PAW_RM_Fig2.JPG)

Il modello a livelli impedisce l'escalation dei privilegi limitando gli amministratori di controllare e in cui è possibile accedere (perché l'accesso a un computer concede il controllo di tali credenziali e tutte le risorse gestite da queste credenziali).

### <a name="control-restrictions"></a>Restrizioni di controllo
Restrizioni di controllo sono visualizzate nella figura seguente:

![Diagramma delle restrizioni di controllo](../media/securing-privileged-access-reference-material/PAW_RM_Fig3.JPG)

### <a name="primary-responsibilities-and-critical-restrictions"></a>Responsabilità principali e restrizioni critiche
**Amministratore di livello 0** -gestire l'archivio identità e un numero ridotto di sistemi che controllano in modo efficace, e:

-   Può gestire e controllare le risorse di qualsiasi livello in base alle esigenze

-   Può accedere solo in modo interattivo o alle risorse attendibili di livello 0

**Amministratore di livello 1** -gestire server aziendali, servizi e applicazioni, e:

-   Può gestire e controllare solo risorse a livello di livello 1 o 2

-   Possono solo accedere alle risorse (tramite il tipo di accesso di rete) che sono considerati attendibili a livello di livello 0 o livello 1

-   Può accedere solo in modo interattivo alle risorse attendibili a livello di livello 1

**Amministratore di livello 2** -gestire enterprise desktop, portatili, stampanti e altri dispositivi utente, e:

-   Può gestire e controllare solo risorse a livello di livello 2

-   Può accedere alle risorse (tramite il tipo di accesso di rete) in qualsiasi livello in base alle esigenze

-   Può accedere solo in modo interattivo alle risorse attendibili di livello 2

### <a name="logon-restrictions"></a>Restrizioni di accesso
Le restrizioni di accesso sono visualizzate nella figura riportata di seguito:

![Diagramma delle restrizioni di accesso](../media/securing-privileged-access-reference-material/PAW_RM_Fig4.JPG)

> [!NOTE]
> Si noti che alcune risorse possono avere impatto sulla disponibilità dell'ambiente di livello 0, ma non direttamente la riservatezza o l'integrità delle risorse. Queste includono il servizio Server DNS e i dispositivi di rete critici come proxy di Internet.

## <a name="CSP_BM"></a>Principio di origine pulita
Il principio di origine pulita richiede tutte le dipendenze di sicurezza siano attendibili come l'oggetto protetto.

![Diagramma che illustra come un soggetto che controlla un oggetto è una dipendenza di sicurezza di tale oggetto](../media/securing-privileged-access-reference-material/PAW_RM_Fig5.JPG)

Qualsiasi soggetto che controlla un oggetto è una dipendenza di sicurezza di tale oggetto. Se un antagonista può controllare qualsiasi elemento che effettivamente controlla un oggetto di destinazione, è possibile controllare tale oggetto di destinazione. Per questo motivo, è necessario assicurarsi che le garanzie di tutte le dipendenze di sicurezza siano uguale o superiore a livello di protezione desiderato dell'oggetto stesso.

Un principio semplice, durante la sua applicazione è necessario comprendere le relazioni di controllo di una risorsa di interesse (oggetto) e l'esecuzione di un'analisi di dipendenza di per individuare tutte le dipendenze di sicurezza (Subject(s)).

Poiché il controllo è transitivo, questo principio deve essere ripetuto in modo ricorsivo. Per esempio, se A controlla B e B controlla C, quindi a controlla indirettamente anche C.

![Diagramma che illustra come se A controlla B e B controlla C, quindi a controlla indirettamente anche C](../media/securing-privileged-access-reference-material/PAW_RM_Fig6.JPG)

Un utente malintenzionato che compromette A ottiene accesso a tutti gli elementi A controlla (incluso B) e tutto ciò che B controlla (incluso C). Utilizzando il linguaggio delle dipendenze di sicurezza in questo stesso esempio, B e A sono dipendenze di sicurezza di C e devono essere protette a livello di garanzia di C perché C abbia tale livello di garanzia.

Per i sistemi di identità e l'infrastruttura IT, questo principio deve essere applicato ai mezzi più comuni di controllo, compresi l'hardware in cui sono installati i sistemi, il supporto di installazione per i sistemi, l'architettura e la configurazione del sistema e operazioni quotidiane.

### <a name="clean-source-for-installation-media"></a>Origine pulita per i supporti di installazione
![Diagramma che illustra un'origine pulita per i supporti di installazione](../media/securing-privileged-access-reference-material/PAW_RM_CSIM.JPG)

Applicare il principio di origine pulita al supporto di installazione è necessario assicurarsi che il supporto di installazione non sia stato manomesso dopo il rilascio dal produttore (come migliore in cui è in grado di determinare). La figura illustra un utente malintenzionato che usa questo percorso per compromettere un computer:

![Figura che illustra un utente malintenzionato tramite un percorso per compromettere un computer](../media/securing-privileged-access-reference-material/PAW_RM_Fig8.JPG)

Applicare il principio di origine pulita al supporto di installazione è necessaria la convalida dell'integrità del software in tutto il ciclo vita, tra cui durante l'acquisizione, l'archiviazione e trasferire fino a quando non viene usato.

#### <a name="software-acquisition"></a>Acquisizione del software
L'origine del software deve essere convalidata in uno dei modi seguenti:

-   Software viene ottenuto da supporti fisici noti che provengono dal produttore o da una fonte autorevole, in genere si tratta di supporti spediti da un fornitore.

-   Software viene ottenuto da Internet e convalidato con hash file fornita dal produttore.

-   Software viene ottenuto da Internet e convalidato scaricando e confrontando due copie separate:

    -   Download in due host senza alcuna relazione di sicurezza (non nello stesso dominio e non gestiti dagli stessi strumenti), preferibilmente da connessioni Internet separate.

    -   Confrontare i file scaricati mediante un'utilità come certutil:

        `certutil -hashfile <filename>`

When possible, all application software, such as application installers and tools should be digitally signed and verified using Windows Authenticode with the [Windows Sysinternal](https://www.microsoft.com/sysinternals)s tool, *sigcheck.exe*, with revocation checking. Potrebbe essere necessari alcuni software in cui il fornitore potrebbe non fornire questo tipo di firma digitale.

#### <a name="software-storage-and-transfer"></a>Trasferimento e archiviazione del software
Dopo aver ottenuto il software, devono essere archiviato in un percorso protetto dalle modifiche, in particolare da host connessi a internet o personale attendibile a un livello inferiore rispetto ai sistemi in cui verrà installato il sistema operativo o software. Questa archiviazione può essere un supporto fisico o un percorso elettronico sicuro.

#### <a name="software-usage"></a>Utilizzo del software
In teoria, il software deve essere convalidato al momento che viene utilizzato, ad esempio quando viene manualmente, compresso per uno strumento di Gestione configurazione o installata importato uno strumento di gestione della configurazione.

### <a name="clean-source-for-architecture-and-design"></a>Origine pulita per la progettazione e architettura
Applicare il principio di origine pulita all'architettura di sistema è necessario assicurarsi che il sistema non sia dipendente da sistemi di attendibilità inferiori. Un sistema può essere dipendente da un sistema di attendibilità superiore, ma non su un sistema di attendibilità inferiore con standard di protezione inferiore.

Ad esempio, accettabile per Active Directory controllare un desktop utente standard, ma è un'escalation significativa del rischio dei privilegi per un desktop utente standard controllare di Active Directory.

![Diagramma che illustra come un sistema può essere dipendente da un sistema di attendibilità superiore, ma non su un sistema di attendibilità inferiore con standard di protezione inferiore](../media/securing-privileged-access-reference-material/PAW_RM_Fig09.JPG)

La relazione di controllo può essere introdotta in diversi modi, inclusi elenchi di controllo di accesso (ACL) di sicurezza per gli oggetti come file System, l'appartenenza al gruppo administrators locale su un computer o gli agenti installati in un computer in cui viene eseguito come sistema (con la possibilità di eseguire script e codice arbitrario).

Un esempio spesso trascurato è l'esposizione mediante accesso, che crea una relazione di controllo esponendo le credenziali amministrative di un sistema a un altro sistema. Questa è la causa principale perché furto di credenziali come passare l'hash è così potente. Quando un amministratore accede a un desktop utente standard con le credenziali di livello 0, sta esponendo le credenziali su quel PC desktop, consentendogli così il controllo di Active Directory e creando un'escalation del percorso dei privilegi ad Active Directory. Per ulteriori informazioni su questi attacchi, vedere [questa pagina](https://technet.microsoft.com/en-us/security/dn785092).

A causa del numero elevato di risorse che dipendono da sistemi di identità come Active Directory, è necessario ridurre il numero di sistemi che di Active Directory e controller di dominio dipendono.

![Diagramma che mostra che è necessario ridurre il numero di sistemi che dipendono da Active Directory e controller di dominio](../media/securing-privileged-access-reference-material/PAW_RM_Fig010.JPG)

Per ulteriori informazioni sulla protezione avanzata dei rischi principali di active directory, vedere [questa pagina](http://aka.ms/hardenAD).

### <a name="OSBCS_BM"></a>Standard operativi basati sul principio di origine pulita
Questa sezione descrive gli standard operativi e le aspettative per il personale amministrativo. Questi standard sono progettati per proteggere il controllo amministrativo dei sistemi del reparto IT dell'organizzazione contro i rischi che possono sorgere dai processi e procedure operative.

![Diagramma che illustra come standard sono progettati per proteggere il controllo amministrativo di informazioni di un'organizzazione contro i rischi che possono sorgere dai processi e procedure operative sistemi con tecnologia](../media/securing-privileged-access-reference-material/PAW_RM_Fig11.JPG)

#### <a name="integrating-the-standards"></a>Integrazione degli standard
È possibile integrare questi standard negli standard e nelle procedure generali dell'organizzazione. È possibile adattarli ai requisiti specifici, strumenti disponibili e al desiderio di rischio della propria organizzazione, ma è consigliabile apportare solo modifiche minime per ridurre i rischi. Si consiglia di utilizzare le impostazioni predefinite in questa guida come benchmark per lo stato finale ideale e gestire eventuali delta come eccezioni da affrontare in ordine di priorità.

Le indicazioni standard sono organizzate nelle sezioni seguenti:

-   Presupposti

-   Comitato consultivo per

-   Procedure operative

    -   Riepilogo

    -   Dettagli sugli standard

#### <a name="assumptions"></a>Presupposti
Gli standard in questa sezione si presuppongono che l'organizzazione ha gli attributi seguenti:

-   La maggior parte o tutti i server e workstation nell'ambito vengono aggiunti ad Active Directory.

-   Tutti i server da gestire eseguono Windows Server 2008 R2 o versione successiva e hanno abilitata la modalità RestrictedAdmin di RDP.

-   Tutte le workstation da gestire eseguono Windows 7 o versione successiva e hanno abilitata la modalità RestrictedAdmin di RDP.

    > [!NOTE]
    > Per abilitare la modalità RestrictedAdmin di RDP, vedere [questa pagina](http://aka.ms/RDPRA).

-   Le smart card sono disponibili e rilasciate per tutti gli account amministrativi.

-   Il *Builtin\Administrator* per ogni dominio è stato designato come un account di accesso di emergenza

-   Viene distribuita una soluzione di gestione di identità aziendale.

-   [LAPS](http://aka.ms/laps) è stato distribuito ai server e workstation per gestire la password dell'account amministratore locale

-   Esiste una soluzione di gestione accesso con privilegi, ad esempio Microsoft Identity Manager, sul posto, oppure è un piano di adottare una.

-   Il personale vengono assegnato a monitorare gli avvisi di protezione e della relativa risposta.

-   La capacità tecnica per applicare rapidamente gli aggiornamenti della sicurezza Microsoft è disponibile.

-   Baseboard management controller sul server non verrà utilizzato oppure dovranno conformarsi ai controlli di sicurezza.

-   Account amministratore e i gruppi per i server (amministratori di livello 1) e le workstation (amministratori di livello 2) verranno gestiti dagli amministratori del dominio (livello 0).

-   Esiste un comitato consultivo modifica (CAB) o un'altra autorità designata sul posto dell'approvazione delle modifiche di Active Directory.

#### <a name="change-advisory-board"></a>Comitato consultivo per
Un comitato consultivo modifica (CAB) è l'autorità di approvazione e del forum di discussione per le modifiche che potrebbero interferire con il profilo di sicurezza dell'organizzazione. Tutte le eccezioni a questi standard devono essere inviate al CAB con una valutazione dei rischi e una giustificazione.

Ogni standard in questo documento è influenzato dalla criticità di soddisfare gli standard per un determinato livello.

![Diagramma che illustra lo standard per determinati livelli](../media/securing-privileged-access-reference-material/PAW_RM_Fig12.JPG)

Tutte le eccezioni per gli elementi obbligatori (contrassegnati con ottagono rosso o un triangolo arancione in questo documento) sono considerate temporanee e devono essere approvate dal CAB. Linee guida includono:

-   La richiesta iniziale implica l'accettazione del rischio giustificazione firmata dal Supervisore immediato del personale e scade dopo sei mesi.

-   I rinnovi richiedono giustificazione e accettazione del rischio firmati da un direttore della business unit e scadono dopo sei mesi.

Tutte le eccezioni per gli elementi consigliati (contrassegnati con un cerchio giallo in questo documento) sono considerate temporanee e devono essere approvate dal CAB. Linee guida includono:

-   La richiesta iniziale implica l'accettazione del rischio giustificazione firmata dal Supervisore immediato del personale e scade dopo 12 mesi.

-   I rinnovi richiedono giustificazione e accettazione del rischio firmati da un direttore della business unit e scadono dopo 12 mesi.

#### <a name="operational-practices-standards-summary"></a>Riepilogo degli standard delle procedure operative
Le colonne del livello in questa tabella fare riferimento al livello dell'account amministrativo, il cui controllo in genere influisce su tutte le risorse in tale livello.

![Tabella che illustra i livelli dell'account amministrativo](../media/securing-privileged-access-reference-material/PAW_RM_Fig13.JPG)

Le decisioni operative che vengono effettuate con cadenza regolare sono fondamentali per mantenere le condizioni di sicurezza dell'ambiente. Questi standard per i processi e procedure consigliate aiutano a garantire che un errore operativo non comporti una vulnerabilità operativa sfruttabile nell'ambiente.

##### <a name="administrator-enablement-and-accountability"></a>Responsabilità e abilitazione dell'amministratore
Gli amministratori devono essere informati, abilitati, formazione e ritenuti responsabili per il funzionamento dell'ambiente nel modo più sicuro possibile.

###### <a name="administrative-personnel-standards"></a>Standard del personale amministrativo
Il personale amministrativo predisposto devono essere esaminato per garantire l'affidabilità e hanno l'esigenza dei privilegi amministrativi:

-   Eseguire controlli periodici sul personale prima dell'assegnazione di privilegi amministrativi.

-   Esaminare i privilegi amministrativi ogni trimestre per stabilire quale personale abbia ancora un'azienda legittima necessario per l'accesso amministrativo.

###### <a name="administrative-security-briefing-and-accountability"></a>Responsabilità e istruzioni sulla sicurezza amministrativa amministrativa
Gli amministratori devono essere informati e sono responsabili per i rischi per l'organizzazione e del loro ruolo nella gestione di tale rischio. Gli amministratori devono essere formati annualmente su:

-   Ambiente di rischio generale

    -   Antagonisti

    -   Tra cui pass-the-hash le tecniche di attacco e furto di credenziali

-   Operazioni non consentite e minacce specifiche dell'organizzazione

-   Ruoli dell'amministratore nella protezione contro gli attacchi

    -   Gestione di esposizione delle credenziali con il modello a livelli

    -   Uso delle workstation amministrative

    -   Uso della modalità RestrictedAdmin di protocollo Desktop remoto

-   Procedure amministrative specifiche dell'organizzazione

    -   Esaminare tutte le linee guida operative in questo standard

    -   Implementare le regole principali seguenti:

        -   Non utilizzare gli account amministrativi su se non le workstation amministrative

        -   Non disabilitare o annullare i controlli di sicurezza sul proprio account o workstation (ad esempio, le restrizioni di accesso o gli attributi necessari per le smart card)

        -   Segnalare problemi o attività insolite

Per garantire l'affidabilità, tutto il personale con gli account amministrativi necessario firmare un documento amministrativi con privilegi codice di condotta che dichiara che intende seguire consigliate per i criteri amministrativi specifici dell'organizzazione.

###### <a name="provisioning-and-deprovisioning-processes-for-administrative-accounts"></a>Provisioning e deprovisioning dei processi per gli account amministrativi
Gli standard seguenti devono essere soddisfatti per soddisfare i requisiti del ciclo di vita.

-   Tutti gli account amministrativi devono essere approvati da autorità di approvazione indicata nella tabella seguente.

    -   Approvazione deve essere concessa solo se il personale ha effettivamente esigenza dei privilegi amministrativi.

    -   Concessione dei privilegi amministrativi non deve superare sei mesi.

-   Accesso ai privilegi amministrativi deve essere immediatamente il deprovisioning dei quando:

    -   Il personale modifica le posizioni.

    -   Il personale lasciano l'organizzazione.

-   Gli account devono essere disabilitati immediatamente il personale lascia l'organizzazione.

-   Gli account disabilitati devono essere eliminati entro sei mesi e il record della loro eliminazione deve essere inserito nei record del comitato approvazione modifica.

-   Esaminare mensilmente tutte le appartenenze di account con privilegi per verificare che non siano state concesse autorizzazioni non autorizzate. Questo può essere sostituito da uno strumento automatico che segnala le modifiche.

|Livello di account con privilegi|Autorità di approvazione|Frequenza di revisione appartenenza|
|--------------|------------|----------------|
|Amministratore di livello 0|Comitato di approvazione|Mensile o automatico|
|Amministratore di livello 1|Gli amministratori di livello 0 o sicurezza|Mensile o automatico|
|Amministratore di livello 2|Gli amministratori di livello 0 o sicurezza|Mensile o automatico|

##### <a name="operationalize-least-privilege"></a>Utilizzare i privilegi minimi
Questi standard aiutano a ottenere privilegi minimi, riducendo il numero di amministratori nel ruolo e la quantità di tempo che dispongono di privilegi.

> [!NOTE]
> Per ottenere privilegi minimi dell'organizzazione è necessario comprendere i ruoli aziendali, i requisiti e i meccanismi di progettazione per assicurarsi che siano in grado di eseguire il proprio lavoro con privilegi minimi. Per raggiungere uno stato di privilegio minimo in un modello amministrativo spesso richiede l'utilizzo di più approcci:
>
> -   Limitare il numero di amministratori o membri di gruppi con privilegi
> -   Delegare meno privilegi agli account
> -   Fornire privilegi a scadenza su richiesta
> -   Fornire capacità per altri membri del personale eseguire attività (approccio concierge)
> -   Fornire i processi per l'accesso di emergenza e gli scenari di uso rari

###### <a name="limit-count-of-administrators"></a>Limitare il numero di amministratori
Un minimo di due membri del personale qualificati deve essere assegnato a ogni ruolo amministrativo per garantire la continuità aziendale.

Se il numero di membri del personale assegnati a qualsiasi ruolo supera i due, il comitato di approvazione deve approvare i motivi specifici per l'assegnazione dei privilegi ai singoli membri (inclusi i due originali). La giustificazione dell'approvazione deve includere:

-   Quali attività tecniche eseguite dagli amministratori che richiedono privilegi amministrativi

-   La frequenza con cui vengono eseguite le attività

-   Motivo specifico perché non è possibile eseguire le attività da un altro amministratore per suo conto

-   Documentare tutti gli altri approcci alternativi noti per la concessione del privilegio e perché ognuno non è accettabile

###### <a name="dynamically-assign-privileges"></a>Assegnare i privilegi in modo dinamico
Gli amministratori devono ottenere autorizzazioni "just-in-time" per poterle usare durante l'esecuzione delle attività. Nessuna autorizzazione verranno assegnata in modo permanente per gli account amministrativi.

> [!NOTE]
> Privilegi di amministratore assegnati in modo permanente tendono a creare una strategia di "privilegio massimo" perché il personale amministrativo richiedono un accesso rapido alle autorizzazioni per garantire la disponibilità operativa se si verifica un problema. -In-time autorizzazioni consentono di:
>
> -   Assegnare autorizzazioni in modo più granulare, avvicinandosi ai privilegi minimi.
> -   Ridurre il tempo di esposizione dei privilegi
> -   Monitoraggio autorizzazioni utilizzano per rilevare attacchi o abusi.

##### <a name="manage-risk-of-credential-exposure"></a>Gestire il rischio di esposizione delle credenziali
Utilizzare le procedure seguenti per gestire il rischio di esposizione delle credenziali corretto.

###### <a name="separate-administrative-accounts"></a>Account amministrativi separati
Tutto il personale autorizzato a disporre di privilegi amministrativi devono possedere account separati per le funzioni amministrative diverse dagli account utente.

-   **Gli account utente standard** -concessi privilegi di utente standard per attività di utente standard, ad esempio posta elettronica, esplorazione del web e utilizzare applicazioni line-of-business. Questi account non devono essere concessi privilegi amministrativi.

-   **Gli account amministrativi** -account separati creati per il personale assegnato ai privilegi amministrativi appropriati. Un amministratore che è necessario per gestire le risorse in ogni livello deve avere un account separato per ogni livello. Questi account non devono avere accesso alla posta elettronica o alla rete Internet pubblica.

###### <a name="administrator-logon-practices"></a>Procedure di accesso amministratore
Prima che un amministratore possa accedere a un host in modo interattivo (in locale rispetto al protocollo RDP standard, usando RunAs oppure utilizzando la console di virtualizzazione), tale host deve soddisfare o superare lo standard per il livello dell'account amministrativo (o un livello superiore).

Gli amministratori possono accedere solo per le workstation amministrative con gli account amministrativi. Gli amministratori accedere solo alle risorse gestite tramite la tecnologia di supporto approvata descritta nella sezione successiva.

> [!NOTE]
> Ciò è necessario perché l'accesso a un host in modo interattivo concede il controllo delle credenziali in tale host.
>
> Vedere il [strumenti di amministrazione e tipi di accesso](http://aka.ms/admintoolsecurity) per informazioni dettagliate sui tipi di accesso, strumenti di gestione comuni ed esposizione delle credenziali.

###### <a name="use-of-approved-support-technology-and-methods"></a>Uso della tecnologia di supporto approvata e metodi
Gli amministratori che supportano gli utenti e i sistemi remoti devono seguire queste linee guida per evitare che un antagonista nel controllo del computer remoto possa rubare le credenziali amministrative.

-   Le opzioni di supporto primario devono essere utilizzate se sono disponibili.

-   Le opzioni di supporto secondario devono essere utilizzate solo se l'opzione di supporto primario non è disponibile.

-   Non è possibile usare i metodi di supporto non consentiti.

-   Nessun accesso a internet o alla posta elettronica può essere eseguito per tutti gli account amministrativi in qualsiasi momento.

###### <a name="tier-0-forest-domain-and-dc-administration"></a>Foresta di livello 0, dominio e l'amministrazione di controller di dominio
Assicurarsi che siano applicate le procedure seguenti per questo scenario:

-   **Supporto del server remoto** -quando si accede in modalità remota un server, gli amministratori di livello 0 devono seguire queste linee guida:

    -   **Primario (strumento)** -strumenti remoti (tipo 3) gli accessi alla rete che usa. Per ulteriori informazioni, vedere [strumenti di amministrazione e tipi di accesso](http://aka.ms/admintoolsecurity).

    -   **Primario (interattivo)** -usare RestrictedAdmin di RDP o una sessione RDP Standard da una workstation amministrativa con un account di dominio

    > [!NOTE]
    > Se si dispone di una soluzione di gestione di livello con privilegi di livello 0, aggiungere "che Usa autorizzazioni ottenute in tempo da una soluzione di privileged access management."

-   **Supporto del server fisico** : quando fisicamente presente in una console di server o di una macchina virtuale (strumenti Hyper-V o VMWare), questi account non sono restrizioni di utilizzo di strumenti di amministrazione specifici, solo le restrizioni generali per le attività di utente standard come messaggio di posta elettronica e nelle reti internet aperte l'esplorazione.

    > [!NOTE]
    > Amministrazione di livello 0 è diversa dall'amministrazione degli altri livelli perché tutte le risorse di livello 0 dispongono già di un controllo diretto o indiretto di tutte le risorse. Ad esempio, un utente malintenzionato nel controllo di un controller di dominio non ha bisogno di rubare le credenziali da parte degli amministratori connessi hanno già accesso a tutte le credenziali di dominio nel database.

###### <a name="tier-1-server-and-enterprise-application-support"></a>Server di livello 1 e supporto delle applicazione enterprise
Assicurarsi che siano applicate le procedure seguenti per questo scenario:

-   **Supporto del server remoto** -quando si accede in modalità remota un server, gli amministratori di livello 1 devono seguire queste linee guida:

    -   **Primario (strumento)** -strumenti remoti (tipo 3) gli accessi alla rete che usa. For more information, see [Mitigating Pass-the-Hash and Other Credential Theft](https://www.microsoft.com/pth) v1 (pp 42-47).

    -   **Primario (interattivo)** -usare RestrictedAdmin di RDP da una workstation amministrativa con un account di dominio che usa le autorizzazioni ottenute in tempo da una soluzione di gestione accesso con privilegi.

    -   **Secondario** -accedere al server utilizzando una password dell'account locale impostata da LAPS da una workstation amministrativa.

    -   **Non consentito** -protocollo RDP Standard non possono essere utilizzati con un account di dominio.

    -   **Non consentito** -utilizzando l'account di dominio delle credenziali durante la sessione (ad esempio, usando *RunAs* o l'autenticazione a una condivisione). Ciò espone le credenziali di accesso al rischio di furto.

-   **Supporto del server fisico** : quando fisicamente presente in una console di server o di una macchina virtuale (strumenti Hyper-V o VMWare), gli amministratori di livello 1 devono recuperare la password dell'account locale da LAPS prima dell'accesso al server.

    -   **Primario** -recuperare la password dell'account locale impostata da LAPS da una workstation amministrativa prima di accedere al server.

    -   **Non consentito** -accesso con un account di dominio non è consentito in questo scenario.

    -   **Non consentito** -utilizzando l'account di dominio delle credenziali durante la sessione (ad esempio, RunAs o autenticazione per una condivisione). Ciò espone le credenziali di accesso al rischio di furto.

###### <a name="tier-2-help-desk-and-user-support"></a>Livello 2 help desk e supporto utente
Help Desk e utente le organizzazioni di supporto offrono assistenza agli utenti finali (che non richiedono privilegi amministrativi) e alle workstation dell'utente (che richiedono privilegi amministrativi).

**Supporto per l'utente** -attività includono l'assistenza agli utenti con le attività che non richiedono alcuna modifica alla workstation, che li spesso Mostra come usare una funzionalità dell'applicazione o una funzionalità del sistema operativo.

-   **Supporto per l'utente parte dell'help desk** -il personale di supporto di livello 2 si trova fisicamente all'area di lavoro dell'utente.

    -   **Primario** -"Over the shoulder" supporto può essere fornito con nessuno strumento.

    -   **Non consentito** -accesso con credenziali amministrative dell'account di dominio non è consentito in questo scenario. Se sono necessari privilegi amministrativi, passare al supporto per workstation parte dell'help desk.

-   **Supporto per l'utente remoto** -il personale di supporto di livello 2 è lontano fisicamente rispetto all'utente.

    -   **Primario** -assistenza remota, Skype for Business o la condivisione dello schermo dell'utente simile possono essere utilizzati. For more information, see [What is Windows Remote Assistance?](https://windows.microsoft.com/en-us/windows/what-is-windows-remote-assistance)

    -   **Non consentito** -accesso con credenziali amministrative dell'account di dominio non è consentito in questo scenario. Se sono necessari privilegi amministrativi, passare al supporto per workstation.

-   **Supporto per workstation** - attività includono manutenzione delle workstation o risoluzione dei problemi che richiede l'accesso a un sistema per la visualizzazione dei registri, l'installazione di software, l'aggiornamento dei driver e così via.

    -   **Supporto per workstation parte dell'help desk** -personale di supporto di livello 2 si trova fisicamente vicino alla workstation dell'utente.

        -   **Primario** -recuperare la password dell'account locale impostata da LAPS da una workstation amministrativa prima di connettersi alla workstation dell'utente.

        -   **Non consentito** -accesso con credenziali amministrative dell'account di dominio non è consentito in questo scenario.

    -   **Supporto per workstation remoto** -il personale di supporto di livello 2 è lontano fisicamente rispetto alla workstation.

        -   **Primario** -usare RestrictedAdmin di RDP da una workstation amministrativa con un account di dominio che usa le autorizzazioni ottenute in tempo da una soluzione di gestione accesso con privilegi.

        -   **Secondario** -recuperare la password di un account locale impostata da LAPS da una workstation amministrativa prima di connettersi alla workstation dell'utente.

        -   **Non consentito** -utilizzare il protocollo RDP standard con un account di dominio.

###### <a name="no-browsing-the-public-internet-with-admin-accounts-or-from-admin-workstations"></a>Nessuna ricerca sulle reti Internet pubbliche con gli account amministrativi o dalla workstation amministrativa
Il personale amministrativo non possono navigare nelle reti Internet aperte mentre è connesso con un account amministrativo o mentre si è connessi a una workstation amministrativa. Le uniche eccezioni autorizzate sono l'uso di un web browser per amministrare un servizio basato su cloud, come Microsoft Azure, Amazon Web Services, Microsoft Office 365 o Gmail aziendale.

###### <a name="no-accessing-email-with-admin-accounts-or-from-admin-workstations"></a>Nessun accesso alla posta elettronica con gli account amministrativi o dalla workstation amministrativa
Il personale amministrativo non possono accedere alla posta elettronica mentre è connesso con un account amministrativo o mentre si è connessi a una workstation amministrativa.

###### <a name="store-service-and-application-account-passwords-in-a-secure-location"></a>Servizio Store e applicazione le password in un luogo sicuro
Utilizzare le seguenti linee guida per la sicurezza fisica elabora che controllano l'accesso alla password:

-   Bloccare le password degli account di servizio in una cassaforte fisica.

-   Assicurarsi che solo il personale considerato attendibile in o sopra la classificazione di livello dell'account hanno accesso alla password dell'account.

-   Limitare il numero di persone che accedono alle password per un numero minimo di per garantire l'affidabilità.

-   Assicurarsi che tutti gli accessi alla password sono registrato, tenere traccia e monitorato da un interessata, ad esempio un responsabile che non è in grado di eseguire l'amministrazione IT.

##### <a name="strong-authentication"></a>Autenticazione avanzata
Utilizzare le procedure seguenti per configurare correttamente l'autenticazione avanzata.

###### <a name="enforce-smartcard-multi-factor-authentication-mfa-for-all-admin-accounts"></a>Applicare smart card multi-factor authentication (MFA) per tutti gli account amministrativi
Nessun account amministrativo può usare una password per l'autenticazione. Le uniche eccezioni autorizzate sono gli account di accesso di emergenza che sono protetti dai processi appropriati.

Collegare tutti gli account amministrativi a una smart card e abilitare l'attributo "**per l'accesso interattivo occorre una Smart Card**."

Uno script deve essere implementato per reimpostare automaticamente e periodicamente il valore hash di password casuale disabilitando e riabilitando immediatamente l'attributo "**per l'accesso interattivo occorre una Smart Card**."

Non autorizzare eccezioni per gli account usati dal personale umano oltre gli account di accesso di emergenza.

###### <a name="enforce-multi-factor-authentication-for-all-cloud-admin-accounts"></a>Applicare multi-Factor Authentication per tutti gli account di amministratore di Cloud
Tutti gli account con privilegi amministrativi in un servizio cloud, ad esempio Microsoft Azure e Office 365, è necessario utilizzare l'autenticazione a più fattori.

##### <a name="rare-use-emergency-procedures"></a>Procedure di emergenza usare raramente
Procedure operative devono supportare gli standard seguenti:

-   Verificare interruzioni possano essere risolte rapidamente.

-   Verificare le attività rare con privilegi elevati possono essere completate in base alle esigenze.

-   Assicurarsi che vengano usate procedure sicure per proteggere le credenziali e i privilegi.

-   Assicurarsi che i processi di rilevamento e di approvazione appropriati sono seguiti.

###### <a name="correctly-follow-appropriate-processes-for-all-emergency-access-accounts"></a>Seguire correttamente i processi appropriati per tutti gli account di accesso di emergenza
Assicurarsi che ogni account di accesso di emergenza disponga di un foglio di rilevamento nella cassaforte.

La procedura descritta nel foglio di rilevamento della password deve essere seguita per ogni account, che include la modifica della password dopo ogni uso e la disconnessione da qualsiasi workstation o server usati dopo il completamento.

Tutto l'uso di accesso di emergenza gli account devono essere approvati dal comitato di approvazione in avanzati o dopo i fatti come un'emergenza approvata.

###### <a name="restrict-and-monitor-usage-of-emergency-access-accounts"></a>Limitare e monitorare l'utilizzo di account di accesso di emergenza
Per l'uso degli account di accesso di emergenza:

-   Amministratori di dominio autorizzati solo accessibili per gli account di accesso di emergenza con privilegi di amministratore di dominio.

-   Gli account di accesso di emergenza possono essere usati solo sui controller di dominio e altri host di livello 0.

-   Questo account deve essere utilizzato solo per:

    -   Eseguire la risoluzione dei problemi e la correzione di problemi tecnici che impediscono l'utilizzo di account amministrativi corretti.

    -   Eseguire attività rare, ad esempio:

        -   Amministrazione dello schema

        -   Attività a livello di foresta che richiedono privilegi amministrativi aziendali si noti che la gestione della topologia compresa la gestione di siti e subnet di Active Directory viene delegata per limitare l'uso di questi privilegi.

-   L'uso di uno di questi account deve avere autorizzazione scritta dal responsabile del gruppo di sicurezza

-   La procedura nel foglio di rilevamento per ogni account di accesso di emergenza richiede la password venga modificata a ogni uso. Un membro del gruppo di sicurezza deve convalidare l'avvenuta modifica.

###### <a name="temporarily-assign-enterprise-admin-and-schema-admin-membership"></a>Assegnare temporaneamente l'appartenenza di enterprise admin e schema admin
Privilegi devono essere aggiunti come necessario e rimosso dopo l'uso. Questi privilegi assegnati per la sola durata dell'attività da completare e per un massimo di 10 ore devono essere all'account di emergenza. Uso e la durata di questi privilegi devono essere documentati nel record da tavolo approvazione delle modifiche dopo il completamento dell'attività.

## <a name="ESAE_BM"></a>Approccio di progettazione di foresta amministrativa ESAE
Questa sezione contiene un approccio per una foresta amministrativa basata sull'architettura di riferimento Enhanced Security amministrativi ambiente (ESAE) distribuita dal team di servizi professionali Microsoft per proteggere i clienti da attacchi alla sicurezza.

Le foreste amministrative dedicate consentono alle organizzazioni di ospitare gli account amministrativi, le workstation e gruppi in un ambiente che dispone di controlli di sicurezza più avanzati rispetto all'ambiente di produzione.

Questa architettura consente un numero di controlli di sicurezza che non sono possibili o sono difficilmente configurabili in un'architettura a foresta singola, anche in una gestita con workstation amministrative con privilegi (Paw). Questo approccio consente il provisioning degli account come standard senza privilegi utenti nella foresta amministrativa che hanno privilegi elevati nell'ambiente di produzione, consentendo maggiore imposizione tecnica della governance. Questa architettura consente inoltre l'utilizzo della funzionalità di autenticazione selettiva di un trust come mezzo per limitare gli accessi (e l'esposizione delle credenziali) solo a host autorizzati. In situazioni in cui si desidera un maggiore livello di sicurezza per la foresta di produzione senza maggiori costi e complessità di una ricompilazione completa, una foresta amministrativa può fornire un ambiente che consentono di aumentare il livello di garanzia dell'ambiente di produzione.

Sebbene questo approccio aggiunga una foresta in un ambiente Active Directory, i costi e complessità sono limitati dalla progettazione fissa, il footprint hardware/software di piccole dimensioni e numero ridotto di utenti.

> [!NOTE]
> Questo approccio è ideale per l'amministrazione di Active Directory, ma molte applicazioni non sono compatibili con amministrato da account da una foresta esterna e Usa un trust standard.

La figura illustra una foresta ESAE usata per l'amministrazione delle risorse di livello 0 e una foresta PRIV configurata per l'uso con la funzionalità di Privileged Access Management di Microsoft Identity Manager. Per ulteriori informazioni sulla distribuzione di un'istanza PAM MIM, vedere [Privileged Identity Management per servizi di dominio Active Directory (AD DS)](https://technet.microsoft.com/en-us/library/mt150258.aspx) articolo.

![Figura che illustra una foresta ESAE usata per l'amministrazione delle risorse di livello 0 e una foresta PRIV configurata per l'uso con la funzionalità di Privileged Access Management di Microsoft Identity Manager](../media/securing-privileged-access-reference-material/PAW_RM_Fig14.JPG)

Una foresta amministrativa dedicata è un singolo dominio standard dedicata alla funzione di gestione di Active Directory foresta di Active Directory. Domini e foreste amministrative possono essere protetti più rigorosamente foreste di produzione a causa di limitato di casi d'usano.

Progettazione di una foresta amministrativa deve includere le seguenti considerazioni:

-   **Ambito limitato** -il valore primario di una foresta amministrativa è l'elevato livello di garanzia di sicurezza e la superficie di attacco ridotta che comporta un rischio residuo inferiore. La foresta può essere usata per ospitare applicazioni e funzioni di gestione aggiuntive, ma ogni incremento nell'ambito aumenta la superficie di attacco della foresta e delle relative risorse. L'obiettivo consiste nel limitare le funzioni degli utenti all'interno dell'insieme di strutture e amministratore per mantenere la superficie di attacco minima, pertanto è consigliabile considerare attentamente ogni incremento dell'ambito.

-   **Configurazioni del trust** -configurare il trust dalle foreste gestite (s) o domini alla foresta amministrativa

    -   Un trust unidirezionale viene richiesto dall'ambiente di produzione alla foresta amministrativa. Può trattarsi di un trust di dominio o un trust tra foreste. Non è necessario che il dominio o un insieme di strutture admin consideri attendibili i domini o foreste gestite per gestire Active Directory, anche se altre applicazioni possono richiedere una relazione di trust bidirezionale, una convalida di sicurezza e il test.

    -   Autenticazione selettiva dovrebbe essere utilizzato per limitare l'account nella foresta amministrativa per l'accesso solo a host di produzione appropriati. Per mantenere i controller di dominio e delegare i diritti in Active Directory, questo richiede in genere la concessione di accesso consentito"" a destra per controller di dominio a livello account amministrativi designati 0 nella foresta amministrativa. Per ulteriori informazioni, vedere Configurazione delle impostazioni di autenticazione selettiva.

-   **Privilegi e protezione avanzata del dominio** -foresta amministrativa deve essere configurata per privilegi minimi, in base ai requisiti per l'amministrazione di Active Directory.

    -   Concessione dei diritti per amministrare i controller di dominio e delegare le autorizzazioni richiede l'aggiunta di account amministrativi della foresta al gruppo locale di dominio BUILTIN\Administrators.. Questo avviene perché il gruppo globale Domain Admins non può avere membri da un dominio esterno.

    -   Un'avvertenza all'uso di questo gruppo per concedere diritti è che non hanno accesso amministrativo a nuovi oggetti Criteri di gruppo per impostazione predefinita. This can be changed by following the procedure in [this knowledge base article](https://support.microsoft.com/kb/321476) to change the schema default permissions.

    -   Gli account nella foresta amministrativa utilizzati per amministrare l'ambiente di produzione non devono essere concessi privilegi amministrativi per la foresta amministrativa, domini o le workstation in essa contenuti.

    -   I privilegi amministrativi sulla foresta amministrativa devono essere controllati attentamente da un processo offline per ridurre la possibilità per un utente o dipendente malintenzionato cancelli i registri di controllo. Ciò consente di garantire che il personale con gli account amministrativi di produzione non possano ridurre le restrizioni sui propri account e aumentare i rischi per l'organizzazione.

    -   The administrative forest should follow the Microsoft Security Compliance Baseline (SCB) configurations for the domain, including strong configurations for authentication protocols.

    -   Tutti gli host della foresta amministrativa devono essere aggiornati automaticamente con gli aggiornamenti della sicurezza. Anche se ciò può comportare il rischio di interrompere le operazioni di manutenzione di controller di dominio, fornisce una mitigazione significativa dei rischi di sicurezza delle vulnerabilità senza patch.

        > [!NOTE]
        > Un'istanza dedicata di Windows Server Update Services può essere configurata per approvare automaticamente gli aggiornamenti. Per ulteriori informazioni, vedere la sezione "Approvare automaticamente gli aggiornamenti per installazione" in approvazione degli aggiornamenti.

-   **Protezione avanzata della workstation** -compilare le workstation amministrative mediante il [Privileged Access workstation](../securing-privileged-access/privileged-access-workstations.md) (fase 3), ma modificare l'appartenenza al dominio alla foresta amministrativa anziché all'ambiente di produzione.

-   **Protezione avanzata dei controller di dominio e server** - per tutti i controller di dominio e i server nella foresta amministrativa:

    -   Assicurarsi che tutti i supporti siano convalidati sulla base delle indicazioni [origine pulita per i supporti di installazione](http://aka.ms/cleansource)

    -   Assicurarsi che il server della foresta amministrativa deve avere i sistemi operativi più recenti installati, anche se questo non è fattibile nell'ambiente di produzione.

    -   Gli host della foresta amministrativa devono essere aggiornati automaticamente con gli aggiornamenti della sicurezza.

        > [!NOTE]
        > Windows Server Update Services può essere configurato per approvare automaticamente gli aggiornamenti. Per ulteriori informazioni, vedere la sezione "Approvare automaticamente gli aggiornamenti per installazione" in approvazione degli aggiornamenti.

    -   Le baseline di sicurezza devono essere utilizzate come configurazioni di avvio.

        > [!NOTE]
        > Customers can use the Microsoft Security Compliance Toolkit (SCT) for configuring the baselines on the administrative hosts.

    -   Avvio per impedire ai pirati informatici o malware, il tentativo di caricare codice non firmato nel processo di avvio protetto.

        > [!NOTE]
        > Questa funzionalità è stata introdotta in Windows 8 per sfruttare la Unified Extensible Firmware Interface (UEFI).

    -   Crittografia dell'intero volume per ridurre perdita fisica del computer, ad esempio portatili amministrativi utilizzati in modalità remota.

        > [!NOTE]
        > Vedere [BitLocker](https://technet.microsoft.com/en-us/library/dn641993.aspx) per ulteriori informazioni.

    -   Restrizioni USB per proteggere da vettori di infezioni fisici.

        > [!NOTE]
        > Vedere [controllo di accesso in lettura o scrittura su dispositivi rimovibili o su supporti](https://technet.microsoft.com/en-us/library/cc730808(v=ws.10).aspx) per ulteriori informazioni.

    -   Isolamento della rete per proteggere da attacchi di rete e azioni di amministrazione accidentali. I firewall host dovrebbero bloccare tutte le connessioni in ingresso tranne quelle esplicitamente necessarie e bloccare completamente l'accesso Internet in uscita.

    -   Antimalware per proteggere da malware e minacce note.

    -   Analisi della superficie di attacco per impedire l'introduzione di nuovi vettori di attacchi a Windows durante l'installazione del nuovo software.

        > [!NOTE]
        > Uso di strumenti, ad esempio il [Analyzer superficie di attacco (ASA)](https://www.microsoft.com/en-us/download/details.aspx?id=24487) ti aiuteranno a valutare le impostazioni di configurazione in un host e identificare i vettori degli attacchi dovuti a modifiche software o alla configurazione.

-   Protezione avanzata dell'account

    -   Multi-factor authentication deve essere configurato per tutti gli account nella foresta amministrativa, ad eccezione di un account. Almeno un account amministrativo deve essere basato su password per garantire l'accesso funzionerà nel caso in cui l'autenticazione a più fattori di elaborare le interruzioni. Questo account deve essere protetto da un processo di controllo fisico rigoroso.

    -   Account configurati per l'autenticazione a più fattori deve essere configurato per impostare un nuovo NTLM hash sugli account regolarmente. Ciò è possibile disabilitare e abilitare l'attributo dell'account Smart card è necessaria per l'accesso interattivo.

        > [!NOTE]
        > Questa azione può interrompere le operazioni in corso che usano questo account, in modo che il processo deve essere avviato solo quando gli amministratori non devono usare l'account, ad esempio durante la notte o nei fine settimana.

-   Controlli detective

    -   I controlli detective per la foresta amministrativa devono essere progettati per segnalare eventuali anomalie nella foresta amministrativa. Il numero limitato di scenari autorizzati e attività consente di ottimizzare questi controlli in modo più accurato rispetto all'ambiente di produzione.

Per ulteriori informazioni sui servizi Microsoft per la progettazione e distribuzione di un ESAE per l'ambiente, vedere [questa pagina](https://www.microsoft.com/services).

## <a name="T0E_BM"></a>Equivalenza di livello 0
La maggior parte delle organizzazioni controllano l'appartenenza ai gruppi potente Active Directory di livello 0 come Administrators, Domain Admins ed Enterprise Admins.  Molte organizzazioni trascurano il rischio di altri gruppi che fanno in modo efficace con privilegi equivalenti in un ambiente tipico di active directory. These groups offer a relatively easy escalation path for an attacker to the same explicit Tier 0 privileges using various different attack methods.

Ad esempio, un operatore di server può accedere a un supporto di backup di un controller di dominio ed estrarre tutte le credenziali dai file di supporto e usarle per riassegnare i privilegi.

Le organizzazioni devono controllare e monitorare l'appartenenza a tutti i gruppi di livello 0 (inclusa l'appartenenza nidificata) tra cui:

-   Enterprise Admins

-   Domain Admins

-   Schema Admins

-   BUILTIN\Administrators.

-   Account Operators

-   Gruppo Backup Operators

-   Operatori di stampa

-   Server Operators

-   Controller di dominio

-   Controller di dominio di sola lettura

-   Proprietari autori criteri di gruppo

-   Operazioni di crittografia

-   Utenti DCOM

-   Altri gruppi delegati

    > [!NOTE]
    > "Altri gruppi delegati" fa riferimento a gruppi che possono essere creati dall'organizzazione per gestire le operazioni di directory che possono avere anche accesso al livello 0.

## <a name="ATLT_BM"></a>Strumenti di amministrazione e tipi di accesso
Si tratta di informazioni di riferimento per identificare il rischio di esposizione delle credenziali associato all'uso di diversi strumenti di amministrazione per l'amministrazione remota.

In uno scenario di amministrazione remota, le credenziali sono sempre esposte nel computer di origine in modo da una workstation amministrativa attendibile con privilegi (PAW) è sempre consigliabile per gli account sensibili o ad alto impatto. Se le credenziali sono esposte al rischio di furto potenziale sul computer (remoto) di destinazione dipende principalmente dal tipo di accesso windows usato dal metodo di connessione.

Questa tabella include istruzioni per le più comuni strumenti di amministrazione e i metodi di connessione:

|Connessione<br />metodo|Tipo di accesso|Credenziali riusabili nella destinazione|Commenti|
|-----------|-------|--------------------|------|
|Accedere alla console|Interattivo|v|Include l'accesso remoto all'hardware / schede di Lights-Out e KVMs di rete.|
|RUNAS|Interattivo|v||
|RUNAS/NETWORK|Nuove credenziali|v|Clona la sessione LSA corrente per l'accesso locale, ma usa le nuove credenziali quando ci si connette alle risorse di rete.|
|Desktop remoto (esito positivo)|RemoteInteractive|v|Se il client desktop remoto è configurato per la condivisione di dispositivi e risorse locali, quelli potrebbero risultare compromessi.|
|Desktop remoto (errore - tipo di accesso negato)|RemoteInteractive|-|Per impostazione predefinita, se il protocollo RDP accesso ha esito negativo credenziali vengono archiviate solo brevemente. Questo potrebbe non essere il caso, se il computer viene compromesso.|
|Uso di NET * \\\SERVER|Rete|-||
|Uso di NET * \\\SERVER /u: utente|Rete|-||
|Snap-in MMC al computer remoto|Rete|-|Esempio: Computer, Visualizzatore eventi, dispositivo di gestione, servizi|
|Gestione remota di Windows PowerShell|Rete|-|Esempio: Server Enter-PSSession|
|PowerShell WinRM con CredSSP|NetworkClearText|v|Server New-PSSession<br />-Credssp di autenticazione<br />-Credenziale Credential|
|PsExec senza credenziali esplicite|Rete|-|Esempio: PsExec \\\server cmd|
|PsExec con credenziali esplicite|Rete + interattivo|v|PsExec \\\server -u utente -p pwd cmd<br />Crea più sessioni di accesso.|
|Registro di sistema remoto|Rete|-||
|Gateway Desktop remoto|Rete|-|L'autenticazione a Gateway Desktop remoto.|
|Attività pianificata|Batch|v|Password verrà salvata anche come segreto LSA sul disco.|
|Eseguire gli strumenti come servizio|Servizio|v|Password verrà salvata anche come segreto LSA sul disco.|
|Scanner delle vulnerabilità|Rete|-|La maggior parte degli scanner per impostazione predefinita utilizzando gli accessi alla rete, anche se alcuni fornitori possono implementare gli accessi non di rete e introdurre ulteriori rischi di furto delle credenziali.|

Per l'autenticazione web, usare il riferimento dalla tabella riportata di seguito:

|Connessione<br />metodo|Tipo di accesso|Credenziali riusabili nella destinazione|Commenti|
|-----------|-------|--------------------|------|
|"L'autenticazione di base" di IIS|NetworkCleartext<br />(IIS) 6.0 +<br /><br />Interattivo<br />(precedente a IIS 6.0)|v||
|IIS "autenticazione integrata di Windows"|Rete|-|Provider Kerberos e NTLM.|

Definizioni delle colonne:

-   **Tipo di accesso** identifica il tipo di accesso avviato dalla connessione.

-   **Credenziali riusabili nella destinazione** indica che i seguenti tipi di credenziali verranno archiviati nella memoria del processo LSASS nel computer di destinazione in cui l'account specificato è connesso in locale:

    -   Hash LM e NT

    -   TGT di Kerberos

    -   Password non crittografata (se applicabile).

    -

I simboli in questa tabella sono definiti come segue:

-   (-) indica quando le credenziali non sono esposte.

-   (v) indica quando le credenziali sono esposte.

Per le applicazioni di gestione che non sono presenti in questa tabella, è possibile determinare il tipo di accesso dal campo tipo di accesso in Controlla eventi di accesso. For more information, see [Audit logon events](https://technet.microsoft.com/en-us/library/cc787567(v=ws.10).aspx).

In computer basati su Windows, tutte le autenticazioni vengono elaborate come uno dei diversi tipi di accesso, indipendentemente dal fatto che l'autenticazione protocollo o autenticatore usato. Questa tabella include i tipi di accesso più comuni e i relativi attributi rispetto al furto di credenziali:

|Tipo di accesso|#|Autenticatori accettati|Credenziali riusabili nella sessione LSA|Esempi|
|-------|---|--------------|--------------------|------|
|Interattivo (anche noto come accesso locale)|2|Password, smart card,<br />altri|Sì|Accesso alla console;<br />RUNAS;<br />Soluzioni di controllo remoto hardware (ad esempio KVM di rete o accesso remoto / scheda Lights-Out nel server)<br />Autenticazione di base di IIS (prima di IIS 6.0)|
|Rete|3|Password,<br />Hash NT,<br />Ticket Kerberos|No (eccetto se è abilitata la delega, quindi il ticket Kerberos presente)|NET USE;<br />Chiamate RPC;<br />Registro di sistema remoto.<br />IIS autenticazione integrata di Windows;<br />Autenticazione di Windows SQL;|
|Batch|4|Password (in genere archiviata come segreto LSA)|Sì|Attività pianificate|
|Servizio|5|Password (in genere archiviata come segreto LSA)|Sì|Servizi di Windows|
|NetworkCleartext|8|Password|Sì|Autenticazione di base di IIS (IIS 6.0 e versioni successive).<br />Windows PowerShell con CredSSP|
|Nuove credenziali|9|Password|Sì|RUNAS/NETWORK|
|RemoteInteractive|10|Password, smart card,<br />altri|Sì|Desktop remoto (precedentemente noto come "Servizi Terminal")|

Definizioni delle colonne:

-   **Tipo di accesso** è il tipo di accesso richiesto.

-   **#**è l'identificatore numerico per il tipo di accesso che viene segnalato negli eventi di controllo nel registro eventi di protezione.

-   **Autenticatori accettati** indica quali tipi di autenticatori sono in grado di avviare un accesso di questo tipo.

-   **Credenziali riusabili** in LSA sessione indica se il tipo di accesso risulta nella sessione LSA che contiene le credenziali, ad esempio password non crittografate, gli hash NT o i ticket Kerberos che potrebbero essere usati per l'autenticazione ad altre risorse di rete.

-   **Esempi** elencati scenari comuni in cui viene usato il tipo di accesso.

> [!NOTE]
> For more information about Logon Types, see [SECURITY_LOGON_TYPE enumeration](https://technet.microsoft.com/en-us/library/aa380129(VS.85).aspx).
