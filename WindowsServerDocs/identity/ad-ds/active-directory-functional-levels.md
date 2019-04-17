---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: "Livelli di funzionalità di Windows Server 2016"
description: 
author: MicrosoftGuyJFlo
ms.author: joflore
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: a39955cf088ce7ce8bef20c70b83d49c6d508497
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="forest-and-domain-functional-levels"></a>Livelli di funzionalità foresta e dominio

>Si applica a: Windows Server

Con la fine del ciclo di vita di Windows 2003, Windows 2003 controller di dominio (DC) devono essere aggiornati a Windows Server 2008, 2012 o 2016. Di conseguenza, qualsiasi controller di dominio che esegue Windows Server 2003 devono essere rimossi dal dominio. Il livello di funzionalità del dominio e foresta deve essere generato almeno Windows Server 2008 per evitare che un controller di dominio che esegue una versione precedente di Windows Server venga aggiunto all'ambiente.

È consigliabile che i clienti aggiornare il livello di funzionalità del dominio (livello) e livello di funzionalità della foresta (FFL) come parte di questo, dato che il livello di funzionalità 2003 e FFL sono state deprecate in Windows Server 2016 e non verrà non sono più supportati in futuro rilascia.

Per i clienti che hanno l'esigenza di tempo aggiuntivo per valutare lo spostamento le funzionalità del dominio e FFL da 2003, il livello di funzionalità 2003 e FFL continuerà a essere supportato con Windows 10 e Windows Server 2016 fornite tutti i controller di dominio nel dominio e foresta si trovano in Windows Server 2008, 2008 R2, 2012, 2012 R2, o 2016.

In Windows Server 2008 e più livelli di funzionalità dominio, replica del file system distribuito (DFS, Distributed File Service) viene utilizzata per replicare i contenuti della cartella SYSVOL tra i controller di dominio. Se si crea un nuovo dominio a livello funzionale di dominio di Windows Server 2008 o versione successiva, replica DFS viene automaticamente utilizzata per replicare SYSVOL. Se hai creato il dominio a un livello funzionale inferiore, è necessario eseguire la migrazione da utilizza FRS a replica DFS per SYSVOL. Per la procedura di migrazione, è possibile seguire la [procedure su TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) o è possibile fare riferimento al [semplificata di set di passaggi nel blog sui File CAB del Team di archiviazione](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

I livelli di funzionalità dominio e foresta Windows Server 2003 continuano a essere supportato, ma le organizzazioni devono aumentare il livello di funzionalità a Windows Server 2008 (o versione successiva, se possibile) per garantire la compatibilità di replica di SYSVOL e il supporto in futuro. Inoltre, sono disponibili molti altri vantaggi e funzionalità a più livelli di funzionalità superiore. Le risorse seguenti per ulteriori informazioni, vedere:

## <a name="windows-server-2016"></a>Windows Server 2016

Sistema operativo Controller di dominio supportati:

* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Funzionalità a livello funzionalità foresta Windows Server 2016

* Tutte le funzionalità disponibili a livello funzionale di Windows Server 2012 R2 insieme di strutture e le funzionalità seguenti, sono disponibili:
    * [Privileged access management (PAM) con Microsoft Identity Manager (MIM)] (https://docs.microsoft.com/windows-server/identity/what's-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Funzionalità a livello funzionale del dominio Windows Server 2016

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità dal livello di funzionalità dominio di Windows Server 2012 R2, più le funzionalità seguenti:
    * I controller di dominio possono supportare i segreti NTLM pubblico chiave solo di un utente in sequenza. 
    * Quando un utente è limitata a specifici dispositivi aggiunti a un dominio, i controller di dominio possono supportare rete consentendo NTLM. 
    * I client Kerberos correttamente l'autenticazione con l'estensione della validità PKInit riceveranno l'identità di chiave pubblica nuovo SID.

    Per ulteriori informazioni vedere [What's New in Kerberos Authentication](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) e [What's new in protezione delle credenziali](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012 R2

Sistema operativo Controller di dominio supportati:

* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Funzionalità livello funzionalità foresta Windows Server 2012 R2

* Tutte le funzionalità disponibili in Windows Server 2012 funzionalità di livello, ma non altre funzionalità della foresta.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Funzionalità livello funzionalità dominio Windows Server 2012 R2

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità di livello di funzionalità del dominio Windows Server 2012, più le funzionalità seguenti:
    * Protezioni sul lato controller di dominio per utenti protetti. Protetti gli utenti l'autenticazione a un dominio non è più possibile di Windows Server 2012 R2:
        * Eseguire l'autenticazione con l'autenticazione NTLM
        * Utilizzare i pacchetti di crittografia DES o RC4 nella preautenticazione Kerberos
        * Delega vincolata o non usare la delega
        * Rinnovare i ticket utente (TGT) oltre la durata iniziale di 4 ore
    * Criteri di autenticazione
        * Basato sul nuovo insieme di strutture Active Directory i criteri che possono essere applicati ad account in domini di Windows Server 2012 R2 per controllare quali host un account può sign-on da e applicare condizioni di controllo di accesso per l'autenticazione ai servizi in esecuzione come account.
    * Silo di criteri di autenticazione
        * Nuovo insieme di strutture-oggetto basato su Active Directory, che è possibile creare una relazione tra utente, servizi gestiti e computer, account da utilizzare per classificare gli account per i criteri di autenticazione o per l'isolamento di autenticazione.

## <a name="windows-server-2012"></a>Windows Server 2012

Sistema operativo Controller di dominio supportati:

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Funzionalità a livello funzionalità foresta Windows Server 2012

* Tutte le funzionalità disponibili in Windows Server 2008 R2 le funzionalità di livello, ma non altre funzionale della foresta.

### <a name="windows-server-2012-domain-functional-level-features"></a>Funzionalità a livello funzionale del dominio Windows Server 2012

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità dal livello di funzionalità dominio di Windows Server 2008 R2, più le funzionalità seguenti:
    * Il supporto KDC per attestazioni, autenticazione composta e blindatura criteri modelli amministrativi KDC hanno due impostazioni (sempre forniscono attestazioni e Rifiuta richieste di autenticazione non blindate) che richiedono il livello di funzionalità dominio Windows Server 2012 Kerberos. Per ulteriori informazioni, vedere [What's New in Kerberos Authentication](https://technet.microsoft.com/en-us/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008 R2

Sistema operativo Controller di dominio supportati:

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Funzionalità livello funzionalità foresta Windows Server 2008 R2

* Tutte le funzionalità disponibili in Windows Server 2003 foresta a livello di funzionalità, più le funzionalità seguenti:
    * Cestino per Active Directory, che fornisce la possibilità di ripristinare completamente gli oggetti eliminati mentre è in esecuzione servizi di dominio Active Directory.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Funzionalità livello funzionalità dominio Windows Server 2008 R2

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello di funzionalità dominio Windows Server 2008, più le funzionalità seguenti:
    * Verifica del meccanismo di autenticazione, che consente di raccogliere informazioni sul tipo di metodo di accesso (smart card oppure nome utente e password) utilizzato per autenticare gli utenti di dominio nel token Kerberos di ogni utente. Quando questa funzionalità è abilitata in un ambiente di rete a cui è distribuita un'infrastruttura di gestione delle identità federative, ad esempio Active Directory Federation Services (ADFS), le informazioni nel token possono essere estratte ogni volta che un utente tenta di accedere a qualsiasi applicazione in grado di riconoscere attestazioni sviluppata per determinare le autorizzazioni in base al metodo di accesso dell'utente.
    * Gestione di SPN automatica per i servizi in esecuzione su un particolare computer nel contesto di un Account del servizio gestito quando il nome o host DNS nome account del computer. Per ulteriori informazioni sugli account del servizio gestiti, vedere [gli account del servizio Step-by-Step Guida](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Sistema operativo Controller di dominio supportati:

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Funzionalità a livello funzionalità foresta Windows Server 2008

* È disponibili tutte le funzionalità disponibili a livello funzionale di foresta di Windows Server 2003, ma nessuna funzionalità aggiuntiva. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Funzionalità a livello funzionale del dominio Windows Server 2008

* Tutti del valore predefinito di dominio Active Directory funzionalità, tutte le funzionalità a livello funzionale del dominio di Windows Server 2003, e sono disponibili le funzionalità seguenti:
    * Supporto della replica DFS (File System) per Windows Server 2003 System Volume (SYSVOL) distribuita
        * Supporto della replica DFS consente una replica più affidabile e dettagliata del contenuto SYSVOL.
        [!NOTE]>
        >A partire da Windows Server 2012 R2, servizio Replica File (FRS) è deprecato. Un nuovo dominio creato in un controller di dominio che esegua almeno Windows Server 2012 R2 deve essere impostata sul livello funzionale di dominio di Windows Server 2008 o versione successiva.

    * Basato su dominio spazi dei nomi DFS in esecuzione in modalità Windows Server 2008, che include il supporto per l'enumerazione basata sull'accesso e una maggiore scalabilità. Spazi dei nomi basati su dominio in modalità Windows Server 2008 richiedono anche la foresta di usare il livello di funzionalità foresta Windows Server 2003. Per ulteriori informazioni, vedere [scegliere un tipo di Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).
    * Supporto avanzato Encryption Standard (AES 128 e AES 256) per il protocollo Kerberos. Affinché i ticket di concessione ticket essere rilasciati con AES, il livello di funzionalità del dominio deve essere Windows Server 2008 o versione successiva e la password di dominio deve essere modificato. 
        * Per ulteriori informazioni, vedere [miglioramenti a Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).
        [!NOTE]>
        >Errori di autenticazione possono verificarsi in un controller di dominio dopo il livello di funzionalità del dominio viene generato a Windows Server 2008 o versione successiva se il controller di dominio ha già replicato la modifica di funzionalità del dominio ma non è stato ancora aggiornato la password krbtgt. In questo caso, riavviare il servizio KDC nel controller di dominio verrà avviare un aggiornamento in memoria la nuova password krbtgt e risolvere gli errori di autenticazione correlati.

    * [Ultimo accesso interattivo](https://go.microsoft.com/fwlink/?LinkId=180387) informazioni vengono visualizzate le informazioni seguenti:
        * Il numero totale di tentativi di accesso a un server di Windows Server 2008 aggiunto al dominio o una workstation Windows Vista
        * Il numero totale di tentativi di accesso non riusciti dopo l'accesso a un server Windows Server 2008 o una workstation Windows Vista
        * L'ora dell'ultimo tentativo di accesso non riusciti a Windows Server 2008 o una workstation Windows Vista
        * Tentare l'ora dell'ultimo accesso riuscito a un server Windows Server 2008 o una workstation Windows Vista
    * Criteri granulari per le password consentono di specificare criteri di blocco degli account e password per gli utenti e gruppi di sicurezza globali in un dominio. Per ulteriori informazioni, vedere [Guida dettagliata alla configurazione dei criteri di blocco Account e Password di specifici](https://go.microsoft.com/fwlink/?LinkID=91477).
    * Desktop virtuali personali
        * Per usare la funzionalità aggiunta fornita dalla scheda Desktop virtuale personale nella finestra di dialogo Proprietà Account utente in Active Directory Users and Computers, è necessario estendere lo schema di Active Directory per Windows Server 2008 R2 (versione oggetto dello schema = 47). Per ulteriori informazioni, vedere [la distribuzione di desktop virtuali personali utilizzando Connessione RemoteApp e Desktop Step-by-Step Guida](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Sistema operativo Controller di dominio supportati:

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Funzionalità di livello funzionalità foresta Windows Server 2003

* Tutte le funzionalità predefinite di dominio Active Directory e le funzionalità seguenti, sono disponibili:
    * Trust tra foreste
    * Ridenominazione del dominio
    * Replica del valore collegato
        - Replica del valore collegato consente di modificare l'appartenenza al gruppo per archiviare e replicare i valori per i singoli membri anziché replicare l'intera appartenenza come unità singola. Archiviare e replicare i valori dei singoli membri Usa meno cicli del processore durante la replica e minore larghezza di banda di rete e impedisce la perdita di aggiornamenti quando si aggiungono o rimuovono più membri simultaneamente per diversi controller di dominio.
    * La possibilità di distribuire un controller di dominio di sola lettura (RODC)
    * Algoritmi di controllo di coerenza informazioni (KCC) e la scalabilità migliorati
        - Il generatore della topologia tra siti (ISTG) utilizza algoritmi migliorati che si adattano per supportare le foreste con un numero maggiore di siti che di dominio Active Directory può supportare al livello di funzionalità foresta Windows 2000. L'algoritmo di elezione migliorato è un meccanismo meno invasivo per scegliere l'ISTG a livello funzionale di foresta Windows 2000.
    * La possibilità di creare istanze della classe ausiliaria dinamica denominata **dynamicObject** in una partizione di directory dominio
    * La possibilità di convertire un **inetOrgPerson** istanza dell'oggetto in un **utente** istanza dell'oggetto e completare la conversione nella direzione opposta
    * La possibilità di creare istanze di nuovi tipi di gruppo per il supporto di autorizzazione basata su ruoli. 
        - Questi tipi sono denominati gruppi applicazione di base e gruppi di query LDAP.
    * Disattivazione e ridefinizione degli attributi e classi nello schema. Gli attributi seguenti possono essere riutilizzati: ldapDisplayName, schemaIdGuid, OID e mapiID.
    * Basato su dominio spazi dei nomi DFS in esecuzione in modalità Windows Server 2008, che include il supporto per l'enumerazione basata sull'accesso e una maggiore scalabilità. Per ulteriori informazioni, vedere [scegliere un tipo di Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Funzionalità a livello funzionale del dominio Windows Server 2003

* Sono disponibili tutte le funzionalità predefinite di dominio Active Directory, tutte le funzionalità disponibili a livello funzionale di dominio nativo di Windows 2000 e le funzionalità seguenti:
    * Lo strumento di gestione del dominio, Netdom.exe, che rende possibile la ridenominazione dei controller di dominio
    * Gli aggiornamenti di timestamp di accesso
        * Il **lastLogonTimestamp** attributo viene aggiornato con l'ora dell'ultimo accesso dell'utente o computer. Questo attributo viene replicato all'interno del dominio.
    * La possibilità di impostare il **userPassword** attributo come password efficace **inetOrgPerson** e gli oggetti utente
    * La possibilità di reindirizzare gli utenti e computer di contenitori
        * Per impostazione predefinita, sono disponibili due contenitori noti per l'inserimento di account utente e computer, vale a dire, cn = Computers,<domain root> e cn = Users,<domain root>. Questa funzionalità consente la definizione di una nuova posizione nota per questi account.
    * La possibilità per Gestione autorizzazioni per archiviare i criteri di autorizzazione di dominio Active Directory
    * Delega vincolata
        * La delega vincolata consente alle applicazioni di sfruttare la delega sicura delle credenziali utente mediante l'autenticazione basata su Kerberos.
        * È possibile limitare la delega solo servizi di destinazione specifici.
    * Autenticazione selettiva
        * Autenticazione selettiva rende possibile è possibile specificare utenti e gruppi da una foresta trusted autorizzati a eseguire l'autenticazione ai server di risorse in una foresta trusting.

## <a name="windows-2000"></a>Windows 2000

Sistema operativo Controller di dominio supportati:

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Funzionalità livello funzionalità foresta nativo di Windows 2000

* Tutte le funzionalità predefinita AD DS sono disponibili.

### <a name="windows-2000-native-domain-functional-level-features"></a>Funzionalità a livello funzionale del dominio nativo Windows 2000

* Tutte le funzionalità predefinite di dominio Active Directory e le directory seguenti sono disponibili tra cui:
    * Gruppi universali per gruppi di distribuzione e di sicurezza.
    * Nidificazione dei gruppi
    * Conversione dei gruppi, che consente la conversione tra gruppi di sicurezza e di distribuzione
    * Cronologia ID (SID) di sicurezza

## <a name="next-steps"></a>Passaggi successivi

* [Aumentare il livello funzionale di dominio](https://technet.microsoft.com/library/cc753104.aspx)  
* [Aumentare il livello funzionale di foresta](https://technet.microsoft.com/library/cc730985.aspx)
