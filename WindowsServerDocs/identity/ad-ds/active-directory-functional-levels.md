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
ms.openlocfilehash: c1e2108084b03fabbf7c6a18c2ecbcaf3cbd1dd9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868261"
---
# <a name="forest-and-domain-functional-levels"></a>Livelli di funzionalità del dominio e della foresta

>Si applica a: Windows Server

I livelli di funzionalità determinano le funzionalità disponibili per il dominio o la foresta di Active Directory Domain Services (AD DS). Determinano inoltre quali sistemi operativi Windows Server è possibile eseguire sui controller di dominio nel dominio o nella foresta. Tuttavia, i livelli di funzionalità non influiscono sui sistemi operativi che è possibile eseguire sulle workstation e sui server membri aggiunti al dominio o alla foresta.

Quando si distribuisce servizi di dominio Active Directory, impostare i livelli di funzionalità del dominio e della foresta sul valore massimo che l'ambiente può supportare. In questo modo, è possibile utilizzare il maggior numero possibile di funzionalità di servizi di dominio Active Directory. Quando si distribuisce una nuova foresta, viene richiesto di impostare il livello di funzionalità della foresta e quindi di impostare il livello di funzionalità del dominio. È possibile impostare il livello di funzionalità del dominio su un valore maggiore del livello di funzionalità della foresta, ma non è possibile impostare il livello di funzionalità del dominio su un valore inferiore al livello di funzionalità della foresta.

Con la fine del ciclo di vita di Windows 2003, i controller di dominio Windows 2003 devono essere aggiornati a Windows Server 2008, 2008R2, 2012, 2012R2, 2016 o 2019. Di conseguenza, qualsiasi controller di dominio che esegue Windows Server 2003 devono essere rimossi dal dominio.

In Windows Server 2008 e più livelli funzionali di dominio, replica del file system distribuito (DFS, Distributed File Service) viene utilizzata per la replica di contenuto della cartella SYSVOL tra i controller di dominio. Se si crea un nuovo dominio a livello funzionale di dominio di Windows Server 2008 o versione successiva, replica DFS viene automaticamente utilizzata per replicare SYSVOL. Se è stato creato il dominio a un livello funzionale inferiore, è necessario eseguire la migrazione da utilizza FRS a replica DFS per SYSVOL. Per la procedura di migrazione, è possibile seguire il [procedure su TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) o è possibile fare riferimento il [semplificata di set di passaggi nel blog del Team di archiviazione File CAB](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

## <a name="windows-server-2019"></a>Windows Server 2019

In questa versione non sono stati aggiunti nuovi livelli di funzionalità della foresta o del dominio.

Il requisito minimo per aggiungere un controller di dominio Windows Server 2019 è un livello di funzionalità di Windows Server 2008. Il dominio deve anche usare DFS-R come motore per replicare SYSVOL.

## <a name="windows-server-2016"></a>Windows Server 2016

Sistema operativo del controller di dominio supportato:

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta di Windows Server 2016

* Sono disponibili tutte le funzionalità disponibili per il livello di funzionalità della foresta 2012R2 di Windows Server e le seguenti funzionalità:
   * [Gestione accessi con privilegi (PAM) con Microsoft Identity Manager (MIM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio di Windows Server 2016

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello di funzionalità del dominio Windows Server 2012R2, oltre alle funzionalità seguenti:
   * I controller di dominio possono supportare il Rolling automatico dei segreti NTLM e altri segreti basati su password in un account utente configurato per richiedere l'autenticazione PKI. Questa configurazione è nota anche come "smart card necessaria per l'accesso interattivo"
   * I controller di dominio possono supportare la rete NTLM quando un utente è limitato a dispositivi appartenenti a un dominio specifico.
   * I client Kerberos che eseguono l'autenticazione con l'estensione di aggiornamento PKInit otterranno il SID di identità della chiave pubblica aggiornata.

    Per ulteriori informazioni, vedere Novità dell' [autenticazione Kerberos](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) e novità [di protezione delle credenziali](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012 R2

Sistema operativo del controller di dominio supportato:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta 2012R2 di Windows Server

* Tutte le funzionalità disponibili nel livello di funzionalità della foresta di Windows Server 2012, ma senza funzionalità aggiuntive.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio di Windows Server 2012R2

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello di funzionalità del dominio Windows Server 2012, più le funzionalità seguenti:
   * Protezioni lato controller di dominio per utenti protetti. Gli utenti protetti che eseguono l'autenticazione a un dominio di Windows Server 2012 R2 non possono più:
      * Eseguire l'autenticazione con l'autenticazione NTLM
      * Usare i pacchetti di crittografia DES o RC4 nella preautenticazione Kerberos
      * Delega vincolata o vincolata
      * Rinnova i ticket utente (TGT) oltre la durata iniziale di 4 ore
   * Authentication Policies
      * Nuovi criteri di Active Directory basati sulle foreste che possono essere applicati agli account nei domini di Windows Server 2012 R2 per controllare quali host possono accedere da un account e applicare le condizioni di controllo di accesso per l'autenticazione ai servizi in esecuzione come account.
   * Authentication Policy Silos
      * Nuovo oggetto Active Directory basato su foresta, che può creare una relazione tra utente, servizio gestito e computer, account da usare per classificare gli account per i criteri di autenticazione o per l'isolamento dell'autenticazione.

## <a name="windows-server-2012"></a>Windows Server 2012

Sistema operativo del controller di dominio supportato:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta di Windows Server 2012

* Tutte le funzionalità disponibili nel livello di funzionalità della foresta di Windows Server 2008 R2, ma senza funzionalità aggiuntive.

### <a name="windows-server-2012-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio di Windows Server 2012

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello di funzionalità del dominio Windows Server 2008R2, oltre alle funzionalità seguenti:
   * Il criterio dei modelli amministrativi KDC Supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos ha due impostazioni (Fornisci sempre attestazioni e Rifiuta richieste di autenticazione non blindate) che richiedono il livello di funzionalità del dominio Windows Server 2012. Per ulteriori informazioni, vedere Novità [dell'autenticazione Kerberos](https://technet.microsoft.com/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

Sistema operativo del controller di dominio supportato:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta 2008R2 di Windows Server

* Tutte le funzionalità disponibili per il livello di funzionalità della foresta Windows Server 2003, più le funzionalità seguenti:
   * Cestino di Active Directory, che consente di ripristinare completamente gli oggetti eliminati mentre Servizi di dominio Active Directory è in esecuzione.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio di Windows Server 2008R2

* Tutte le funzionalità predefinite di Active Directory, tutte le funzionalità del livello di funzionalità del dominio Windows Server 2008, più le funzionalità seguenti:
   * Garanzia del meccanismo di autenticazione, che consente di inserire informazioni sul tipo di metodo di accesso (smart card o nome utente/password) usato per autenticare gli utenti del dominio all'interno del token Kerberos di ogni utente. Quando questa funzionalità è abilitata in un ambiente di rete in cui è stata distribuita un'infrastruttura di gestione delle identità federate, ad esempio Active Directory Federation Services (AD FS), le informazioni nel token possono essere estratte ogni volta che un utente tenta di accedere a qualsiasi applicazione in grado di riconoscere attestazioni sviluppata per determinare le autorizzazioni in base al metodo di accesso dell'utente.
   * Gestione automatica dei nomi SPN per i servizi in esecuzione in un particolare computer nel contesto di un account del servizio gestito quando viene modificato il nome o il nome host DNS dell'account del computer. Per ulteriori informazioni sugli account dei servizi gestiti, vedere la [Guida dettagliata agli account di servizio](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Sistema operativo del controller di dominio supportato:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta di Windows Server 2008

* Tutte le funzionalità disponibili nel livello di funzionalità della foresta di Windows Server 2003, ma non sono disponibili funzionalità aggiuntive. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio di Windows Server 2008

* Sono disponibili tutte le funzionalità predefinite di servizi di dominio Active Directory, tutte le funzionalità del livello di funzionalità del dominio Windows Server 2003 e le seguenti funzionalità:
  * Supporto della replica di file system distribuito (DFS) per il volume di sistema di Windows Server 2003 (SYSVOL)
    * Il supporto della replica DFS fornisce una replica più affidabile e dettagliata del contenuto SYSVOL.

      > [!NOTE]
      > A partire da Windows Server 2012 R2, il servizio Replica file (FRS) è deprecato. Un nuovo dominio creato in un controller di dominio che esegue almeno Windows Server 2012 R2 deve essere impostato sul livello di funzionalità del dominio di Windows Server 2008 o superiore.

  * Spazi dei nomi DFS basati su dominio in esecuzione in modalità Windows Server 2008, che include il supporto per l'enumerazione basata sull'accesso e una maggiore scalabilità. Gli spazi dei nomi basati su dominio in modalità Windows Server 2008 richiedono anche che la foresta usi il livello di funzionalità della foresta di Windows Server 2003. Per altre informazioni, vedere [scegliere un tipo di spazio dei nomi](https://go.microsoft.com/fwlink/?LinkId=180400).
  * Supporto di Advanced Encryption Standard (AES 128 e AES 256) per il protocollo Kerberos. Per poter emettere TGT usando AES, il livello di funzionalità del dominio deve essere Windows Server 2008 o versione successiva e la password del dominio deve essere modificata. 
    * Per ulteriori informazioni, vedere la pagina relativa ai [miglioramenti di Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).

      > [!NOTE]
      >Gli errori di autenticazione possono verificarsi in un controller di dominio dopo la generazione del livello di funzionalità del dominio a Windows Server 2008 o versione successiva se il controller di dominio ha già replicato la modifica di DFL ma non ha ancora aggiornato la password di krbtgt. In questo caso, il riavvio del servizio KDC sul controller di dominio attiverà un aggiornamento in memoria della nuova password KRBTGT e risolverà gli errori di autenticazione correlati.

  * [Ultimo accesso interattivo](https://go.microsoft.com/fwlink/?LinkId=180387) Vengono visualizzate le informazioni seguenti:
     * Numero totale di tentativi di accesso non riusciti in un server Windows Server 2008 o in una workstation Windows Vista aggiunti a un dominio
     * Il numero totale di tentativi di accesso non riusciti dopo un accesso riuscito a un server Windows Server 2008 o a una workstation Windows Vista
     * Ora dell'ultimo tentativo di accesso non riuscito a una workstation Windows Server 2008 o Windows Vista
     * Ora dell'ultimo tentativo di accesso riuscito a un server Windows Server 2008 o a una workstation Windows Vista
  * I criteri granulari per le password consentono di specificare i criteri per la password e il blocco degli account per gli utenti e i gruppi di sicurezza globali in un dominio. Per ulteriori informazioni, vedere [la guida dettagliata alla configurazione dei criteri specifici per le password e il blocco degli account](https://go.microsoft.com/fwlink/?LinkID=91477).
  * Desktop virtuali personali
     * Per utilizzare la funzionalità aggiunta fornita dalla scheda desktop virtuale personale della finestra di dialogo Proprietà account utente in Active Directory utenti e computer, è necessario estendere lo schema di servizi di dominio Active Directory per Windows Server 2008 R2 (versione oggetto schema = 47). Per ulteriori informazioni, vedere la pagina relativa alla [distribuzione di desktop virtuali personali tramite connessione RemoteApp e desktop Guida dettagliata](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Sistema operativo del controller di dominio supportato:

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta di Windows Server 2003

* Sono disponibili tutte le funzionalità predefinite di servizi di dominio Active Directory e le seguenti funzionalità:
   * Trust tra foreste
   * Ridenominazione dei domini
   * Replica di valori collegati
      - La replica di valori collegati consente di modificare l'appartenenza al gruppo per archiviare e replicare i valori per i singoli membri anziché replicare l'intera appartenenza come singola unità. L'archiviazione e la replica dei valori dei singoli membri utilizzano una minore larghezza di banda di rete e un minor numero di cicli del processore durante la replica e impedisce la perdita di aggiornamenti quando si aggiungono o rimuovono più membri simultaneamente in controller di dominio diversi.
   * Possibilità di distribuire un controller di dominio di sola lettura (RODC)
   * Miglioramento della scalabilità e degli algoritmi di controllo di coerenza informazioni (KCC)
      - Il generatore di topologia tra siti (ISTG) utilizza algoritmi migliorati che si ridimensionano per supportare le foreste con un numero maggiore di siti che servizi di dominio Active Directory può supportare al livello di funzionalità della foresta di Windows 2000. L'algoritmo di elezione ISTG migliorato è un meccanismo meno intrusivo per la scelta di ISTG al livello di funzionalità della foresta di Windows 2000.
   * Possibilità di creare istanze della classe ausiliaria dinamica denominata **DynamicObject** in una partizione di directory del dominio
   * Possibilità di convertire un'istanza dell'oggetto **InetOrgPerson** in un'istanza dell'oggetto **utente** e di completare la conversione nella direzione opposta
   * Possibilità di creare istanze di nuovi tipi di gruppo per supportare l'autorizzazione basata sui ruoli. 
      - Questi tipi sono denominati gruppi di base dell'applicazione e gruppi di query LDAP.
   * Disattivazione e ridefinizione degli attributi e delle classi nello schema. È possibile riusare gli attributi seguenti: ldapDisplayName, schemaIdGuid, OID e mapiID.
   * Spazi dei nomi DFS basati su dominio in esecuzione in modalità Windows Server 2008, che include il supporto per l'enumerazione basata sull'accesso e una maggiore scalabilità. Per altre informazioni, vedere [scegliere un tipo di spazio dei nomi](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio di Windows Server 2003

* Tutte le funzionalità predefinite di servizi di dominio Active Directory, tutte le funzionalità disponibili nel livello di funzionalità del dominio nativo di Windows 2000 e sono disponibili le seguenti funzionalità:
   * Lo strumento di gestione del dominio, Netdom. exe, che consente di rinominare i controller di dominio
   * Aggiornamenti dell'indicatore di data e ora di accesso
      * L'attributo **lastLogonTimestamp** viene aggiornato con l'ora dell'ultimo accesso da parte dell'utente o del computer. Questo attributo viene replicato all'interno del dominio.
   * Possibilità di impostare l'attributo **userPassword** come password valida per **InetOrgPerson** e gli oggetti utente
   * Possibilità di reindirizzare i contenitori di utenti e computer
      * Per impostazione predefinita, vengono forniti due contenitori noti per ospitare account computer e utente, ovvero CN = Computers<domain root> , e CN = Users,.<domain root> Questa funzionalità consente la definizione di un nuovo percorso noto per questi account.
   * Possibilità per gestione autorizzazioni di archiviare i propri criteri di autorizzazione in servizi di dominio Active Directory
   * Delega vincolata
      * La delega vincolata consente alle applicazioni di sfruttare la delega sicura delle credenziali utente per mezzo dell'autenticazione basata su Kerberos.
      * È possibile limitare la delega solo a servizi di destinazione specifici.
   * Autenticazione selettiva
      * L'autenticazione selettiva consente di specificare gli utenti e i gruppi di una foresta trusted a cui è consentito eseguire l'autenticazione nei server di risorse in una foresta trusting.

## <a name="windows-2000"></a>Windows 2000

Sistema operativo del controller di dominio supportato:

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Funzionalità del livello di funzionalità della foresta nativa di Windows 2000

* Sono disponibili tutte le funzionalità di servizi di dominio Active Directory predefinite.

### <a name="windows-2000-native-domain-functional-level-features"></a>Funzionalità del livello di funzionalità del dominio nativo di Windows 2000

* Sono disponibili tutte le funzionalità predefinite di servizi di dominio Active Directory e le seguenti funzionalità di directory, tra cui:
   * Gruppi universali per i gruppi di distribuzione e di sicurezza.
   * Annidamento di gruppi
   * Conversione di gruppi, che consente la conversione tra gruppi di sicurezza e di distribuzione
   * Cronologia dell'ID di sicurezza (SID)

## <a name="next-steps"></a>Passaggi successivi

* [Aumentare il livello di funzionalità del dominio](https://technet.microsoft.com/library/cc753104.aspx)  
* [Aumentare il livello di funzionalità della foresta](https://technet.microsoft.com/library/cc730985.aspx)
