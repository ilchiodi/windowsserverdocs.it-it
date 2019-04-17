---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: Account interessanti per il furto di credenziali
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ae1bfe017e8b21c3abfcdc137153b0fd379053fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="attractive-accounts-for-credential-theft"></a>Account interessanti per il furto di credenziali

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gli attacchi contro il furto di credenziali sono quelle in cui un utente malintenzionato ottiene inizialmente più elevato con privilegi (radice, amministratore o del sistema, a seconda del sistema operativo in uso) l'accesso a un computer in una rete e quindi Usa liberamente disponibili strumenti per estrarre le credenziali dalle sessioni di altri account connesso. A seconda della configurazione di sistema, queste credenziali possono essere estratti sotto forma di hash, biglietti o password non crittografate anche. Se uno qualsiasi delle credenziali raccolte per gli account locali che potrebbero esistere in altri computer nella rete (ad esempio, gli account di amministratore in Windows o account radice in OSX, UNIX o Linux), l'autore dell'attacco presenta le credenziali da altri computer della rete per propagare i compromessi in altri computer e per tentare di ottenere le credenziali di due tipi specifici di account :  

1.  Account di dominio con privilegi con privilegi sia vasto e complesso (vale a dire, gli account che dispongono di privilegi di amministratore in diversi computer e in Active Directory). Questi account non possono essere membri di uno dei gruppi con privilegi più elevato in Active Directory, ma potrebbero essere stato concesso privilegi a livello di amministratore in diversi server e workstation nel dominio o foresta, che li rende in modo efficace potente come membri di gruppi con privilegi in Active Directory. Nella maggior parte dei casi, gli account che sono stati concessi privilegi elevati in quantità generale dell'infrastruttura di Windows sono account di servizio, in modo che gli account di servizio devono sempre essere valutati per e della complessità dei privilegi.  

2.  "Persona molto importante" account di dominio (VIP). Nel contesto di questo documento, un account VIP è qualsiasi account che dispone dell'accesso alle informazioni un utente malintenzionato vuole (proprietà intellettuale e altre informazioni sensibili) o tutti gli account che può essere utilizzato per concedere l'accesso utente malintenzionato a tali informazioni. Esempi di questi account utente:  

    1.  Dirigenti il cui account hanno accesso a informazioni aziendali sensibili  

    2.  Account per il personale dell'Help Desk responsabili per la gestione di computer e applicazioni utilizzate da dirigenti  

    3.  Account per l'ufficio legale che hanno accesso ai documenti di offerta e un contratto di un'organizzazione, se i documenti sono per la propria organizzazione o alle organizzazioni di client  

    4.  Addetti alla pianificazione del prodotto che hanno accesso a specifiche e i piani per i prodotti in fase di sviluppo di una società di pipeline, indipendentemente dai tipi di prodotti che rende la società  

    5.  Ricercatori con account vengono utilizzati per accedere a dati study, formulazione prodotto o qualsiasi altra ricerca di interesse per un utente malintenzionato  

Poiché gli account con privilegi elevati in Active Directory possono essere usati per propagare i compromessi e modificare i dati che possano accedere o account VIP, gli account particolarmente utili per gli attacchi contro il furto di credenziali sono account che sono membri dei gruppi Administrators, Enterprise Admins e Domain Admins in Active Directory.  

Poiché i controller di dominio sono i repository per il database di Active Directory e controller di dominio hanno accesso completo a tutti i dati in Active Directory, i controller di dominio sono destinati anche per la compromissione, se in parallelo con gli attacchi contro il furto di credenziali, o dopo l'uno o più Active Directory con privilegi elevati account viene compromesso. Sebbene numerose pubblicazioni (e molti utenti malintenzionati) concentrarsi sull'appartenenza al gruppo Domain Admins quando si descrive gli attacchi contro il furto di credenziali pass-the-hash e altre (come descritto [riduzione della superficie di attacco Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)), un account che sia un membro di uno dei gruppi elencati di seguito può essere utilizzato per compromettere l'intera installazione di servizi di dominio Active Directory.  

> [!NOTE]  
> Per informazioni complete su pass-the-hash e altri attacchi contro il furto di credenziali, vedere il [attacchi Mitigating Pass-the-Hash (PTH) e altre tecniche di furto delle credenziali](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf) white paper elencati in [appendice m: collegamenti ai documenti e consiglia di leggere](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md). Per ulteriori informazioni sugli attacchi da antagonisti, che vengono talvolta detta "minacce persistenti avanzate" (APTs), vedere [determinati utenti malintenzionati e destinazione attacchi](https://www.microsoft.com/download/details.aspx?id=34793).  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>Attività che aumentano la probabilità di compromissione  
Poiché la destinazione di furto di credenziali è in genere gli account di dominio con privilegi elevati e gli account VIP, è importante per gli amministratori di tenere conto delle attività che aumentano la probabilità di successo di un attacco di furto di credenziali. Anche se gli utenti malintenzionati target account VIP, anche se gli indirizzi VIP non sono assegnate livelli elevati di privilegi su sistemi o nel dominio, il furto di credenziali richiede altri tipi di attacchi, ad esempio ingegneria sociale l'indirizzo VIP per fornire informazioni segrete. O l'autore dell'attacco deve ottenere prima l'accesso con privilegi a un sistema sul quale VIP vengono memorizzate nella cache le credenziali. Per questo motivo, le attività che aumentano il rischio di furto di credenziali descritto di seguito sono incentrate principalmente su come evitare l'acquisizione delle credenziali amministrative con privilegi elevati. Queste attività sono meccanismi più comuni che gli utenti malintenzionati sono in grado di danneggiare i sistemi per ottenere le credenziali con privilegi.  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>L'accesso a un computer non protetti con gli account con privilegi  
La vulnerabilità dei componenti di base che consente di attacchi contro il furto di credenziali abbia esito positivo sia l'atto di accesso al computer che non sono protette con gli account che sono su larga scala e profondamente con privilegi in tutto l'ambiente. Questi accessi possono dipendere da diversi errori di configurazione descritte di seguito.  

#### <a name="not-maintaining-separate-administrative-credentials"></a>Senza mantenere Separate le credenziali amministrative  
Anche se questo è relativamente raro, valutare diverse installazioni di dominio Active Directory, abbiamo trovato i dipendenti IT che usano un singolo account per tutte le attività. L'account è membro di almeno uno dei gruppi con privilegi più elevati in Active Directory ed è lo stesso account che l'uso ai dipendenti di accedere alle proprie workstation AM, controllare la posta elettronica, esplorare i siti Internet e scaricare il contenuto ai computer. Quando gli utenti eseguono con gli account siano concesse le autorizzazioni e diritti di amministratore locale, espongono il computer locale per completare compromissione. Quando tali account sono anche membri dei gruppi con più privilegi in Active Directory, espongono nell'intera foresta di compromissione dell'installazione, semplificando in modo semplice per un utente malintenzionato di ottenere il controllo completo dell'ambiente Active Directory e Windows.  

Analogamente, in alcuni ambienti, abbiamo scoperto che gli stessi nomi utente e password vengono utilizzate per account principale nel computer non Windows come vengono utilizzate nell'ambiente Windows, che consente a utenti malintenzionati di estendere i compromesso da sistemi UNIX o Linux in sistemi Windows e viceversa.

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>Accessi a workstation compromessa o server membro con gli account con privilegi  
Quando un account di dominio con privilegi elevati è utilizzato per accedere a una workstation compromessa o un server membro in modo interattivo, tale computer compromesso può raccogliere le credenziali da tutti gli account che esegue l'accesso al sistema.  

#### <a name="unsecured-administrative-workstations"></a>Workstation amministrative protette  
In molte organizzazioni, il personale IT utilizzare più account. Un account utilizzato per l'accesso alla workstation del dipendente e poiché si tratta di personale IT, hanno spesso diritti amministrativi sulle proprie workstation. In alcuni casi, controllo dell'account utente è abilitato in modo che l'utente riceve almeno un accesso split sinistra token all'accesso e necessario elevare quando sono necessari i privilegi. Quando gli utenti eseguono attività di manutenzione, in genere utilizzare strumenti di gestione installato in locale e fornire le credenziali per gli account di dominio con privilegi, selezionando il **Esegui come amministratore** opzione o per fornire le credenziali quando richiesto. Sebbene questa configurazione può sembrare appropriata, che espone l'ambiente per compromettere perché:  

-   L'account utente "normale" che utilizza il dipendente di accedere alle workstation disponga di diritti di amministratore locale, il computer è vulnerabile ad [download inconsapevole](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx) attacchi in cui l'utente viene indotto a installare malware.  

-   Il malware è installato nel contesto di un account amministrativo, il computer può ora essere usato per acquisire le pressioni di tasti, il contenuto degli Appunti, screenshot e le credenziali residenti in memoria, ognuna delle quali possono causare l'esposizione delle credenziali di un account di dominio potente.  

I problemi in questo scenario sono duplice. Prima di tutto, anche se l'account separati vengono utilizzati per l'amministrazione locale e il dominio, il computer non è protetto e non consente di proteggere gli account contro il furto. In secondo luogo, l'account utente normale e l'account amministrativo sono state concesse le autorizzazioni e diritti eccessivi.  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>Esplorazione di Internet con un Account con privilegiato elevati  
Gli utenti che accedono a computer con gli account che sono membri del gruppo Administrators locale nel computer o i membri di gruppi con privilegi in Active Directory e che quindi esplorare Internet (o una rete intranet compromessa) espongono il computer locale e compromettere la directory.  

L'accesso a un sito Web appositamente con un browser in esecuzione con privilegi amministrativi può consentire un attacco effettuare il codice dannoso nel computer locale nel contesto dell'utente con privilegiato. Se l'utente dispone di diritti di amministratore locale nel computer, utenti malintenzionati possono indurre l'utente il download di codice dannoso o apertura di allegati di posta elettronica che sfruttano le vulnerabilità di applicazione e sfruttano i privilegi dell'utente di estrarre in locale memorizzato nella cache le credenziali per tutti gli utenti attivi nel computer. Se l'utente disponga di diritti amministrativi nella directory dall'appartenenza al gruppo Enterprise Admins, Domain Admins o dei gruppi Administrators in Active Directory, l'autore dell'attacco può estrarre le credenziali di dominio e usarli per compromettere l'intero dominio di Active Directory o un insieme di strutture, senza la necessità di compromettere qualsiasi altro computer nella foresta.  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>Configurazione account con privilegi locali con le stesse credenziali in tutti i sistemi  
Configurare il nome dell'account amministratore locale stesso e la password nei molti o tutti i computer consente le credenziali di furto dal database SAM in un computer a essere usati per compromettere tutti gli altri computer che usano le stesse credenziali. Come minimo, è necessario utilizzare password diverse per gli account amministratore locali in ogni sistema appartenenti a un dominio. Gli account amministratore locale possono anche essere denominati in modo univoco, ma l'utilizzo di password diverse per gli account locali con privilegi di ciascun sistema è sufficiente a garantire che non possono essere utilizzate in altri sistemi.  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation e utilizzo eccessivo di gruppi di dominio con privilegi  
Concedere l'appartenenza ai gruppi di EA, DA o BA in un dominio crea una destinazione per gli utenti malintenzionati. Maggiore è il numero di membri di questi gruppi, maggiori attacchi la probabilità che un utente con privilegiato potrebbe usare impropriamente le credenziali inavvertitamente ed esporli per furto di credenziali. Ogni workstation o server a cui accede un utente di dominio con privilegi presenta un possibile meccanismo mediante il quale le credenziali dell'utente con privilegi possono essere raccolti e usate per compromettere il dominio Active Directory e la foresta.  

### <a name="poorly-secured-domain-controllers"></a>Controller di dominio protetti  
I controller di dominio ospitano una replica del database di Active Directory del dominio. Nel caso di controller di dominio di sola lettura, la replica del database locale contiene le credenziali per solo un sottoinsieme di account nella directory, nessuno dei quali sono gli account di dominio con privilegi per impostazione predefinita. Nei controller di dominio di sola lettura, ogni controller di dominio viene mantenuta una replica completa dei database di Active Directory, incluse le credenziali non solo per gli utenti con privilegi come Domain Admins, ma gli account con privilegi, ad esempio gli account di controller di dominio o account Krbtgt del dominio, ovvero l'account è associato il KDC service nei controller di dominio. Se esistono applicazioni aggiuntive che non sono necessari per le funzionalità di controller di dominio sono installate nei controller di dominio, se i controller di dominio non sono rigorosamente patch e protette, gli autori di attacchi potrebbero compromettere tramite vulnerabilità senza patch o possono sfruttare altri vettori di attacco per installare il software dannoso direttamente su di essi.  

## <a name="privilege-elevation-and-propagation"></a>La propagazione ed elevazione dei privilegi  
Indipendentemente dai metodi di attacco usati, Active Directory è sempre come destinazione quando si verifichi un ambiente Windows, perché consente di controllare in definitiva accesso a qualsiasi gli utenti malintenzionati desiderato. Questo non significa che è destinata l'intera directory, tuttavia. Account specifico, server e componenti dell'infrastruttura sono in genere le destinazioni di Active Directory attacchi di primario. Questi account sono descritti di seguito.  

### <a name="permanent-privileged-accounts"></a>Account con privilegi permanente  
Poiché l'introduzione di Active Directory, è stato possibile per gli account di utilizzo privilegiata elevati per creare l'insieme di strutture Active Directory e quindi delegato diritti e autorizzazioni necessarie per eseguire l'amministrazione quotidiana agli account con meno privilegi. Appartenenza ai gruppi Administrators, Domain Admins o Enterprise Admins in Active Directory è richiesto solo temporaneamente e raramente in un ambiente che implementa approcci di privilegi di amministrazione giornaliera.  

Gli account con privilegi permanenti sono account che sono stati inseriti in gruppi con privilegi e a sinistra è di un giorno per giorno. Se l'organizzazione luoghi e cinque gli account nel gruppo Domain Admins per un dominio, i cinque account può essere destinazione 24 ore un giorno, 7 giorni su 7. Tuttavia, la reale necessità di utilizzare gli account con privilegi di Domain Admins è in genere solo per la configurazione a livello di dominio specifici e per brevi periodi di tempo.  

### <a name="vip-accounts"></a>Account VIP  
Una destinazione di violazioni di Active Directory spesso trascurata è l'account di "persone molto importante" (o gli indirizzi VIP) in un'organizzazione. Account con privilegi sono destinati perché questi account possono concedere l'accesso a utenti malintenzionati, che consente loro di danneggiare o distruggere anche i sistemi di destinazione, come descritto in precedenza in questa sezione.  

### <a name="privilege-attached-active-directory-accounts"></a>Account di "Privilegio collegati" Active Directory  
"Collegato privilegio" account di Active Directory sono account di dominio che non sono state apportate i membri di tutti i gruppi che hanno il massimo livello di privilegi in Active Directory, ma invece sono stato concesso livelli elevati di privilegi su molti server e workstation nell'ambiente di. Questi account sono la maggior parte dei spesso basati su dominio gli account configurati per l'esecuzione di servizi in sistemi appartenenti a un dominio, in genere per le applicazioni in esecuzione in grandi sezioni dell'infrastruttura. Sebbene questi account senza privilegi in Active Directory, se sono stati concessi privilegi elevati in un numero elevato di sistemi, possono essere utilizzati per violare o persino distruggere i segmenti dell'infrastruttura, per ottenere lo stesso effetto compromissione di un account con privilegi di Active Directory di grandi dimensioni.  
