---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: Account che possono favorire il furto di credenziali
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e5d2b01f73127f84f700dab3518b66687f0294f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863102"
---
# <a name="attractive-accounts-for-credential-theft"></a>Account che possono favorire il furto di credenziali

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Attacchi con furto di credenziali sono quelle in cui un utente malintenzionato ottiene inizialmente più elevato con privilegi di livello (radice, amministratore o del sistema, a seconda del sistema operativo in uso) l'accesso a un computer in una rete e quindi Usa liberamente gli strumenti disponibili per estrarre le credenziali dalle sessioni di altri account connessi. A seconda della configurazione di sistema, queste credenziali possono essere estratte sotto forma di hash, ticket o password non crittografate. Se sono presenti le credenziali raccolte per gli account locali che potrebbero esistere in altri computer nella rete (ad esempio, gli account di amministratore in Windows o account radice in OSX, UNIX o Linux), l'autore dell'attacco vengono visualizzate le credenziali da altri computer in la rete per propagare i compromessi in altri computer e tentare di ottenere le credenziali di due tipi di account specifici:  

1.  Account di dominio con privilegi elevati con privilegi sia vasto e complesso (vale a dire, gli account che dispongono di privilegi a livello di amministratore in diversi computer e in Active Directory). Questi account non siano membri di uno qualsiasi dei gruppi di privilegi più elevati in Active Directory, ma potrebbe essere stato concesso privilegi a livello di amministratore in diversi server e workstation nel dominio o foresta, rendendole così in modo efficace potente membri di gruppi con privilegi in Active Directory. Nella maggior parte dei casi, gli account che sono stati concessi livelli di privilegio elevati tra vasta quantità di infrastruttura di Windows sono gli account del servizio, in modo che gli account del servizio devono sempre essere valutati per ampiezza e la profondità dei privilegi.  

2.  "Person molto importante" account di dominio (VIP). Nel contesto di questo documento, un account VIP è qualsiasi account che dispone dell'accesso alle informazioni di un utente malintenzionato vuole (proprietà intellettuale e altre informazioni riservate), o qualsiasi account che può essere utilizzato per concedere all'utente malintenzionato l'accesso a tali informazioni. Esempi di questi account utente:  

    1.  Dirigenti il cui account hanno accesso a informazioni aziendale riservate  

    2.  Account per il personale Help Desk che sono responsabile della gestione di computer e applicazioni usate dai dirigenti  

    3.  Account per l'ufficio legale che hanno accesso ai documenti di un'offerta e il contratto dell'organizzazione, se i documenti sono per la propria organizzazione o organizzazioni client  

    4.  I pianificatori di prodotto che hanno accesso ai piani e le specifiche per i prodotti in fase di sviluppo di un'azienda di pipeline, indipendentemente dai tipi di prodotti che dell'azienda rende  

    5.  Ricercatori di cui vengono usati per accedere ai dati study, le formulazioni prodotto o qualsiasi altre ricerche di interesse a un utente malintenzionato  

Poiché gli account con privilegi elevati in Active Directory possono essere usati per propagare i compromessi e modificare account VIP o i dati che possono accedere, gli account più utili per gli attacchi contro il furto di credenziali sono account che sono membri del gruppo Enterprise Admins, Gli amministratori di dominio e gli amministratori dei gruppi in Active Directory.  

Poiché i controller di dominio sono presenti i repository per il database di Active Directory Domain Services e i controller di dominio hanno accesso completo a tutti i dati in Active Directory, i controller di dominio anche interessati dalla compromissione, se in parallelo con attacchi con furto di credenziali o dopo uno o più account di Active Directory con privilegi elevati essere stata compromessa. Anche se molte pubblicazioni (e molti utenti malintenzionati) l'obiettivo dell'appartenenza al gruppo Domain Admins la descrizione di attacchi con furto di credenziali pass-the-hash e altri (come descritto nella [riducendo la superficie di attacco Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)) , un account che sia un membro di uno qualsiasi dei gruppi elencati di seguito è utilizzabile per compromettere l'intera installazione di Active Directory Domain Services.  

> [!NOTE]  
> Per informazioni complete sulla pass-the-hash e altri attacchi contro il furto di credenziali, vedere la [attacchi Mitigating Pass-the-Hash (PTH) e altre tecniche di furto delle credenziali](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf) elencati nel white paper [appendice M : Collegamenti a documenti e letture consigliate](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md). Per altre informazioni sugli attacchi da avversari determinati, che sono talvolta detti "minacce persistenti avanzate" (APT), vedi [avversari determinati e attacchi destinati](https://www.microsoft.com/download/details.aspx?id=34793).  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>Attività che aumentano la probabilità di compromissione  
Poiché la destinazione di furto di credenziali è in genere account VIP e gli account di dominio con privilegi elevati, è importante per gli amministratori di essere consapevole di attività che aumentano la probabilità di successo di un attacco di furto di credenziali. Anche se gli utenti malintenzionati attaccano account VIP, anche se gli indirizzi VIP non vengono assegnati livelli elevati di privilegio nei sistemi o nel dominio, il furto delle proprie credenziali richiede altri tipi di attacchi, ad esempio l'indirizzo VIP per fornire informazioni segrete di ingegneria sociale. O l'autore dell'attacco prima di tutto necessario ottenere l'accesso con privilegi a un sistema nel quale VIP vengono memorizzati nella cache le credenziali. Per questo motivo, le attività che aumentano la probabilità di furto di credenziali descritta di seguito sono incentrate principalmente su come impedire l'acquisizione delle credenziali amministrative con privilegi elevati. Queste attività sono meccanismi comuni mediante il quale gli utenti malintenzionati sono in grado di compromettere i sistemi per ottenere credenziali con privilegi.  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>L'accesso a computer non protetto con gli account con privilegi  
La vulnerabilità di core che consenta di attacchi con furto di credenziali abbia esito positivo è l'atto di accedere al computer che non sono protette con account che sono su vasta scala e profondamente con privilegi in tutto l'ambiente. Questi accessi possono essere il risultato di diversi errori di configurazione descritte di seguito.  

#### <a name="not-maintaining-separate-administrative-credentials"></a>Non mantenere Separate le credenziali amministrative  
Sebbene sia raramente, per valutare varie installazioni di Active Directory Domain Services, sono state trovate ai dipendenti IT usando un singolo account per tutte le operazioni. L'account è membro di almeno uno dei gruppi con privilegi più elevati in Active Directory ed è lo stesso account che i dipendenti usano per accedere alle loro workstation del mattino, controllare la posta elettronica, esplorare i siti Internet e scaricare contenuto al loro computer. Quando gli utenti eseguono con gli account vengono concesse le autorizzazioni e diritti di amministratore locale, che espongono il computer locale per completare compromissione. Quando tali account sono anche membri dei gruppi con più privilegi in Active Directory, che espongono l'intera foresta per compromettere, semplificando in modo semplice per un utente malintenzionato di assumere il controllo completo dell'ambiente Active Directory e Windows.  

Analogamente, in alcuni ambienti, abbiamo scoperto che i nomi utente stesso e le password vengono usate per gli account radice nel computer non Windows poiché vengono usate nell'ambiente di Windows, che consente agli utenti malintenzionati di estendere compromissione dei sistemi UNIX o Linux per i sistemi Windows e viceversa.

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>Accessi da workstation compromessa o server membro con gli account con privilegi  
Quando viene usato un account di dominio con privilegi elevati per l'accesso interattivo a una workstation compromesso o un server membro, tale computer compromesso può raccogliere le credenziali da qualsiasi account che esegue l'accesso al sistema.  

#### <a name="unsecured-administrative-workstations"></a>Workstation amministrative non protette  
In molte organizzazioni, il personale IT usare più account. Un account utilizzato per l'accesso alla workstation del dipendente e perché si tratta del personale IT, hanno spesso diritti di amministratore locale sulle proprie workstation. In alcuni casi, controllo account utente è a sinistra abilitato in modo che l'utente riceve almeno un accesso di suddivisione dei token al momento dell'accesso e necessario elevare quando sono necessari i privilegi. Quando gli utenti che eseguono attività di manutenzione, vengono in genere usare gli strumenti di gestione installato in locale e fornire le credenziali per gli account di dominio con privilegi, selezionando il **Esegui come amministratore** opzione o da fornire le credenziali quando richiesto. Anche se questa configurazione potrebbe sembrare appropriata, che espone l'ambiente per compromettere perché:  

-   L'account utente "normale" che il dipendente viene utilizzato per accedere alla propria workstation abbia diritti di amministratore locale, il computer viene vulnerabile agli [da drive-by download](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx) attacchi in cui l'utente viene indotto a installare malware.  

-   Il malware è installato nel contesto di un account amministrativo, il computer ora può essere utilizzato per acquisire le sequenze di tasti, il contenuto degli Appunti, screenshot e le credenziali residenti in memoria, ognuna delle quali può comportare l'esposizione delle credenziali di un dominio potenti account.  

I problemi in questo scenario sono duplici. In primo luogo, anche se vengono usati account separati per l'amministrazione locale e il dominio, il computer non è protetto e non consente di proteggere gli account dal furto. In secondo luogo, l'account utente normale e l'account dell'amministratore sono state concesse le autorizzazioni e diritti significativamente.  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>Esplorazione di Internet con un Account con privilegiato elevati  
Gli utenti che accedono ai computer con gli account che sono membri del gruppo Administrators locale nel computer o i membri di gruppi con privilegi in Active Directory e che quindi esplorare Internet (o una intranet compromessa) espongono il computer locale e il Directory da compromettere.  

L'accesso a un sito Web da utenti malintenzionati predisposto con un browser in esecuzione con privilegi amministrativi può consentire un utente malintenzionato depositare codice dannoso nel computer locale nel contesto dell'utente con privilegi. Se l'utente ha diritti di amministratore locale nel computer, gli utenti malintenzionati possono indurre l'utente download codice dannoso o l'apertura degli allegati di posta elettronica che sfruttano vulnerabilità delle applicazioni e sfruttano i privilegi dell'utente per l'estrazione nella cache locale credenziali per tutti gli utenti attivi nel computer. Se l'utente dispone di diritti amministrativi nella directory tramite l'appartenenza a gruppi Administrators in Active Directory, Enterprise Admins e Domain Admins, l'autore dell'attacco può estrarre le credenziali di dominio e usarli per compromettere l'intero dominio di Active Directory Domain Services o un insieme di strutture, senza la necessità di compromettere eventuali altri computer nella foresta.  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>Configurare gli account con privilegi locali con le stesse credenziali nei sistemi  
Configurazione di stesso nome di account amministratore locale e la password in molti o tutti i computer consente le credenziali di prelevata dal database SAM su un unico computer per essere usato per compromettere tutti gli altri computer che utilizzano le stesse credenziali. Come minimo, si dovrebbe usare password diverse per gli account amministratore locali in ogni sistema del dominio. Gli account amministratore locale possono anche essere denominati in modo univoco, ma l'uso di password diverse per gli account locali con privilegi di ciascun sistema è sufficiente a garantire che le credenziali possono essere utilizzate in altri sistemi.  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation e uso eccessivo di gruppi di dominio con privilegi  
Concedere l'appartenenza ai gruppi in un dominio con contratto Enterprise, DA o BA crea una destinazione per gli utenti malintenzionati. Maggiore è il numero di membri di questi gruppi, maggiore di attacchi la probabilità che un utente con privilegi possa inavvertitamente usare impropriamente le credenziali e li espone per il furto di credenziali. Ogni workstation o server a cui accede un utente di dominio con privilegi presenta un meccanismo possibili con cui le credenziali dell'utente con privilegi possono essere raccolte e usate per compromettere il dominio di Active Directory Domain Services e la foresta.  

### <a name="poorly-secured-domain-controllers"></a>Controller di dominio scarsamente protetto  
I controller di dominio ospitano una replica di database di Active Directory Domain Services di un dominio. Nel caso di controller di dominio di sola lettura, la replica locale del database contiene le credenziali per solo un subset degli account nella directory, nessuno dei quali sono gli account di dominio con privilegi elevati per impostazione predefinita. Sul controller di dominio di lettura / scrittura, ogni controller di dominio gestisce una replica completa dei database di Active Directory Domain Services, incluse le credenziali non solo per gli utenti con privilegi, ad esempio Domain Admins, ma gli account con privilegi, ad esempio gli account di controller di dominio o Krbtgt del dominio account, ovvero l'account associato con il servizio KDC nei controller di dominio. Se esistono applicazioni aggiuntive che non sono necessari per la funzionalità di controller di dominio vengono installate nei controller di dominio o se i controller di dominio non rigorosamente delle patch e protetta, gli utenti malintenzionati potrebbero compromettere li tramite le vulnerabilità senza patch installate, oppure possono sfruttare altri vettori di attacco per installare il software dannoso direttamente su di essi.  

## <a name="privilege-elevation-and-propagation"></a>La propagazione e l'elevazione dei privilegi  
Indipendentemente dal fatto i metodi di attacco usati, Active Directory viene sempre come destinazione quando viene attaccato un ambiente Windows, perché in ultima analisi controlla l'accesso a qualsiasi elemento gli utenti malintenzionati desiderato. Ciò non significa che è assegnato l'intera directory, tuttavia. Account specifico, i server e i componenti dell'infrastruttura sono in genere le destinazioni principali di attacchi contro Active Directory. Questi account sono descritti di seguito.  

### <a name="permanent-privileged-accounts"></a>Account con privilegi permanenti  
Poiché l'introduzione di Active Directory, è stato possibile per gli account di uso di privilegi elevati per creare la foresta di Active Directory e quindi le autorizzazioni necessarie per eseguire attività di ordinaria amministrazione agli account con meno privilegi e diritti di delegato. Appartenenza a gruppi Administrators, Domain Admins o Enterprise Admins in Active Directory è richiesto solo temporaneamente e raramente in un ambiente che implementa gli approcci con privilegi minimi per l'amministrazione giornaliera.  

Gli account con privilegi permanenti sono account che sono stati inseriti in gruppi con privilegi elevati e sono a sinistra di un giorno in giorno. Se l'organizzazione colloca i cinque account nel gruppo Domain Admins per un dominio, questi cinque account possono essere destinazione 24-ore un giorno, sette giorni alla settimana. Tuttavia, l'effettiva necessità di usare account con privilegi di Domain Admins è in genere solo per configurazione a livello di dominio specifici e per brevi periodi di tempo.  

### <a name="vip-accounts"></a>Account VIP  
Una destinazione spesso trascurata in violazioni della sicurezza di Active Directory è l'account di "persons molto importante" (o gli indirizzi VIP) in un'organizzazione. Gli account con privilegi sono destinati perché tali account possono concedere l'accesso a utenti malintenzionati, che consente loro di danneggiare o distruggere anche sistemi di destinazione, come descritto in precedenza in questa sezione.  

### <a name="privilege-attached-active-directory-accounts"></a>Account di "Privilegio-Attached" Active Directory  
Account di Active Directory "Associati con privilegi" sono account di dominio che non sono state apportate i membri di tutti i gruppi che hanno i massimi livelli di privilegi in Active Directory, ma sono invece state concesse elevati livelli di privilegio per il numero di server di e workstation nell'ambiente. Questi account sono la maggior parte degli account spesso basato su dominio sono configurati per eseguire i servizi in sistemi appartenenti a un dominio, in genere per le applicazioni in esecuzione in sezioni estese dell'infrastruttura. Anche se questi account non hanno privilegi in Active Directory, se sono stati concessi i privilegi elevati in un numero elevato di sistemi, possono essere utilizzati per compromettere o persino distruggere i segmenti di grandi dimensioni dell'infrastruttura, ottenere lo stesso effetto di compromissione di un account di Active Directory con privilegi.  
