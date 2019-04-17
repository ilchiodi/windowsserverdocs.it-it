---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: Monitoraggio dei segnali di compromissione di Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 096f2fa58b9aae53a06bf26c2107eb4cee3aa5d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitoraggio dei segnali di compromissione di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Legge numero cinque: Eternal sorveglianza è il prezzo di sicurezza.* - [10 Immutable Laws of Security Administration](https://technet.microsoft.com/library/cc722488.aspx)  
  
Un tinta unito registro eventi di sistema di monitoraggio è una parte fondamentale di qualsiasi infrastruttura di Active Directory protetta. Molti compromessi sulla protezione di computer potrebbe essere individuate in anticipo nell'evento se vittime attivati appropriato registro eventi di monitoraggio e avvisi. Report indipendenti tempo supportano questa conclusione. Ad esempio, il [2009 Verizon dati violazione Report](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) stati:  
  
"L'idraulici apparente di analisi di registro e il monitoraggio degli eventi continua a essere un po' di una macchina enigma. È l'opportunità per il rilevamento. ricercatori notato che 66% della vittima era sufficiente prova disponibile entro i log per individuare la violazione se fossero stati più accurati analisi di tali risorse."  
  
La mancanza di monitoraggio active i registri eventi rimane un punto debole coerenza nei piani di difesa sicurezza molte aziende. Il [report violazione dei dati Verizon 2012](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf) trovato che anche se 85% delle violazioni impiegato per essere notati settimane, 84% della vittima disponeva di prova della violazione nei relativi registri eventi.  
  
## <a name="windows-audit-policy"></a>Criteri di controllo di Windows  
Di seguito sono collegamenti al blog di supporto Microsoft ufficiale dell'organizzazione. Il contenuto di queste blog fornisce consigli, linee guida e consigli sulle impostazioni di controllo che consentono di migliorare la protezione dell'infrastruttura di Active Directory e sono una risorsa preziosa durante la progettazione di un criterio di controllo.  
  
-   [Controllo dell'accesso oggetto globale è magia](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -descrive un meccanismo di controllo denominato avanzata configurazione criteri di controllo che è stato aggiunto a Windows 7 e Windows Server 2008 R2 che consente di impostare i tipi di dati si desidera controllare facilmente e non afferra auditpol.exe e script.  
  
-   [Introduzione di controllo delle modifiche in Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -introduce il controllo le modifiche apportate in Windows Server 2008.  
  
-   [Controllo trucchi in Vista e 2008 interessante](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -spiega interessanti funzionalità di controllo di Windows Vista e Windows Server 2008 che può essere utilizzato per la risoluzione dei problemi o vedere ciò che accade nel proprio ambiente.  
  
-   [Una posizione centralizzata per il controllo in Windows Server 2008 e Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contiene un elenco di controllo delle funzionalità e le informazioni contenute in Windows Server 2008 e Windows Vista.  
  
I seguenti collegamenti forniscono informazioni sui miglioramenti apportati a Windows di controllo in Windows 8 e Windows Server 2012 e informazioni di dominio Active Directory controllo in Windows Server 2008.  
  
-   [Novità di controllo della sicurezza](https://technet.microsoft.com/library/hh849638.aspx) -offre una panoramica delle nuove funzionalità in Windows 8 e Windows Server 2012 di controllo della sicurezza.  
  
-   [Il controllo di Active Directory Step-by-Step Guida](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -descrive la nuova funzionalità di controllo servizi di dominio Active Directory (AD DS) in Windows Server 2008. Fornisce inoltre le procedure per implementare questa nuova funzionalità.  
  
### <a name="windows-audit-categories"></a>Categorie di controllo di Windows  
Prima di Windows Vista e Windows Server 2008, Windows erano disponibili solo nove categorie di criteri di controllo del registro eventi:  
  
-   Eventi accesso account  
  
-   Gestione degli account  
  
-   Accesso al servizio directory  
  
-   Eventi di accesso  
  
-   Accesso agli oggetti  
  
-   Modifica dei criteri  
  
-   Uso dei privilegi  
  
-   Analisi dei processi  
  
-   Eventi di sistema  
  
Queste categorie di controllo tradizionale nove costituiscono un criterio di controllo. Ogni categoria di criteri di controllo può essere abilitata per esito positivo, errore, o esito positivo e negativo eventi. Le descrizioni sono inclusi nella sezione successiva.  
  
#### <a name="audit-policy-category-descriptions"></a>Descrizioni delle categorie di criteri di controllo  
Le categorie di criteri di controllo Abilita i seguenti tipi di messaggio del registro eventi.  
  
##### <a name="audit-account-logon-events"></a>Controlla eventi accesso Account  
Report ogni istanza di un'entità di sicurezza (ad esempio, utente, computer o account del servizio) che è a o disconnessione da un computer in cui viene usato un altro computer per convalidare l'account. Eventi accesso account vengono generati quando un account dell'entità di sicurezza di dominio viene autenticato in un controller di dominio. Autenticazione di un utente locale in un computer locale genera un evento di accesso che viene registrato nel Registro di protezione locale. Non vengono registrati gli eventi di disconnessione alcun account.  
  
Questa categoria genera molto "rumore" perché Windows è infatti possibile account registrazione a e da computer locali e remoti durante il normale di business. Tuttavia, qualsiasi piano di sicurezza deve includere l'esito positivo e negativo di questa categoria di controllo.  
  
##### <a name="audit-account-management"></a>Controlla gestione degli Account  
Questa impostazione di controllo determina se tenere traccia di gestione di utenti e gruppi. Ad esempio, gli utenti e gruppi devono essere registrati quando un utente o account computer, un gruppo di sicurezza o un gruppo di distribuzione viene creato, modificato o eliminato; Quando un account utente o computer viene rinominato, disabilitato o abilitato. oppure, se viene modificata una password utente o computer. Può essere generato un evento per gli utenti o gruppi che vengono aggiunti o rimossi da altri gruppi.  
  
##### <a name="audit-directory-service-access"></a>Controlla accesso al servizio Directory  

Questa impostazione di criteri determina se per controllare l'accesso dell'entità di sicurezza a un oggetto di Active Directory che ha il proprio elenco di controllo di accesso di sistema specificato (SACL). In generale, questa categoria deve essere abilitata solo nei controller di dominio. Quando attivata, questa impostazione genera molto "rumore".  
  
##### <a name="audit-logon-events"></a>Controlla eventi di accesso  
Gli eventi di accesso vengono generati quando entità di sicurezza locale viene autenticata in un computer locale. Gli eventi di accesso registra gli accessi al dominio che si verificano nel computer locale. Non vengono generati eventi di disconnessione account. Quando abilitata, gli eventi di accesso genera molto "rumore", ma deve essere abilitate per impostazione predefinita in qualsiasi piano di controllo di sicurezza.  
  
##### <a name="audit-object-access"></a>Controlla accesso agli oggetti  
Accesso agli oggetti può generare eventi quando accede a oggetti definiti successivamente con il controllo è attivato (ad esempio, Opened, lettura, rinominato, eliminato o chiuso). Dopo aver abilitata la categoria di controllo principale, l'amministratore deve singolarmente definire gli oggetti che sarà di controllo abilitato. Molti oggetti di sistema di Windows includono il controllo è attivato, in modo che l'abilitazione di questa categoria inizieranno in genere di generare gli eventi prima che l'amministratore ha definito qualsiasi.  
  
Questa categoria è "disturbavano" e genera gli eventi di 5-10 per ogni accesso agli oggetti. Può essere difficile per gli amministratori di nuovi per il controllo degli oggetti ottenere informazioni utili. Deve essere abilitata solo quando necessario.  
  
##### <a name="auditing-policy-change"></a>Modifica dei criteri di controllo  
Questa impostazione di criteri consente di controllare tutti gli eventi di modifica di criteri di assegnazione diritti utente, criteri di Windows Firewall, criteri di attendibilità o modifiche al criterio di controllo. Questa categoria deve essere abilitata su tutti i computer. Genera rumore minimo.  
  
##### <a name="audit-privilege-use"></a>Controlla uso dei privilegi  

Esistono decine di diritti e autorizzazioni in Windows (ad esempio, accesso come processo Batch e agire come parte del sistema operativo). Questa impostazione di criteri consente di controllare ogni istanza di un'entità di sicurezza mettendo un privilegio o un diritto utente. Abilitazione di questa risultati categoria in un gran numero di "rumore", ma può essere utile per tenere traccia degli account dell'entità di sicurezza con privilegi elevati.  
  
##### <a name="audit-process-tracking"></a>Controlla traccia processo  
Questa impostazione di criteri consente di controllare il processo dettagliato informazioni per gli eventi, ad esempio l'attivazione di programma, uscita dal processo, la duplicazione di handle e accesso indiretto agli oggetti di monitoraggio. È utile per tenere traccia di utenti malintenzionati e i programmi che usano.  
  
Abilitare il controllo dei processi di controllo genera un numero elevato di eventi, in genere è impostato su **Nessun controllo**. Tuttavia, questa impostazione può offrire un grande vantaggio durante un intervento al log dettagliato dei processi di avvio e l'ora che sono stati avviati. Per i controller di dominio e altri server dell'infrastruttura singolo ruolo, questa categoria può essere attivata in modo sicuro in tutto il tempo. Server singolo ruolo non generano quantità processo di registrazione del traffico durante il normale delle proprie mansioni. Di conseguenza, può essere abilitate per acquisire eventi non autorizzati, se si verificano.  
  
##### <a name="system-events-audit"></a>Controlla eventi di sistema  

Eventi di sistema è quasi una categoria generica generica, registrazione degli eventi diversi che influiscono su computer, la protezione di sistema o il Registro di protezione. Include gli eventi per arresti e riavvii, interruzioni dell'alimentazione, modifiche in fase di sistema, inizializzazione pacchetto di autenticazione, clearings log di controllo, problemi di rappresentazione e un host di altri eventi generali. In generale, l'abilitazione di questa categoria di controllo genera molto "rumore", ma genera abbastanza eventi molto utili che è difficile mai consigliabile non consentendole.  
  
#### <a name="advanced-audit-policies"></a>Criteri di controllo avanzati  
A partire da Windows Vista e Windows Server 2008, Microsoft migliorato il modo registro eventi categoria selezioni multiple, creando sottocategorie in ogni categoria di controllo principale. Le sottocategorie consentono un controlli più granulare rispetto al in caso contrario usando le categorie principali. Usano le sottocategorie, è possibile abilitare solo parti di una categoria principale specifica e passare la generazione di eventi per cui non si utilizza. Ogni sottocategoria dei criteri di controllo può essere abilitata per esito positivo, errore, o esito positivo e negativo eventi.  
  
Per elencare tutte le sottocategorie di controllo disponibili, rivedere il contenitore Criteri di controllo avanzati in un oggetto Criteri di gruppo o digitare il comando seguente al prompt dei comandi in qualsiasi computer che esegue Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, Windows 8, Windows 7 o Windows Vista:  
  
**auditpol//list /subcategory: \ ***  
  
Per ottenere un elenco di sottocategorie di controllo attualmente configurate in un computer che esegue Windows Server 2012, Windows Server 2008 R2 o Windows 2008, digitare quanto segue:  
  
**auditpol//get /category: \ ***  
  
Nella schermata seguente mostra un esempio di presentazione di criteri di controllo correnti auditpol.exe.  
  
![Monitoraggio di Active Directory](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> Criteri di gruppo non restituire sempre accuratamente lo stato di tutti i criteri di controllo abilitati, differenza auditpol.exe. Vedere [ottenere i criteri di controllo effettivi in Windows 7 e 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) per altri dettagli.  
  
Ogni categoria principale ha più sottocategorie. Di seguito è riportato un elenco di categorie, le sottocategorie e una descrizione delle relative funzioni.  
  
### <a name="auditing-subcategories-descriptions"></a>Le descrizioni delle sottocategorie di controllo  
Le sottocategorie dei criteri di controllo Abilita i seguenti tipi di messaggio del registro eventi:  
  
#### <a name="account-logon"></a>Accesso account  
  
##### <a name="credential-validation"></a>Convalida delle credenziali  
Questa sottocategoria segnala i risultati dei test di convalida credenziali inviato per una richiesta di accesso di account utente. Questi eventi si verificano nel computer in cui è autorevole per le credenziali. Per gli account di dominio, il controller di dominio è autorevole, mentre per gli account locali, il computer locale è autorevole.  
  
Negli ambienti di dominio, la maggior parte degli eventi accesso account vengono registrata nel Registro di protezione dei controller di dominio che sono autorevoli per gli account di dominio. Tuttavia, questi eventi possono verificarsi in altri computer nell'organizzazione quando gli account locali vengono utilizzati per l'accesso.  
  
##### <a name="kerberos-service-ticket-operations"></a>Operazioni Ticket di servizio Kerberos  
Questa sottocategoria segnala gli eventi generati dai processi di richiesta di ticket di Kerberos nel controller di dominio che è autorevole per l'account di dominio.  
  
##### <a name="kerberos-authentication-service"></a>Servizio di autenticazione Kerberos  
Questa sottocategoria segnala gli eventi generati dal servizio di autenticazione Kerberos. Questi eventi si verificano nel computer in cui è autorevole per le credenziali.  
  
##### <a name="other-account-logon-events"></a>Altri eventi accesso Account  
Questa sottocategoria fornisce un report che si verificano in risposta alle credenziali inviato per una richiesta di accesso di account utente che non riguardano la convalida delle credenziali o i ticket Kerberos. Questi eventi si verificano nel computer in cui è autorevole per le credenziali. Per gli account di dominio, il controller di dominio è autorevole, mentre per gli account locali, il computer locale è autorevole.  
  
Negli ambienti di dominio, la maggior parte degli eventi di accesso account vengono registrati nel Registro di protezione dei controller di dominio che sono autorevoli per gli account di dominio. Tuttavia, questi eventi possono verificarsi in altri computer nell'organizzazione quando gli account locali vengono utilizzati per l'accesso. Gli esempi includono quanto segue:  
  
-   Disconnessioni di sessione di Servizi Desktop remoti  
  
-   Nuove sessioni di Servizi Desktop remoto  
  
-   Il blocco e sblocco di una workstation  
  
-   La chiamata di uno screen saver  
  
-   Chiudere uno screen saver  
  
-   Attacco, in cui viene ricevuta due volte una richiesta di Kerberos con informazioni identiche riprodurre di nuovo il rilevamento di Kerberos  
  
-   Accesso a una rete wireless concessa a un account utente o computer  
  
-   Accesso a una cablata 802.1 x concesse a un account utente o computer di rete  
  
#### <a name="account-management"></a>Gestione degli account  
  
##### <a name="user-account-management"></a>Gestione degli Account utente  
Questa sottocategoria restituisce ogni evento di gestione degli account utente, ad esempio quando un account utente viene creato, modificato o eliminato; un account utente viene rinominato, disabilitato o abilitato. o una password è impostata o modificata. Se questa impostazione di criteri di controllo è abilitata, gli amministratori possono tenere traccia eventi per rilevare dannosa accidentale e autorizzata creazione di account utente.  
  
##### <a name="computer-account-management"></a>Gestione degli Account computer  
Questa sottocategoria report ogni evento di gestione degli account computer, ad esempio quando un account computer creato, modificato, eliminato, rinominare, disabilitato o attiva.  
  
##### <a name="security-group-management"></a>Gestione dei gruppi di sicurezza  
Questa sottocategoria restituisce ogni evento di sicurezza di gestione di gruppo, ad esempio quando un gruppo di sicurezza viene creato, modificato o eliminato o quando un membro aggiunto o rimosso da un gruppo di sicurezza. Se questa impostazione di criteri di controllo è abilitata, gli amministratori possono tenere traccia eventi per rilevare dannosa accidentale e autorizzata creazione degli account di gruppo di sicurezza.  
  
##### <a name="distribution-group-management"></a>Gestione dei gruppi di distribuzione  
Questa sottocategoria restituisce ogni evento di distribuzione di gestione di gruppo, ad esempio quando un gruppo di distribuzione viene creato, modificato o eliminato o quando un membro aggiunto o rimosso da un gruppo di distribuzione. Se questa impostazione di criteri di controllo è abilitata, gli amministratori possono tenere traccia eventi per rilevare dannosa accidentale e autorizzata creazione degli account di gruppo.  
  
##### <a name="application-group-management"></a>Gestione di gruppo di applicazioni  
Questa sottocategoria restituisce ogni evento di applicazione di gestione di gruppo in un computer, ad esempio quando un gruppo di applicazioni viene creato, modificato o eliminato o quando un membro aggiunto o rimosso da un gruppo di applicazioni. Se questa impostazione di criteri di controllo è abilitata, gli amministratori possono tenere traccia eventi per rilevare dannosa accidentale e autorizzata creazione degli account di gruppo di applicazioni.  
  
##### <a name="other-account-management-events"></a>Altri eventi di gestione di Account  
Questa sottocategoria report altri eventi di gestione di account.  
  
#### <a name="detailed-process-tracking"></a>Monitoraggio processo dettagliato  
  
##### <a name="process-creation"></a>Creazione del processo  
Questa sottocategoria segnala la creazione di un processo e il nome dell'utente o del programma che l'ha creata.  
  
##### <a name="process-termination"></a>Chiusura del processo  
Questa sottocategoria segnala quando un processo viene terminato.  
  
##### <a name="dpapi-activity"></a>Attività DPAPI  
Questa sottocategoria segnala crittografare o decrittografare chiama in data protection application programming interface (DPAPI). DPAPI è utilizzato per proteggere le informazioni segrete, ad esempio password archiviate e la chiave.  
  
##### <a name="rpc-events"></a>Eventi RPC  
Questa procedura remota report di sottocategoria chiamare eventi di connessione (RPC).  
  
#### <a name="directory-service-access"></a>Accesso al servizio directory  
  
##### <a name="directory-service-access"></a>Accesso al servizio directory  
Questa sottocategoria segnala quando si accede a un oggetto di dominio Active Directory. Solo gli oggetti con configurati SACL genera eventi di controllo essere generati e solo quando è possibile accedervi in modo che le voci SACL corrispondente. Questi eventi sono simili agli eventi di accesso del servizio di directory nelle versioni precedenti di Windows Server. Questa sottocategoria si applica solo ai controller di dominio.  
  
##### <a name="directory-service-changes"></a>Modifiche servizio directory  
Questa sottocategoria segnala le modifiche apportate agli oggetti in Active Directory. I tipi di alcune delle modifiche riportate sono creare, modificare, spostare e annullare l'eliminazione delle operazioni eseguite su un oggetto. Directory service il controllo delle modifiche, se appropriato, indica gli obsoleti e nuovi valori delle proprietà modificate degli oggetti che sono state modificate. Solo gli oggetti con SACL genera eventi di controllo essere generati e solo quando è possibile accedervi in modo che corrisponda alle relative voci SACL. Alcuni oggetti e proprietà non causano deve essere generato a causa di impostazioni nella classe oggetto nello schema di eventi di controllo. Questa sottocategoria si applica solo ai controller di dominio.  
  
##### <a name="directory-service-replication"></a>Replica del servizio directory  
Questa sottocategoria segnala quando la replica tra due controller di dominio inizia e finisce.  
  
##### <a name="detailed-directory-service-replication"></a>Replica dettagliata servizio Directory  
Questa sottocategoria riporta informazioni dettagliate sulla replica tra controller di dominio. Questi eventi possono essere molto elevati volume.  
  
#### <a name="logonlogoff"></a>Accesso/fine sessione  
  
##### <a name="logon"></a>Accesso  
Questa sottocategoria segnala quando un utente tenta di accedere al sistema. Questi eventi si verificano nel computer a cui si accede. Per gli accessi interattivi, la generazione di questi eventi viene eseguita nel computer in cui è connesso. Se un accesso alla rete viene effettuato l'accesso alla condivisione, questi eventi generano nel computer che ospita la risorsa a cui si accede. Se questa impostazione è configurata per **Nessun controllo**, è difficile o Impossibile determinare l'utente che ha accesso o tentato di accedere ai computer dell'organizzazione.  
  
##### <a name="network-policy-server"></a>Server dei criteri di rete  
Questa sottocategoria segnala gli eventi generati dalle richieste di accesso utente RADIUS (IAS) e alla protezione di accesso di rete (NAP). Queste richieste possono essere **Grant**, **Nega**, **eliminare**, **quarantena**, **blocco**, e **sblocco**. Il controllo di questa impostazione determinerà un volume medio o alto dei record nel server dei criteri di rete e IAS.  
  
##### <a name="ipsec-main-mode"></a>Modalità principale IPsec  
Questa sottocategoria segnala i risultati del protocollo Internet Key Exchange (IKE) e Authenticated Internet Protocol (AuthIP) durante le negoziazioni in modalità principale.  
  
##### <a name="ipsec-extended-mode"></a>Modalità estesa IPsec  
Questa sottocategoria segnala i risultati di AuthIP durante le negoziazioni in modalità estesa.  
  
##### <a name="other-logonlogoff-events"></a>Altri eventi di accesso/fine sessione  
Questa sottocategoria segnala altri di accesso e gli eventi correlati alla disconnessione, ad esempio Servizi Desktop remoto sessione disconnessione e la riconnessione, mediante il comando RunAs per eseguire processi con un account diverso e il blocco e sblocco di una workstation.  
  
##### <a name="logoff"></a>Disconnessione  
Questa sottocategoria segnala quando un utente si disconnette il sistema. Questi eventi si verificano nel computer a cui si accede. Per gli accessi interattivi, la generazione di questi eventi viene eseguita nel computer in cui è connesso. Se un accesso alla rete viene effettuato l'accesso alla condivisione, questi eventi generano nel computer che ospita la risorsa a cui si accede. Se questa impostazione è configurata per **Nessun controllo**, è difficile o Impossibile determinare l'utente che ha accesso o tentato di accedere ai computer dell'organizzazione.  
  
##### <a name="account-lockout"></a>Blocco degli account  
Questa sottocategoria segnala quando un account utente è bloccato in seguito a un numero eccessivo di tentativi di accesso.  
  
##### <a name="ipsec-quick-mode"></a>Modalità rapida IPsec  
Questa sottocategoria segnala i risultati del protocollo IKE e AuthIP durante le negoziazioni in modalità rapida.  
  
##### <a name="special-logon"></a>Accesso speciale  
Questa sottocategoria segnala quando viene utilizzato un accesso speciale. Un accesso speciale è un accesso con privilegi di amministratore di un gruppo equivalente che può essere usato per elevare un processo a un livello superiore.  
  
#### <a name="policy-change"></a>Modifica dei criteri  
  
##### <a name="audit-policy-change"></a>Controlla Modifica criteri  
Questa sottocategoria segnala le modifiche in Criteri di controllo SACL modifiche incluse.  
  
##### <a name="authentication-policy-change"></a>Modifica criteri di autenticazione  
Questa sottocategoria segnala le modifiche in Criteri di autenticazione.  
  
##### <a name="authorization-policy-change"></a>Modifica criteri di autorizzazione  
Questa sottocategoria segnala le modifiche in Criteri di autorizzazione, tra cui le modifiche alle autorizzazioni (DACL).  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Modifica di criteri a livello di regola MPSSVC  
Questa sottocategoria segnala le modifiche apportate nelle regole di criteri utilizzate da (MPSSVC.exe) il servizio di protezione di Microsoft. Questo servizio viene utilizzato da Windows Firewall.  
  
##### <a name="filtering-platform-policy-change"></a>Modifica criteri piattaforma filtro  
Questa sottocategoria segnala l'aggiunta e rimozione di oggetti da Piattaforma filtro Windows, inclusi i filtri di avvio. Questi eventi possono essere molto elevati volume.  
  
##### <a name="other-policy-change-events"></a>Altri eventi di modifica dei criteri  
Questa sottocategoria report altri tipi di modifiche a criteri di sicurezza, ad esempio di configurazione del modulo TPM (Trusted Platform) o provider di crittografia.  
  
#### <a name="privilege-use"></a>Uso dei privilegi  
  
##### <a name="sensitive-privilege-use"></a>Utilizzo privilegi sensibili  
Questa sottocategoria segnala quando un account utente o il servizio utilizza un privilegi sensibili. Privilegio sensibile include i seguenti diritti utente: agire come parte del sistema operativo, eseguire il backup di file e directory, creare un oggetto token, eseguire il debug di programmi, abilitare l'account computer ed utente a tipo trusted per delega, generazione di controlli di sicurezza, rappresentare un client dopo l'autenticazione, caricare e scaricare driver di dispositivo, gestire il controllo e Registro di protezione, modificare i valori di ambiente firmware, sostituzione di token a livello di processo , ripristinare file e directory e assumere la proprietà del file o altri oggetti. Questa sottocategoria di controllo verrà creato un volume elevato di eventi.  
  
##### <a name="nonsensitive-privilege-use"></a>Utilizzo privilegi non riservati  
Questa sottocategoria segnala quando un account utente o il servizio utilizza un privilegio non riservato. Privilegio non riservato include i seguenti diritti utente: accedere a Gestione credenziali come chiamante trusted, accedere al computer dalla rete, aggiunta di workstation al dominio, regolazione limite risorse memoria per un processo, Consenti accesso locale, Consenti accesso tramite Servizi Desktop remoto, ignorare controllo incrociato, modificare l'ora di sistema, creare un file di paging, creazione oggetti globali, creare oggetti condivisi permanentemente, creare collegamenti simbolici , negare l'accesso al computer dalla rete, Nega accesso come processo batch, Nega accesso come servizio, Nega accesso locale, Nega accesso tramite Servizi Desktop remoto, da un sistema remoto arresto forzato, aumentare un working set del processo, aumentare la priorità di pianificazione, blocco di pagine in memoria, accedere come processo batch, accedere come un servizio, modificare etichette di oggetti , eseguire le attività di manutenzione volume, profilo del singolo processo, le prestazioni del sistema di profilo, rimuovere computer dall'alloggiamento di espansione, arrestare il sistema e sincronizzare i dati del servizio directory. Questa sottocategoria di controllo verrà creato un volume molto elevato di eventi.  
  
##### <a name="other-privilege-use-events"></a>Altri eventi di utilizzo dei privilegi  
Questa impostazione di criteri di sicurezza non è attualmente utilizzata.  
  
#### <a name="object-access"></a>Accesso agli oggetti  
  
##### <a name="file-system"></a>File System  
Questa sottocategoria segnala quando si accede a oggetti del file system. Solo oggetti del file system con SACL genera eventi di controllo essere generati e solo quando è possibile accedervi in modo che le voci SACL corrispondenti. Di per sé, questa impostazione non comporterà il controllo di eventi. Consente di controllare l'evento di un utente che accede a un oggetto di sistema di file che include un elenco di controllo di accesso di sistema specificato (SACL), in modo efficace abilitazione del controllo avvenire.  
  
Se l'impostazione Controlla accesso agli oggetti è configurato per **successo**, una voce di controllo viene generata ogni volta che un utente accede a un oggetto con un SACL specificato. Se questa impostazione è configurata per **errore**, una voce di controllo viene generata ogni volta che un utente ha esito negativo nel tentativo di accedere a un oggetto con un SACL specificato.  
  
##### <a name="registry"></a>Registro di sistema  
Questa sottocategoria segnala quando si accede agli oggetti del Registro di sistema. Solo gli oggetti del Registro di sistema con SACL genera eventi di controllo essere generati e solo quando è possibile accedervi in modo che le voci SACL corrispondenti. Di per sé, questa impostazione non comporterà il controllo di eventi.  
  
##### <a name="kernel-object"></a>Oggetto kernel  
Questa sottocategoria segnala quando si accede a oggetti del kernel, ad esempio i processi e mutex. Solo gli oggetti del kernel con SACL genera eventi di controllo essere generati e solo quando è possibile accedervi in modo che le voci SACL corrispondenti. In genere gli oggetti del kernel vengono forniti solo SACL se sono abilitate le opzioni di controllo AuditBaseObjects o AuditBaseDirectories.  
  
##### <a name="sam"></a>SAM  
Questa sottocategoria segnala quando si accede a oggetti di database di autenticazione degli account di protezione (SAM) locale.  
  
##### <a name="certification-services"></a>Servizi di certificazione  
Questa sottocategoria segnala quando vengono eseguite le operazioni di servizi di certificazione.  
  
##### <a name="application-generated"></a>Generato dall'applicazione  
Questa sottocategoria segnala quando applicazioni tentano di generare eventi di controllo utilizzando il controllo application programming interface (API).  
  
##### <a name="handle-manipulation"></a>Modifica handle  
Questa sottocategoria segnala quando un handle per un oggetto viene aperta o chiusa. Solo gli oggetti con SACL causano questi eventi essere generati e solo se l'operazione tentata handle corrisponde le voci del SACL. Gli eventi di manipolazione handle vengono generati solo per i tipi di oggetto in cui è abilitata la sottocategoria di accesso corrispondenti di oggetto (ad esempio, file system o del Registro di sistema).  
  
##### <a name="file-share"></a>Condivisione file  
Questa sottocategoria segnala quando si accede a una condivisione file. Di per sé, questa impostazione non comporterà il controllo di eventi. Consente di controllare l'evento di un utente che accede a un oggetto condivisione di file che include un elenco di controllo di accesso di sistema specificato (SACL), in modo efficace abilitazione del controllo avvenire.  
  
##### <a name="filtering-platform-packet-drop"></a>Pacchetti Piattaforma filtro  
Questa sottocategoria segnala quando vengono scartati da Windows Filtering Platform (WFP). Questi eventi possono essere molto elevati volume.  
  
##### <a name="filtering-platform-connection"></a>Connessione a piattaforma filtro  
Questa sottocategoria segnala quando le connessioni sono consentite o bloccate da Piattaforma filtro Windows. Questi eventi possono essere elevati volume.  
  
##### <a name="other-object-access-events"></a>Altri eventi di accesso di oggetti  
Questa sottocategoria report altri eventi relativi all'accesso oggetto, ad esempio i processi dell'utilità di pianificazione di attività e gli oggetti COM+.  
  
#### <a name="system"></a>Sistema  
  
##### <a name="security-state-change"></a>Modifica stato sicurezza  
Questa sottocategoria segnala le modifiche nello stato di protezione del sistema, ad esempio quando il sottosistema di sicurezza Avvia e arresta.  
  
##### <a name="security-system-extension"></a>Estensione sistema di sicurezza  
Questa sottocategoria segnala il caricamento del codice di estensione, ad esempio i pacchetti di autenticazione dal sottosistema di sicurezza.  
  
##### <a name="system-integrity"></a>Integrità del sistema  
Questa sottocategoria segnala violazioni di integrità del sottosistema di sicurezza.  
  
Driver IPsec  
  
Questa sottocategoria report sulle attività del driver Internet Protocol security (IPsec).  
  
##### <a name="other-system-events"></a>Altri eventi di sistema  
Questa sottocategoria report sugli altri eventi di sistema.  
  
Per ulteriori informazioni sulle descrizioni sottocategoria, consultare il [strumento Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  
  
Ogni organizzazione deve esaminare le precedenti coperte categorie e sottocategorie e abilitare le opzioni che meglio si adattano proprio ambiente. Le modifiche ai criteri di controllo devono sempre essere testate prima della distribuzione in un ambiente di produzione.  
  
### <a name="configuring-windows-audit-policy"></a>Configurazione dei criteri di controllo di Windows  
È possibile impostare criteri di controllo di Windows con le modifiche del Registro di sistema, auditpol.exe, API o criteri di gruppo. I metodi consigliati per la configurazione dei criteri di controllo per la maggior parte delle società sono i criteri di gruppo o auditpol.exe. L'impostazione di criteri di controllo del sistema richiede le autorizzazioni di un account amministratore o di autorizzazioni delegate appropriate.  
  
> [!NOTE]  
> Il **gestione file registro di controllo e protezione** devono disporre dei privilegi per l'entità di sicurezza (amministratori sono per impostazione predefinita) per consentire la modifica dell'accesso agli oggetti le opzioni delle singole risorse, ad esempio file, gli oggetti di Active Directory e chiavi del Registro di sistema di controllo.  
  
#### <a name="setting-windows-audit-policy-by-using-group-policy"></a>Impostazione dei criteri di controllo di Windows tramite criteri di gruppo  
Per impostare criteri di controllo mediante criteri di gruppo, configurare le categorie di controllo appropriati in **Configurazione computer\Impostazioni di Windows\Impostazioni protezione\Criteri Locali\criteri** (vedere la schermata seguente, ad esempio da locale Editor criteri di gruppo (gpedit.msc)). Ogni categoria di criteri di controllo può essere abilitata per **successo**, **errore**, o **successo** e gli eventi di errore.  
  
![Monitoraggio di Active Directory](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
Utilizzando Active Directory o criteri di gruppo locali, è possibile impostare criteri di controllo avanzati. Per impostare criteri di controllo avanzati, configurare le sottocategorie appropriate sotto **Configurazione computer\Impostazioni di Windows\Impostazioni protezione\configurazione avanzata controllo criteri** (vedere la schermata seguente, ad esempio da locale Editor criteri di gruppo (gpedit.msc)). Ogni sottocategoria dei criteri di controllo può essere abilitata per **successo**, **errore**, o **successo** e **errore** eventi.  
  
![Monitoraggio di Active Directory](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
#### <a name="setting-windows-audit-policy-using-auditpolexe"></a>Impostazione dei criteri di controllo di Windows mediante Auditpol.exe  
Auditpol.exe (per l'impostazione di criteri di controllo di Windows) è stato introdotto in Windows Server 2008 e Windows Vista. Inizialmente, potrebbero essere usati solo auditpol.exe per impostare criteri di controllo avanzati, ma è possibile utilizzare criteri di gruppo in Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, Windows 8 e Windows 7.  
  
Auditpol.exe è un'utilità della riga di comando. La sintassi è come segue:  
  
**auditpol /set / < categoria | Sottocategoria >:<audit category> / < successo | errore: > / < abilitare & disabilitare >**  
  
Esempi di sintassi Auditpol.exe:  
  
**auditpol /set /subcategory: "gestione degli account utente" /Success /failure:enable**  
  
**auditpol /set /subcategory: "accesso" /Success /failure:enable**  
  
**auditpol /set /subcategory: "IPSEC Main Mode" /failure:enable**  
  
> [!NOTE]  
> Auditpol.exe imposta criteri di controllo avanzati in locale. Se i criteri locali in conflitto con Active Directory o criteri di gruppo locali, le impostazioni di criteri di gruppo in genere la priorità sulle impostazioni auditpol.exe. Quando sono presenti più gruppo o i conflitti tra criteri locali, un solo criterio avranno la priorità sui (che è, sostituire). Non esegue l'unione dei criteri di controllo.  
  
#### <a name="scripting-auditpol"></a>Auditpol Scripting  
Microsoft fornisce un [script di esempio](https://support.microsoft.com/kb/921469) per gli amministratori che desiderano impostare criteri di controllo avanzati tramite uno script invece di digitare manualmente in ogni comando auditpol.exe.  
  
**Nota** criteri di gruppo non segnala sempre accuratamente lo stato di tutti i criteri di controllo abilitati, differenza auditpol.exe. Vedere [ottenere i criteri di controllo effettivi in Windows 7 e Windows 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) per altri dettagli.  
  
#### <a name="other-auditpol-commands"></a>Altri comandi Auditpol  
Per salvare e ripristinare un criterio di controllo locale e per visualizzare altri comandi correlati di controllo, è possibile utilizzare Auditpol.exe. Ecco l'altro **auditpol** comandi.  
  
**auditpol /clear** : utilizzato per cancellare e reimpostare i criteri di controllo locali  
  
**auditpol /backup /file:<filename> ** : utilizzato per eseguire il backup di un criterio di controllo locale corrente in un file binario  
  
**auditpol /restore /file:<filename> ** : utilizzato per importare un file di criteri di controllo salvato in precedenza in un criterio locale  
  
**auditpol / < get/set > /opzione:<CrashOnAuditFail> / < Abilita/Disabilita >** -se questa impostazione di criteri di controllo è abilitata, verrà interrotta immediatamente il sistema (con STOP: C0000244 messaggio {controllo fallito}) se non è possibile registrare un controllo di sicurezza per qualsiasi motivo. In genere, un evento ha esito negativo quando il Registro di controllo di sicurezza è pieno e il periodo di mantenimento dati specificato nel Registro di protezione è **non sovrascrivere eventi** o **sovrascrivere eventi ogni giorno**. In genere è abilitata solo per ambienti che richiedono maggiore garantisce che si accede al Registro di protezione. Se abilitata, gli amministratori devono strettamente guarda dimensione del Registro di protezione e ruotare i registri in base alle esigenze. Può inoltre essere impostato con criteri di gruppo modificando l'opzione di protezione **controllo: arresto del sistema immediato se non è possibile registrare i controlli di sicurezza** (impostazione predefinita = disattivata).  
  
**Auditpol / < get/set > /opzione:<AuditBaseObjects> / < Abilita/Disabilita >** -questa impostazione di criteri di controllo consente di controllare l'accesso a oggetti di sistema globale. Se questo criterio è abilitato, le dimensioni di oggetti di sistema, ad esempio mutex, eventi, semafori e periferiche DOS vengono creati con un elenco di controllo di accesso del sistema (SACL) predefinito. Prendere in considerazione la maggior parte degli amministratori il controllo degli oggetti di sistema globale per troppo "disturbata" e verrà abilitare solo se si sospetta violazioni dannoso. Solo gli oggetti denominati vengono assegnati un SACL. Se il controllo oggetto controllo criteri (o sottocategoria di controllo oggetto Kernel) anche è abilitata, viene controllato l'accesso a questi oggetti di sistema. Quando si configura questa impostazione di sicurezza, modifiche non verranno applicate solo dopo il riavvio di Windows. Questo criterio è anche possibile impostare con criteri di gruppo modificando l'opzione di protezione Controlla l'accesso a oggetti di sistema globale (impostazione predefinita = disattivata).  
  
**auditpol / < get/set > /opzione:<AuditBaseDirectories> / < Abilita/Disabilita >** -questa impostazione di criteri di controllo specifica che gli oggetti denominati del kernel (ad esempio, mutex e semafori) devono essere forniti SACL quando vengono creati. AuditBaseDirectories interessa gli oggetti contenitore mentre AuditBaseObjects riguarda gli oggetti che non possono contenere altri oggetti.  
  
**Auditpol / < get/set > /opzione:<FullPrivilegeAuditing> / < Abilita/Disabilita >** -  
  
Questa impostazione di criteri di controllo specifica se il client genera un evento quando per uno o più di questi privilegi vengono assegnati a un token di sicurezza dell'utente: AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege, TakeOwnershipPrivilege e TcbPrivilege. Se questa opzione non è abilitata (impostazione predefinita = disattivata), non vengono registrati i privilegi BackupPrivilege e RestorePrivilege. Per abilitare questa opzione è possibile rendere Registro di protezione estremamente disturbo (a volte centinaia di eventi una seconda) durante un'operazione di backup. Questo criterio è anche possibile impostare con criteri di gruppo modificando l'opzione di protezione **controllo: controlla l'uso dei privilegi di Backup e ripristino**.  
  
> [!NOTE]  
> È state scattate alcune informazioni qui fornite da Microsoft [tipo di opzione controllo](https://msdn.microsoft.com/library/dd973862(prot.20).aspx) e lo strumento Microsoft SCM.  
  
### <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>Applicare il controllo tradizionali o controllo avanzato  
In Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 e Windows Vista, gli amministratori possono scegliere di abilitare le categorie tradizionali nove o utilizzare le sottocategorie. È una scelta binaria che deve essere eseguita in ogni sistema Windows. Le categorie principali possono essere abilitate o non può essere sia il subcategoriesit.  
  
Per impedire il criterio di categoria tradizionali legacy di sovrascrivere le sottocategorie dei criteri di controllo, è necessario abilitare il **imposizione delle impostazioni sottocategoria di criteri di controllo (Windows Vista o versione successiva) per sostituire le impostazioni di categoria di controllo criteri** impostazione si trova in **Configurazione computer\Impostazioni di Windows\Impostazioni protezione\Criteri Locali\opzioni di protezione**.  
  
È consigliabile che le sottocategorie deve essere abilitate e configurate anziché le categorie principali nove. Ciò richiede che un'impostazione di criteri di gruppo sia attivato (per consentire le sottocategorie tra cui eseguire l'override di categorie di controllo) insieme a configurare le sottocategorie diverse che supportano i criteri di controllo.  
  
Le sottocategorie di controllo può essere configurato con diversi metodi, inclusi i criteri di gruppo e il programma da riga di comando, auditpol.exe.  
  
Per ulteriori informazioni sul controllo di Windows, vedere gli articoli seguenti:  
  
-   [Sicurezza avanzata di controllo in Windows 7 e Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
-   [Controllo e conformità in Windows Server 2008](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
-   [Come usare criteri di gruppo per configurare le impostazioni per i computer basati su Windows Server 2008 e Windows Vista in un dominio Windows Server 2008, in un dominio Windows Server 2003 o in un dominio Windows 2000 di controllo di sicurezza dettagliate](https://support.microsoft.com/kb/921469)  
  
-   [Guida dettagliata ai criteri di controllo della protezione avanzata](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
  


