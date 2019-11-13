---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: Account che possono favorire il furto di credenziali
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2d6fdeaef6e8c90d5e723bdd3ee334369ef73c68
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367739"
---
# <a name="attractive-accounts-for-credential-theft"></a>Account che possono favorire il furto di credenziali

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gli attacchi per il furto di credenziali sono quelli in cui un utente malintenzionato ottiene inizialmente i privilegi più elevati (radice, amministratore o sistema, a seconda del sistema operativo in uso) che accedono a un computer in rete e quindi utilizza gli strumenti disponibili gratuitamente per estrarre le credenziali dalle sessioni di altri account connessi. A seconda della configurazione del sistema, queste credenziali possono essere estratte sotto forma di hash, ticket o persino password in testo non crittografato. Se una delle credenziali raccolte riguarda gli account locali che possono esistere in altri computer della rete (ad esempio, gli account amministratore in Windows o gli account radice in OSX, UNIX o Linux), l'autore dell'attacco presenta le credenziali ad altri computer in la rete per propagare la compromissione a computer aggiuntivi e per tentare di ottenere le credenziali di due tipi specifici di account:  

1.  Account di dominio con privilegi con privilegi ampi e profondi (ovvero account con privilegi a livello di amministratore in molti computer e in Active Directory). Questi account non possono essere membri di uno dei gruppi con privilegi più elevati in Active Directory, ma è possibile che siano stati concessi privilegi a livello di amministratore in molti server e workstation del dominio o della foresta, rendendoli efficaci come membri dei gruppi con privilegi in Active Directory. Nella maggior parte dei casi, gli account a cui sono stati concessi livelli elevati di privilegi in vaste aree dell'infrastruttura Windows sono account del servizio, quindi gli account di servizio devono essere sempre valutati per l'ampiezza e la profondità dei privilegi.  

2.  Account di dominio di tipo "persona molto importante" (VIP). Nel contesto di questo documento, un account VIP è un account che ha accesso alle informazioni desiderate da un utente malintenzionato (proprietà intellettuale e altre informazioni riservate) o qualsiasi account che può essere usato per concedere all'utente malintenzionato l'accesso a tali informazioni. Esempi di questi account utente includono:  

    1.  Dirigenti i cui account hanno accesso a informazioni aziendali riservate  

    2.  Account per il personale del supporto tecnico responsabile della gestione dei computer e delle applicazioni utilizzate dai dirigenti  

    3.  Account per gli addetti legali che hanno accesso ai documenti dell'offerta e del contratto di un'organizzazione, indipendentemente dal fatto che i documenti siano per la propria organizzazione o per le organizzazioni client  

    4.  Pianificatori di prodotti che hanno accesso a piani e specifiche per i prodotti della pipeline di sviluppo di una società, indipendentemente dai tipi di prodotti che la società crea  

    5.  Ricercatori i cui account vengono usati per accedere ai dati di studio, alle formulazioni dei prodotti o a qualsiasi altra ricerca di interesse per un utente malintenzionato  

Poiché gli account con privilegi elevati in Active Directory possono essere usati per propagare la compromissione e modificare gli account VIP o i dati a cui possono accedere, gli account più utili per gli attacchi per il furto di credenziali sono account membri di Enterprise Admins, Gruppi Domain Admins e Administrators in Active Directory.  

Poiché i controller di dominio sono i repository per il database di servizi di dominio Active Directory e i controller di dominio hanno accesso completo a tutti i dati in Active Directory, i controller di dominio sono destinati anche a compromessi, sia in parallelo con attacchi con furto di credenziali che dopo uno o più account Active Directory con privilegi elevati sono stati compromessi. Sebbene numerose pubblicazioni (e molti utenti malintenzionati) si concentrino sulle appartenenze ai gruppi Domain Admins quando si descrivono gli attacchi pass-the-hash e altri furti di credenziali (come descritto in [riduzione della superficie di attacco Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)), un account membro di uno dei gruppi elencati di seguito può essere utilizzato per compromettere l'intera installazione di servizi di dominio Active Directory.  

> [!NOTE]  
> Per informazioni complete su Pass-the-hash e altri attacchi per il furto di credenziali, vedere il white paper relativo alla [mitigazione degli attacchi pass-the-hash (PTH) e altre tecniche di furto delle credenziali](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf) elencate nell' [Appendice M: collegamenti a documenti e letture consigliate](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md). Per altre informazioni sugli attacchi determinati da avversari determinati, a volte definiti "minacce permanenti avanzate" (APTs), vedere [avversari determinati e attacchi mirati](https://www.microsoft.com/download/details.aspx?id=34793).  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>Attività che aumentano la probabilità di compromissione  
Poiché la destinazione del furto di credenziali è in genere costituita da account di dominio con privilegi elevati e account VIP, è importante che gli amministratori siano consapevoli delle attività che aumentano la probabilità di successo di un attacco di furto di credenziali. Anche se gli utenti malintenzionati hanno come destinazione gli account VIP, se agli indirizzi VIP non sono assegnati livelli elevati di privilegi nei sistemi o nel dominio, il furto delle credenziali richiede altri tipi di attacchi, ad esempio la progettazione di social networking per fornire informazioni segrete. In alternativa, l'utente malintenzionato deve ottenere l'accesso con privilegi a un sistema in cui vengono memorizzate nella cache le credenziali VIP. Per questo motivo, le attività che aumentano la probabilità di furto delle credenziali descritte di seguito si concentrano principalmente sulla prevenzione dell'acquisizione di credenziali amministrative con privilegi elevati. Queste attività sono meccanismi comuni mediante i quali gli utenti malintenzionati possono compromettere i sistemi per ottenere le credenziali con privilegi.  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>Accesso a computer non protetti con account con privilegi  
La vulnerabilità principale che consente di ottenere gli attacchi di furto delle credenziali è l'operazione di accesso ai computer che non sono protetti con gli account che hanno un ampio livello di privilegi e con un livello di privilegi elevato nell'intero ambiente. Questi accessi possono essere il risultato di varie configurazioni errate descritte qui.  

#### <a name="not-maintaining-separate-administrative-credentials"></a>Mancata gestione di credenziali amministrative separate  
Sebbene questo sia relativamente insolito, nella valutazione di varie installazioni di servizi di dominio Active Directory, i dipendenti IT sono stati rilevati con un unico account per tutto il lavoro. L'account è un membro di almeno uno dei gruppi con privilegi più elevati in Active Directory ed è lo stesso account utilizzato dai dipendenti per accedere alle proprie workstation al mattino, controllare la posta elettronica, esplorare i siti Internet e scaricare il contenuto computer. Quando gli utenti eseguono con account a cui sono concessi diritti e autorizzazioni di amministratore locale, espongono il computer locale per completare la compromissione. Quando tali account sono anche membri dei gruppi con privilegi più elevati in Active Directory, espongono l'intera foresta alla compromissione, rendendo estremamente semplice per un utente malintenzionato il controllo completo dell'ambiente Active Directory e di Windows.  

Analogamente, in alcuni ambienti è stato rilevato che gli stessi nomi utente e password vengono usati per gli account radice in computer non Windows, così come vengono usati nell'ambiente Windows, che consente agli utenti malintenzionati di estendere i compromessi dai sistemi UNIX o Linux ai sistemi Windows e viceversa.

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>Accessi a workstation compromesse o server membri con account con privilegi  
Quando si usa un account di dominio con privilegi elevati per accedere in modo interattivo a una workstation o a un server membro compromesso, il computer compromesso può raccogliere le credenziali da qualsiasi account che accede al sistema.  

#### <a name="unsecured-administrative-workstations"></a>Workstation amministrative non protette  
In molte organizzazioni, il personale IT utilizza più account. Un account viene usato per l'accesso alla workstation del dipendente e, poiché si tratta del personale IT, spesso hanno diritti di amministratore locale per le workstation. In alcuni casi, il controllo dell'account utente rimane abilitato in modo che l'utente riceva almeno un token di accesso suddiviso all'accesso e debba elevare i privilegi quando sono necessari i privilegi. Quando questi utenti eseguono attività di manutenzione, in genere usano gli strumenti di gestione installati localmente e forniscono le credenziali per gli account con privilegi di dominio, selezionando l'opzione **Esegui come amministratore** o fornendo le credenziali quando richiesto. Sebbene questa configurazione possa sembrare appropriata, espone l'ambiente alla compromissione perché:  

-   L'account utente "regolare" utilizzato dal dipendente per accedere alla propria workstation dispone dei diritti di amministratore locale, il computer è vulnerabile agli attacchi di [scaricamento delle unità](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx) in cui l'utente è convinto di installare malware.  

-   Il malware viene installato nel contesto di un account amministrativo, il computer ora può essere usato per acquisire le sequenze di tasti, il contenuto degli Appunti, le schermate e le credenziali residenti in memoria, che possono causare l'esposizione delle credenziali di un dominio potente account.  

I problemi in questo scenario sono duplici. Per prima cosa, anche se per l'amministrazione locale e di dominio vengono utilizzati account distinti, il computer non è protetto e non protegge gli account da eventuali furti. In secondo luogo, all'account utente regolare e all'account amministrativo sono stati concessi diritti e autorizzazioni eccessivi.  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>Esplorazione di Internet con un account con privilegi elevati  
Gli utenti che accedono a computer con account che sono membri del gruppo Administrators locale nel computer o membri di gruppi con privilegi in Active Directory e che quindi esplorano Internet (o una rete Intranet compromessa) espongono il computer locale e il Directory da compromettere.  

L'accesso a un sito Web creato in modo dannoso con un browser in esecuzione con privilegi amministrativi può consentire a un utente malintenzionato di depositare codice dannoso nel computer locale nel contesto dell'utente con privilegi. Se l'utente dispone di diritti di amministratore locale sul computer, gli utenti malintenzionati potrebbero ingannare l'utente nel scaricare codice dannoso o aprire allegati di posta elettronica che sfruttano le vulnerabilità delle applicazioni e sfruttano i privilegi dell'utente per estrarre localmente dalla cache credenziali per tutti gli utenti attivi nel computer. Se l'utente dispone di diritti amministrativi nella directory per appartenenza ai gruppi Enterprise Admins, Domain Admins o Administrators in Active Directory, l'autore dell'attacco può estrarre le credenziali del dominio e usarle per compromettere l'intero dominio o foresta di servizi di dominio Active Directory, senza la necessità di compromettere qualsiasi altro computer della foresta.  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>Configurazione di account con privilegi locali con le stesse credenziali tra i sistemi  
La configurazione dello stesso nome e della stessa password dell'account Administrator locale in molti o tutti i computer consente di eseguire il furto delle credenziali dal database SAM su un computer per compromettere tutti gli altri computer che utilizzano le stesse credenziali. Come minimo, è consigliabile utilizzare password diverse per gli account di amministratore locale in ogni sistema aggiunto al dominio. Gli account amministratore locale possono anche essere denominati in modo univoco, ma l'utilizzo di password diverse per gli account locali con privilegi di ogni sistema è sufficiente per garantire che le credenziali non possano essere utilizzate in altri sistemi.  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Sovrapopolazione e utilizzo eccessivo dei gruppi di dominio con privilegi  
La concessione dell'appartenenza ai gruppi EA, DA o BA in un dominio crea una destinazione per gli utenti malintenzionati. Maggiore è il numero di membri di questi gruppi, maggiore è la probabilità che un utente con privilegi possa inavvertitamente usare le credenziali in modo involontario ed esporle agli attacchi per il furto di credenziali. Ogni workstation o server a cui un utente di dominio con privilegi accede presenta un possibile meccanismo mediante il quale le credenziali dell'utente con privilegi possono essere raccolte e utilizzate per compromettere il dominio e la foresta di servizi di dominio Active Directory.  

### <a name="poorly-secured-domain-controllers"></a>Controller di dominio poco protetti  
I controller di dominio ospitano una replica del database di servizi di dominio Active Directory di un dominio. Nel caso di controller di dominio di sola lettura, la replica locale del database contiene le credenziali per solo un subset degli account della directory, per impostazione predefinita Nessuno degli account di dominio con privilegi. Nei controller di dominio di lettura/scrittura, ogni controller di dominio gestisce una replica completa del database di servizi di dominio Active Directory, incluse le credenziali non solo per utenti con privilegi quali Domain Admins, ma con account con privilegi quali gli account del controller di dominio o il krbtgt del dominio account, ovvero l'account associato al servizio KDC nei controller di dominio. Se le applicazioni aggiuntive non necessarie per la funzionalità del controller di dominio sono installate nei controller di dominio o se i controller di dominio non sono rigorosamente patchati e protetti, gli utenti malintenzionati potrebbero comprometterli tramite vulnerabilità senza patch o può sfruttare altri vettori di attacco per installare software dannoso direttamente su di essi.  

## <a name="privilege-elevation-and-propagation"></a>Innalzamento di livello e propagazione dei privilegi  
Indipendentemente dai metodi di attacco usati, Active Directory viene sempre indirizzato quando viene attaccato un ambiente Windows, perché controlla in ultima analisi l'accesso a tutti gli utenti malintenzionati. Questo non significa tuttavia che l'intera directory è di destinazione. Gli account, i server e i componenti dell'infrastruttura specifici rappresentano in genere gli obiettivi principali degli attacchi contro Active Directory. Questi account sono descritti di seguito.  

### <a name="permanent-privileged-accounts"></a>Account con privilegi permanenti  
Poiché l'introduzione di Active Directory, è possibile utilizzare gli account con privilegi elevati per creare la foresta Active Directory e quindi delegare i diritti e le autorizzazioni necessarie per eseguire l'amministrazione quotidiana per gli account con privilegi di livello inferiore. L'appartenenza ai gruppi Enterprise Admins, Domain Admins o Administrators in Active Directory è obbligatoria solo temporaneamente e raramente in un ambiente che implementa approcci con privilegi minimi per l'amministrazione giornaliera.  

Gli account con privilegi permanenti sono account che sono stati inseriti in gruppi con privilegi e che sono stati lasciati da un giorno all'altro. Se l'organizzazione inserisce cinque account nel gruppo Domain Admins per un dominio, questi cinque account possono essere assegnati a 24 ore al giorno, sette giorni alla settimana. Tuttavia, l'effettiva necessità di utilizzare gli account con privilegi Domain Admins è in genere solo per una configurazione specifica a livello di dominio e per brevi periodi di tempo.  

### <a name="vip-accounts"></a>Account VIP  
Una destinazione spesso trascurata in Active Directory violazioni è costituita dagli account di "persone molto importanti" (o VIP) in un'organizzazione. Gli account con privilegi sono assegnati perché tali account possono concedere l'accesso agli utenti malintenzionati, che consentono di compromettere o persino eliminare i sistemi di destinazione, come descritto in precedenza in questa sezione.  

### <a name="privilege-attached-active-directory-accounts"></a>Account Active Directory "con privilegi collegati"  
Gli account Active Directory "Privileged" sono account di dominio che non sono stati resi membri di nessuno dei gruppi con i livelli di privilegio più elevati in Active Directory, ma sono stati concessi livelli elevati di privilegi in molti server e workstation nell'ambiente. Questi account sono spesso account basati su dominio configurati per l'esecuzione di servizi nei sistemi aggiunti a un dominio, in genere per le applicazioni in esecuzione in sezioni estese dell'infrastruttura. Anche se questi account non dispongono di privilegi in Active Directory, se vengono concessi privilegi elevati su un numero elevato di sistemi, possono essere usati per compromettere o persino distruggere grandi segmenti dell'infrastruttura, ottenendo lo stesso effetto della compromissione di un account Active Directory con privilegi.  
