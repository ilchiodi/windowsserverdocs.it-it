---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Livelli di funzionalità di Windows Server 2016
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: cb9b5b9448f364760c3d2a7e43edd01a5a9f7f9d
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719671"
---
# <a name="forest-and-domain-functional-levels"></a>Livelli di funzionalità foresta e dominio

>Si applica a: Windows Server

Livelli di funzionalità di determinano le funzionalità di dominio o foresta di Active Directory Domain Services (AD DS) disponibili. È inoltre possibile determinare quali sistemi operativi Windows Server è possibile eseguire nei controller di dominio nel dominio o foresta. Livelli di funzionalità, tuttavia, non interessano i sistemi operativi è possibile eseguire nella workstation e server membro che sono connessi al dominio o foresta.

Quando si distribuisce Active Directory Domain Services, impostare i livelli di funzionalità foresta e dominio per il valore più alto che può supportare l'ambiente. In questo modo, è possibile usare tutte le funzionalità di Active Directory Domain Services possibili. Quando si distribuisce una nuova foresta, viene chiesto di impostare il livello funzionale della foresta e quindi impostare il livello funzionale del dominio. È possibile impostare il livello funzionale del dominio su un valore superiore a livello di funzionalità foresta, ma non è possibile impostare il livello funzionale del dominio su un valore inferiore rispetto a livello di funzionalità foresta.

Con la fine del ciclo di vita di Windows 2003, Windows 2003 controller di dominio (DC) dovranno essere aggiornati a Windows Server 2008, 2008 R2, 2012, 2012 R2, 2016 o 2019. Di conseguenza, qualsiasi controller di dominio che esegue Windows Server 2003 devono essere rimossi dal dominio.

In Windows Server 2008 e più livelli funzionali di dominio, replica del file system distribuito (DFS, Distributed File Service) viene utilizzata per la replica di contenuto della cartella SYSVOL tra i controller di dominio. Se si crea un nuovo dominio a livello funzionale di dominio di Windows Server 2008 o versione successiva, replica DFS viene automaticamente utilizzata per replicare SYSVOL. Se è stato creato il dominio a un livello funzionale inferiore, è necessario eseguire la migrazione da utilizza FRS a replica DFS per SYSVOL. Per la procedura di migrazione, è possibile seguire il [procedure su TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) o è possibile fare riferimento il [semplificata di set di passaggi nel blog del Team di archiviazione File CAB](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

## <a name="windows-server-2019"></a>Windows Server 2019

Non esistono nuova foresta o livelli funzionali del dominio aggiunti in questa versione.

Il requisito minimo per aggiungere un Controller di dominio di Windows Server 2019 è un livello di funzionalità di Windows Server 2008. Inoltre, il dominio deve utilizzare DFS-R come motore per la replica di SYSVOL.

## <a name="windows-server-2016"></a>Windows Server 2016

Sistema operativo di Controller di dominio supportati:

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Funzionalità a livello funzionale di Windows Server 2016 dell'insieme di strutture

* Tutte le funzionalità disponibili a livello funzionale di Windows Server 2012R2 foresta e le funzionalità seguenti, disponibili:
   * [Gestione dell'accesso con privilegi (PAM) mediante Microsoft Identity Manager (MIM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Funzionalità a livello funzionalità dominio di Windows Server 2016

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello di funzionalità dominio di Windows Server 2012 R2, più le funzionalità seguenti:
   * I controller di dominio possono supportare il rollover automatico di NTLM e altri segreti basato su password in un account utente configurato per richiedere l'autenticazione PKI. Questa configurazione è nota anche come "Occorre una Smart card per l'accesso interattivo"
   * I controller di dominio può supportare la rete che consente NTLM quando un utente è limitato a specifici dispositivi aggiunti a un dominio.
   * I client Kerberos correttamente l'autenticazione con l'estensione Freshness PKInit ottengono l'identità di chiave pubblica nuova SID.

    Per altre informazioni, vedere [What ' s New in Kerberos Authentication](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) e [nuove funzionalità di protezione delle credenziali](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

Sistema operativo di Controller di dominio supportati:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Funzionalità a livello funzionalità foresta Windows Server 2012 R2

* Tutte le funzionalità disponibili in Windows Server 2012 a livello, ma non altre funzionalità della foresta.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Funzionalità a livello del funzionale di dominio di Windows Server 2012 R2

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello funzionale di dominio Windows Server 2012, più le funzionalità seguenti:
   * Protezioni sul lato controller di dominio per utenti protetti. Protetti agli utenti l'autenticazione a un dominio non è più possibile di Windows Server 2012 R2:
      * Eseguire l'autenticazione con l'autenticazione NTLM
      * Usare pacchetti di crittografia DES o RC4 nella preautenticazione Kerberos
      * Delega vincolata o non usare la delega
      * Rinnovare i ticket utente (TGT) oltre la durata iniziale di 4 ore
   * Authentication Policies
      * Nuova foresta Active Directory i criteri che possono essere applicati agli account nei domini Windows Server 2012 R2 per controllare quali host un account può sign-on da e applicare condizioni di controllo di accesso per l'autenticazione ai servizi in esecuzione come account.
   * Authentication Policy Silos
      * Nuova foresta Active Directory oggetti, che è possono creare una relazione tra utente, servizi gestiti e computer, gli account da usare per classificare gli account per i criteri di autenticazione o per l'isolamento di autenticazione.

## <a name="windows-server-2012"></a>Windows Server 2012

Sistema operativo di Controller di dominio supportati:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Funzionalità a livello funzionalità foresta di Windows Server 2012

* Tutte le funzionalità disponibili in Windows Server 2008 R2 le funzionalità di livello, ma non altre funzionale della foresta.

### <a name="windows-server-2012-domain-functional-level-features"></a>Funzionalità a livello di Windows Server 2012 dominio funzionale

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello funzionale di Windows Server 2008 R2 dominio, più le funzionalità seguenti:
   * Il supporto KDC per attestazioni, autenticazione composta e blindatura criteri dei modelli amministrativi KDC ha due impostazioni (sempre forniscono attestazioni e Rifiuta richieste di autenticazione non blindate) che richiedono Windows Server 2012 dominio funzionale livello Kerberos. Per altre informazioni, vedere [What ' s New in Kerberos Authentication](https://technet.microsoft.com/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

Sistema operativo di Controller di dominio supportati:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Funzionalità a livello funzionalità foresta Windows Server 2008 R2

* Tutte le funzionalità disponibili per il livello di funzionalità della foresta Windows Server 2003, più le funzionalità seguenti:
   * Cestino di Active Directory, che consente di ripristinare completamente gli oggetti eliminati mentre Servizi di dominio Active Directory è in esecuzione.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Windows Server 2008 R2 dominio livelli di funzionalità

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello di funzionalità dominio Windows Server 2008, più le funzionalità seguenti:
   * Verifica del meccanismo di autenticazione, che consente di raccogliere informazioni sul tipo di metodo di accesso (smart card oppure nome utente/password) utilizzato per l'autenticazione degli utenti di dominio nel token Kerberos di ogni utente. Quando questa funzionalità è abilitata in un ambiente di rete che ha sviluppato un'infrastruttura di gestione delle identità federative, ad esempio Active Directory Federation Services (ADFS), le informazioni nel token possono essere estratte ogni volta che un utente prova ad accedere a qualsiasi applicazione di riconoscere attestazioni sviluppata per determinare le autorizzazioni basate sul metodo di accesso dell'utente.
   * Gestione di SPN automatica per i servizi in esecuzione su un computer specifico nel contesto di un Account del servizio gestito quando il nome o il DNS al nome delle modifiche account macchina host. Per altre informazioni sugli account del servizio gestito, vedere [Guida dettagliata agli account di servizio](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Sistema operativo di Controller di dominio supportati:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Funzionalità a livello funzionale di Windows Server 2008 foresta

* È disponibili tutte le funzionalità disponibili a livello funzionale di foresta di Windows Server 2003, ma nessuna funzionalità aggiuntiva. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Funzionalità a livello funzionalità dominio di Windows Server 2008

* Tutti del valore predefinito di dominio Active Directory features, tutte le funzionalità a livello funzionale di dominio di Windows Server 2003, e sono disponibili le seguenti funzionalità:
  * Supporto della replica Distributed File System (DFS) per Windows Server 2003 System Volume (SYSVOL)
    * Supporto della replica DFS consente una replica più affidabile e dettagliata del contenuto SYSVOL.

      > [!NOTE]
      > A partire da Windows Server 2012 R2, servizio Replica File (FRS) è deprecato. Un nuovo dominio creato in un controller di dominio che esegue almeno Windows Server 2012 R2 deve essere impostato sul livello funzionale di dominio di Windows Server 2008 o versione successiva.

  * Basato su dominio spazi dei nomi DFS in esecuzione in modalità Windows Server 2008, che include il supporto per l'enumerazione basata sull'accesso e una maggiore scalabilità. Spazi dei nomi basati su dominio in modalità Windows Server 2008 richiedono anche la foresta di usare il livello di funzionalità foresta Windows Server 2003. Per altre informazioni, vedere [scegliere un tipo di Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).
  * Advanced Encryption Standard (AES 128 e AES a 256) supporto per il protocollo Kerberos. Affinché TGT essere eseguiti utilizzando AES, il livello funzionale del dominio deve essere Windows Server 2008 o versione successiva e la password del dominio deve essere modificato. 
    * Per altre informazioni, vedere [miglioramenti a Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).

      > [!NOTE]
      >Errori di autenticazione possono verificarsi in un controller di dominio dopo viene generato il livello funzionale del dominio a Windows Server 2008 o versione successiva se il controller di dominio è già stata replicata la modifica di funzionalità del dominio ma non è stato ancora aggiornato la password krbtgt. In questo caso, riavviare il servizio KDC nel controller di dominio verrà attivano un aggiornamento in memoria della nuova password krbtgt e risolvere gli errori di autenticazione correlate.

  * [Ultimo accesso interattivo](https://go.microsoft.com/fwlink/?LinkId=180387) informazioni vengono visualizzate le informazioni seguenti:
     * Il numero totale di tentativi di accesso a un server di Windows Server 2008 aggiunto al dominio o una workstation Windows Vista
     * Il numero totale di tentativi di accesso non riuscito dopo l'accesso a un server Windows Server 2008 o una workstation Windows Vista
     * L'ora dell'ultimo tentativo di accesso non riuscito a Windows Server 2008 o una workstation Windows Vista
     * Data e ora dell'ultimo accesso riuscito tentativo a un server Windows Server 2008 o una workstation Windows Vista
  * I criteri granulari per le password consentono di specificare i criteri di blocco degli account e password per gli utenti e gruppi di sicurezza globali in un dominio. Per altre informazioni, vedere [Guida dettagliata alla configurazione dei criteri di blocco degli Account e Password specifici per le](https://go.microsoft.com/fwlink/?LinkID=91477).
  * Desktop virtuali personali
     * Per usare la funzionalità aggiuntiva fornita dalla scheda Desktop virtuale personale nella finestra di dialogo Proprietà Account utente in Active Directory Users and Computers, deve essere esteso lo schema di Active Directory Domain Services per Windows Server 2008 R2 (versione dell'oggetto schema = 47). Per altre informazioni, vedere [distribuzione di desktop virtuali personali utilizzando Connessione RemoteApp e Guida dettagliata alla connessione di Desktop](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Sistema operativo di Controller di dominio supportati:

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Funzionalità a livello funzionalità foresta di Windows Server 2003

* Tutte le funzionalità predefinite di Active Directory Domain Services e le funzionalità seguenti, disponibili:
   * Trust tra foreste
   * Ridenominazione dei domini
   * Replica del valore collegato
      - Replica del valore collegato consente di modificare l'appartenenza al gruppo per archiviare e replicare i valori per i singoli membri anziché replicare l'intera appartenenza come unità singola. Archiviazione e i valori dei singoli membri di replica Usa minore larghezza di banda di rete e un minor numero di cicli del processore durante la replica e impedisce la perdita di aggiornamenti quando si aggiungono o rimuovono più membri simultaneamente in diversi controller di dominio.
   * La possibilità di distribuire un controller di dominio di sola lettura (RODC)
   * Algoritmi di controllo di coerenza informazioni (KCC) e scalabilità migliorati
      - Il generatore della topologia tra siti (ISTG) utilizza algoritmi migliorati che si adattano per supportare le foreste con un maggior numero di siti di Active Directory Domain Services può supportare al livello di funzionalità foresta Windows 2000. L'algoritmo di elezione migliorato è un meccanismo meno invasivo per scegliere l'ISTG a livello di funzionalità foresta Windows 2000.
   * La possibilità di creare istanze della classe ausiliaria dinamica denominata **dynamicObject** in una partizione di directory dominio
   * La capacità di convertire un' **inetOrgPerson** istanza dell'oggetto in un **utente** istanza dell'oggetto e completare la conversione nella direzione opposta
   * La possibilità di creare istanze dei nuovi tipi di gruppo per supportare l'autorizzazione basata sui ruoli. 
      - Questi tipi sono chiamati gruppi di base dell'applicazione e i gruppi di query LDAP.
   * Disattivazione e ridefinizione degli attributi e delle classi nello schema. Gli attributi seguenti possono essere riutilizzati: ldapDisplayName, schemaIdGuid, OID e mapiID.
   * Basato su dominio spazi dei nomi DFS in esecuzione in modalità Windows Server 2008, che include il supporto per l'enumerazione basata sull'accesso e una maggiore scalabilità. Per altre informazioni, vedere [scegliere un tipo di Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Funzionalità a livello funzionale di Windows Server 2003 domain

* Sono disponibili tutte le funzionalità predefinite di Active Directory Domain Services, tutte le funzionalità disponibili a livello funzionale di dominio nativo di Windows 2000 e le funzionalità seguenti:
   * Lo strumento di gestione del dominio, Netdom.exe, che rende possibile la ridenominazione dei controller di dominio
   * Aggiorna indicatore data e ora di accesso
      * L'attributo **lastLogonTimestamp** viene aggiornato con l'ora dell'ultimo accesso da parte dell'utente o del computer. Questo attributo viene replicato all'interno del dominio.
   * Possibilità di impostare il **userPassword** attributo come password efficace **inetOrgPerson** e gli oggetti utente
   * La possibilità di reindirizzare gli utenti e computer di contenitori
      * Per impostazione predefinita, vengono forniti due contenitori noti per l'inserimento di account utente e computer, vale a dire, cn = Computers<domain root> e cn = Users,<domain root>. Questa funzionalità consente la definizione di una nuova posizione nota per questi account.
   * La possibilità per Gestione autorizzazioni per archiviare i relativi criteri di autorizzazione in Active Directory Domain Services
   * Delega vincolata
      * La delega vincolata consente alle applicazioni di sfruttare la delega sicura delle credenziali utente mediante l'autenticazione basata su Kerberos.
      * È possibile limitare la delega ai servizi di destinazione specifico solo.
   * Autenticazione selettiva
      * Rende l'autenticazione selettiva che è possibile specificare utenti e gruppi da una foresta trusted autorizzati a eseguire l'autenticazione ai server di risorse in una foresta trusting.

## <a name="windows-2000"></a>Windows 2000

Sistema operativo di Controller di dominio supportati:

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Funzionalità a livello funzionale di Windows 2000 nativo foresta

* Tutte le funzionalità di dominio Active Directory predefinita di AD sono disponibili.

### <a name="windows-2000-native-domain-functional-level-features"></a>Funzionalità a livello funzionale di Windows 2000 nativo dominio

* Tutte le funzionalità predefinite di Active Directory Domain Services e le directory seguenti sono disponibili tra cui:
   * Gruppi universali per i gruppi di distribuzione e la sicurezza.
   * Nidificazione dei gruppi
   * Conversione dei gruppi, che consente la conversione tra gruppi di sicurezza e distribuzione
   * Cronologia ID (SID) di sicurezza

## <a name="next-steps"></a>Passaggi successivi

* [Aumentare il livello funzionale di dominio](https://technet.microsoft.com/library/cc753104.aspx)  
* [Aumentare il livello funzionale di foresta](https://technet.microsoft.com/library/cc730985.aspx)
