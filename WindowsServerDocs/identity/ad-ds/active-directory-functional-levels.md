---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Livelli di funzionalità di Windows Server 2016
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: 7f16d58eb6c5074c75f49ba7936c4d312a3dbda4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390985"
---
# <a name="forest-and-domain-functional-levels"></a>Livelli di funzionalità del dominio e della foresta

>Si applica a: Windows Server

I livelli di funzionalità determinano le funzionalità disponibili per il dominio o la foresta di Active Directory Domain Services (AD DS). Determinano anche quali sistemi operativi Windows Server puoi eseguire nei controller di dominio del dominio o della foresta. I livelli di funzionalità, tuttavia, non influiscono sui sistemi operativi che puoi eseguire nelle workstation e nei server membri aggiunti al dominio o alla foresta.

Quando distribuisci Active Directory Domain Service, imposta i livelli di funzionalità del dominio e della foresta sul valore massimo che può essere supportato dall'ambiente. In questo modo, puoi usare il maggior numero possibile di funzionalità di Active Directory Domain Service. Quando distribuisci una nuova foresta, ti viene richiesto di impostare il livello di funzionalità della foresta e quindi il livello di funzionalità del dominio. Puoi impostare il livello di funzionalità del dominio su un valore maggiore rispetto al livello di funzionalità della foresta, ma non su un valore minore.

Con la fine del ciclo di vita di Windows 2003, i controller di dominio Windows 2003 devono essere aggiornati a Windows Server 2008, 2008 R2, 2012, 2012 R2, 2016 o 2019. Di conseguenza, qualsiasi controller di dominio che esegue Windows Server 2003 devono essere rimossi dal dominio.

In Windows Server 2008 e più livelli funzionali di dominio, replica del file system distribuito (DFS, Distributed File Service) viene utilizzata per la replica di contenuto della cartella SYSVOL tra i controller di dominio. Se si crea un nuovo dominio a livello funzionale di dominio di Windows Server 2008 o versione successiva, replica DFS viene automaticamente utilizzata per replicare SYSVOL. Se è stato creato il dominio a un livello funzionale inferiore, è necessario eseguire la migrazione da utilizza FRS a replica DFS per SYSVOL. Per la procedura di migrazione, è possibile seguire il [procedure su TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) o è possibile fare riferimento il [semplificata di set di passaggi nel blog del Team di archiviazione File CAB](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

## <a name="windows-server-2019"></a>Windows Server 2019

In questa versione non sono stati aggiunti nuovi livelli di funzionalità della foresta o del dominio.

Il requisito minimo per aggiungere un controller di dominio Windows Server 2019 è un livello di funzionalità pari a Windows Server 2008. Il dominio deve anche usare DFS-R come motore per la replica di SYSVOL.

## <a name="windows-server-2016"></a>Windows Server 2016

Sistemi operativi supportati per i controller di dominio:

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta di Windows Server 2016

* Tutte le funzionalità disponibili per il livello di funzionalità della foresta di Windows Server 2012 R2, più le funzionalità seguenti:
   * [Privileged Access Management (PAM) con Microsoft Identity Manager (MIM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio di Windows Server 2016

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello di funzionalità del dominio di Windows Server 2012 R2, più le funzionalità seguenti:
   * I controller di dominio possono supportare il rolling automatico dei segreti NTLM e di altri segreti basati su password in un account utente configurato per l'autenticazione PKI. Questa configurazione è nota anche come "smart card necessaria per l'accesso interattivo".
   * I controller di dominio possono supportare NTLM di rete quando un utente si limita a specifici dispositivi appartenenti al dominio.
   * I client Kerberos che eseguono l'autenticazione con l'estensione di aggiornamento PKInit otterranno il SID di identità della chiave pubblica aggiornata.

    Per altre informazioni, vedi [Novità dell'autenticazione Kerberos](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) e [Novità della protezione delle credenziali](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection).

## <a name="windows-server-2012r2"></a>Windows Server 2012 R2

Sistemi operativi supportati per i controller di dominio:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta di Windows Server 2012 R2

* Tutte le funzionalità disponibili per il livello di funzionalità della foresta di Windows Server 2012, senza alcuna funzionalità aggiuntiva.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio di Windows Server 2012 R2

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello di funzionalità del dominio di Windows Server 2012, più le funzionalità seguenti:
   * Protezioni sul lato controller di dominio per utenti protetti. Gli utenti protetti che eseguono l'autenticazione a un dominio di Windows Server 2012 R2 non possono più:
      * Usare l'autenticazione NTLM
      * Usare suite di crittografia DES o RC4 nella preautenticazione Kerberos
      * Usare la delega vincolata o non vincolata
      * Rinnovare i ticket utente (TGT) oltre la durata iniziale di 4 ore.
   * Authentication Policies
      * Nuovi criteri di Active Directory basati su foresta che possono essere applicati agli account dei domini di Windows Server 2012 R2 per controllare quali host possono accedere da un account e applicare le condizioni di controllo di accesso per l'autenticazione ai servizi in esecuzione come account.
   * Authentication Policy Silos
      * Nuovo oggetto di Active Directory basato su foresta, che può creare una relazione tra utente, servizio gestito e account computer da usare per classificare gli account per i criteri di autenticazione o per l'isolamento dell'autenticazione.

## <a name="windows-server-2012"></a>Windows Server 2012

Sistemi operativi supportati per i controller di dominio:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta di Windows Server 2012

* Tutte le funzionalità disponibili per il livello di funzionalità della foresta di Windows Server 2008 R2, senza alcuna funzionalità aggiuntiva.

### <a name="windows-server-2012-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio di Windows Server 2012

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello di funzionalità del dominio di Windows Server 2008 R2, più le funzionalità seguenti:
   * Il supporto KDC per attestazioni, autenticazione composta e Kerberos che blindano i criteri relativi ai modelli amministrativi KDC include due impostazioni (Fornisci sempre attestazioni e Errore per richieste di autenticazione non blindate) che richiedono un livello di funzionalità del dominio di Windows Server 2012. Per altre informazioni, vedi [Novità dell'autenticazione Kerberos](https://technet.microsoft.com/library/hh831747.aspx).

## <a name="windows-server-2008r2"></a>Windows Server 2008 R2

Sistemi operativi supportati per i controller di dominio:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta di Windows Server 2008 R2

* Tutte le funzionalità disponibili per il livello di funzionalità della foresta Windows Server 2003, più le funzionalità seguenti:
   * Cestino di Active Directory, che consente di ripristinare completamente gli oggetti eliminati mentre Servizi di dominio Active Directory è in esecuzione.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio di Windows Server 2008 R2

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello di funzionalità del dominio di Windows Server 2008, più le funzionalità seguenti:
   * Verifica del meccanismo di autenticazione, che consente di raccogliere informazioni sul tipo di metodo di accesso (smart card oppure nome utente/password) usato per l'autenticazione degli utenti di dominio nel token Kerberos di ogni utente. Quando questa funzionalità è abilitata in un ambiente di rete in cui è distribuita un'infrastruttura per la gestione delle identità federate, ad esempio Active Directory Federation Services (AD FS), le informazioni nel token possono essere estratte ogni volta che un utente prova ad accedere a qualsiasi applicazione in grado di riconoscere attestazioni sviluppata per determinare l'autorizzazione in base al metodo di accesso di un utente.
   * Gestione automatica dei nomi dell'entità servizio (SPN) per i servizi in esecuzione in uno specifico computer nel contesto di un account del servizio gestito quando viene modificato il nome o il nome host DNS dell'account computer. Per altre informazioni sugli account del servizio gestito, vedi [Guida dettagliata agli account del servizio](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Sistemi operativi supportati per i controller di dominio:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta di Windows Server 2008

* Tutte le funzionalità disponibili per il livello di funzionalità della foresta di Windows Server 2003, senza alcuna funzionalità aggiuntiva. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio di Windows Server 2008

* Tutte le funzionalità predefinite di Active Directory Domain Services, tutte le funzionalità del livello di funzionalità del dominio di Windows Server 2003, più le funzionalità seguenti:
  * Supporto della replica del file system distribuito (DFS) per il volume di sistema (SYSVOL) di Windows Server 2003
    * Il supporto della replica DFS fornisce una replica più affidabile e dettagliata del contenuto di SYSVOL.

      > [!NOTE]
      > A partire da Windows Server 2012 R2, il servizio Replica file (FRS) è deprecato. Un nuovo dominio creato in un controller di dominio che esegue almeno Windows Server 2012 R2 deve essere impostato sul livello di funzionalità del dominio di Windows Server 2008 o superiore.

  * Spazi dei nomi DFS basati su dominio in esecuzione in modalità Windows Server 2008, incluso il supporto per l'enumerazione basata sull'accesso e una maggiore scalabilità. Per gli spazi dei nomi basati su dominio in modalità Windows Server 2008 è necessario che anche la foresta usi il livello di funzionalità di Windows Server 2003. Per altre informazioni, vedi [Scegliere un tipo di spazio dei nomi](https://go.microsoft.com/fwlink/?LinkId=180400).
  * Supporto Advanced Encryption Standard (AES 128 e AES 256) per il protocollo Kerberos. Per poter emettere ticket TGT usando AES, il livello di funzionalità del dominio deve essere pari a Windows Server 2008 o versione successiva e la password del dominio deve essere modificata. 
    * Per altre informazioni, vedi [Miglioramenti di Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).

      > [!NOTE]
      >Gli errori di autenticazione possono verificarsi in un controller di dominio dopo l'aumento del livello di funzionalità del dominio a Windows Server 2008 o superiore se il controller di dominio ha già replicato la modifica DFL ma non ha ancora aggiornato la password di krbtgt. In questo caso, il riavvio del servizio KDC nel controller di dominio attiverà un aggiornamento in memoria della nuova password di krbtgt e risolverà gli errori di autenticazione correlati.

  * L'[ultimo accesso interattivo](https://go.microsoft.com/fwlink/?LinkId=180387) visualizza le informazioni seguenti:
     * Numero totale di tentativi di accesso non riusciti a una workstation Windows Vista o un server Windows Server 2008 appartenente a un dominio
     * Numero totale di tentativi di accesso non riusciti dopo un accesso riuscito a una workstation Windows Vista o un server Windows Server 2008
     * Data e ora dell'ultimo tentativo di accesso non riuscito a una workstation Windows Vista o un server Windows Server 2008
     * Data e ora dell'ultimo tentativo di accesso riuscito a una workstation Windows Vista o un server Windows Server 2008
  * Criteri granulari per le password, che ti consentono di specificare criteri di blocco per le password e gli account per utenti e gruppi di sicurezza globali in un dominio. Per altre informazioni, vedi [Guida dettagliata alla configurazione dei criteri granulari di blocco per le password e gli account](https://go.microsoft.com/fwlink/?LinkID=91477).
  * Desktop virtuali personali
     * Per usare la funzionalità aggiunta fornita dalla scheda Desktop virtuali personali della finestra di dialogo Proprietà account utente in Utenti e computer di Active Directory, devi estendere lo schema di Active Directory Domain Services per Windows Server 2008 R2 (versione oggetto schema = 47). Per altre informazioni, vedi [Guida dettagliata alla distribuzione di desktop personali virtuali tramite Connessione RemoteApp e desktop](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Sistemi operativi supportati per i controller di dominio:

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta di Windows Server 2003

* Tutte le funzionalità predefinite di Active Directory Domain Services, più le funzionalità seguenti:
   * Trust tra foreste
   * Ridenominazione dei domini
   * Replica valore collegato
      - La replica valore collegato ti consente di modificare l'appartenenza ai gruppi per archiviare e replicare i valori dei singoli membri anziché replicare l'intera appartenenza come singola unità. Usando una larghezza di banda di rete inferiore e un minor numero di cicli del processore durante la replica, l'archiviazione e la replica dei valori dei singoli membri ti impediscono di perdere aggiornamenti quando aggiungi o rimuovi più membri simultaneamente in controller di dominio diversi.
   * Possibilità di distribuire un controller di dominio di sola lettura
   * Miglioramento della scalabilità e degli algoritmi di Controllo di coerenza informazioni (KCC).
      - Il generatore della topologia tra siti (ISTG) usa algoritmi migliorati che si adattano per il supporto di foreste con un numero maggiore di siti rispetto a quanto è in grado di supportare Active Directory Domain Services al livello di funzionalità della foresta di Windows 2000. L'algoritmo di elezione migliorato è un meccanismo meno invasivo per scegliere il generatore ISTG al livello di funzionalità della foresta di Windows 2000.
   * Possibilità di creare istanze della classe ausiliaria dinamica denominata **dynamicObject** in una partizione di directory di dominio
   * Possibilità di convertire un'istanza dell'oggetto **inetOrgPerson** in un'istanza dell'oggetto **User** e di eseguire la conversione nella direzione opposta.
   * Possibilità di creare istanze di nuovi tipi di gruppo per supportare l'autorizzazione basata sui ruoli. 
      - Questi tipi sono denominati gruppi di base dell'applicazione e gruppi di query LDAP.
   * Disattivazione e ridefinizione degli attributi e delle classi nello schema. È possibile riusare gli attributi seguenti: ldapDisplayName, schemaIdGuid, OID e mapiID.
   * Spazi dei nomi DFS basati su dominio in esecuzione in modalità Windows Server 2008, incluso il supporto per l'enumerazione basata sull'accesso e una maggiore scalabilità. Per altre informazioni, vedi [Scegliere un tipo di spazio dei nomi](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio di Windows Server 2003

* Tutte le funzionalità predefinite di Active Directory Domain Services, tutte le funzionalità disponibili per il livello di funzionalità del dominio nativo di Windows 2000, più le funzionalità seguenti:
   * Strumento di gestione del dominio, Netdom. exe, che ti consente di rinominare i controller di dominio
   * Aggiornamenti dell'indicatore data e ora di accesso
      * L'attributo **lastLogonTimestamp** viene aggiornato con l'ora dell'ultimo accesso da parte dell'utente o del computer. Questo attributo viene replicato all'interno del dominio.
   * Possibilità di impostare l'attributo **userPassword** come password valida per gli oggetti **inetOrgPerson** e User
   * Possibilità di reindirizzare i contenitori Utenti e Computer
      * Per impostazione predefinita, per l'inserimento di account computer e account utente sono disponibili due contenitori conosciuti, ovvero cn=Computers,<domain root> e cn=Users,<domain root>. Questa funzionalità consente di definire un nuovo percorso conosciuto per questi account.
   * Possibilità per Gestione autorizzazioni di archiviare i propri criteri di autorizzazione in Active Directory Domain Services
   * Delega vincolata
      * La delega vincolata consente alle applicazioni di sfruttare la delega sicura delle credenziali utente tramite l'autenticazione basata su Kerberos.
      * Puoi limitare la delega solo a servizi di destinazione specifici.
   * Autenticazione selettiva
      * L'autenticazione selettiva ti consente di specificare gli utenti e i gruppi di una foresta trusted a cui è consentito eseguire l'autenticazione ai server di risorse di una foresta trusting.

## <a name="windows-2000"></a>Windows 2000

Sistemi operativi supportati per i controller di dominio:

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta nativa di Windows 2000

* Tutte le funzionalità predefinite di Active Directory Domain Services.

### <a name="windows-2000-native-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio nativo di Windows 2000

* Tutte le funzionalità predefinite di Active Directory Domain Services, più le funzionalità di directory seguenti:
   * Gruppi universali per gruppi di distribuzione e di sicurezza
   * Annidamento di gruppi
   * Conversione di gruppi, che consente la conversione tra gruppi di sicurezza e gruppi di distribuzione
   * Cronologia dell'ID di sicurezza (SID)

## <a name="next-steps"></a>Passaggi successivi

* [Aumentare il livello di funzionalità del dominio](https://technet.microsoft.com/library/cc753104.aspx)  
* [Aumentare il livello di funzionalità della foresta](https://technet.microsoft.com/library/cc730985.aspx)
