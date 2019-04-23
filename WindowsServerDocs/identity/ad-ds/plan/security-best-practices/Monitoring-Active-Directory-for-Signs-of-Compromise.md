---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: Monitoraggio dei segnali di compromissione di Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 40d0d06f8d6d25c2c1dbf4662d3296a996d22055
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882942"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitoraggio dei segnali di compromissione di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Legge numero cinque: Vigilanza perenne è il prezzo di sicurezza.* - [10 leggi immutabili della protezione amministrazione](https://technet.microsoft.com/library/cc722488.aspx)  
  
Un registro eventi solido sistema di monitoraggio è una parte fondamentale di qualsiasi infrastruttura di Active Directory protetta. Molti compromette la sicurezza di computer è stato possibile individuare tempestivamente nell'evento se vittime relativo all'azione appropriato registro eventi di monitoraggio e avviso. Report indipendenti hanno tempo supportato questa conclusione. Ad esempio, il [Report sulla violazione dei dati da Verizon 2009](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) stati:  
  
"L'idraulici apparente di analisi di log e monitoraggio degli eventi continuano a essere in qualche modo di una macchina enigma. L'opportunità per il rilevamento è disponibile; addetti notato che 66% delle vittime sono evidenze sufficienti disponibili entro i log per scoprire la violazione se fossero stati più attente l'analisi di tali risorse."  
  
Questa mancanza di monitoraggio dei log eventi attivi rimane un punto debole di coerenza nei piani di difesa sicurezza molte società. Il [report sulla violazione dei dati da Verizon 2012](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf) trovato che anche se dall'85% delle violazioni della sicurezza ha richiesto diverse settimane per essere notati, l'84% della vittima è stato rilevato l'evidenza della violazione in relativi registri degli eventi.  
  
## <a name="windows-audit-policy"></a>Criteri di controllo di Windows

Di seguito vengono forniti collegamenti a blog del supporto aziendale ufficiale di Microsoft. Il contenuto di questi blog fornisce consigli, indicazioni e consigli sulle impostazioni di controllo che consentono di migliorare la sicurezza dell'infrastruttura di Active Directory e sono una risorsa preziosa quando si progetta un criterio di controllo.  
  
* [Controllo dell'accesso agli oggetti globale è Magic](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -viene descritto un meccanismo di controllo denominato controllo configurazione avanzata dei criteri che è stato aggiunto a Windows 7 e Windows Server 2008 R2 che consente di impostare i tipi di dati si vuole controllare con facilità e spostati non gli script e auditpol.exe.  
* [Introduzione a controllo delle modifiche in Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -introduce il controllo delle modifiche apportate in Windows Server 2008.  
* [Ad accesso sporadico suggerimenti per il controllo in Vista e 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -spiega interessante funzionalità di controllo di Windows Vista e Windows Server 2008 che può essere utilizzato per la risoluzione dei problemi o per visualizzare ciò che accade nell'ambiente in uso.  
* [Posizione centralizzata per il controllo in Windows Server 2008 e Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contiene una raccolta di controllo, le funzionalità e le informazioni contenute in Windows Server 2008 e Windows Vista.  
  
I collegamenti seguenti forniscono informazioni sui miglioramenti apportati a Windows il controllo in Windows 8 e Windows Server 2012 e informazioni su Active Directory Domain Services il controllo in Windows Server 2008.  
  
* [What ' s New in controllo della sicurezza](https://technet.microsoft.com/library/hh849638.aspx) -fornisce una panoramica delle nuove funzionalità in Windows 8 e Windows Server 2012 di controllo della sicurezza.  
* [Guida dettagliata il controllo di AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -descrive la nuova funzionalità di controllo Active Directory Domain Services (AD DS) in Windows Server 2008. Fornisce inoltre le procedure per implementare questa nuova funzionalità.  
  
### <a name="windows-audit-categories"></a>Categorie di controllo di Windows

Prima di Windows Vista e Windows Server 2008, Windows stati solo nove categorie di criteri di controllo del registro eventi:  
  
* Eventi accesso account  
* Gestione account  
* Accesso al servizio directory  
* Eventi di accesso  
* Accesso agli oggetti  
* Modifica dei criteri  
* Uso dei privilegi  
* Traccia del processo  
* Eventi di sistema  
  
Questi nove categorie di controllo tradizionale costituiscono un criterio di controllo. Ogni categoria di criteri di controllo può essere abilitata per l'esito positivo, errore, o esito positivo e negativo degli eventi. Nella sezione successiva sono incluse le relative descrizioni.  
  
#### <a name="audit-policy-category-descriptions"></a>Le descrizioni di categoria di criteri di controllo  
Le categorie di criteri di controllo Abilita i seguenti tipi di messaggio registro eventi.  
  
##### <a name="audit-account-logon-events"></a>Controlla eventi accesso Account  
Restituisce ogni istanza di un'entità di sicurezza (ad esempio, utente, computer o account del servizio) che è a o disconnessione da un computer in cui un altro computer viene usato per convalidare l'account. Eventi accesso account vengono generati quando un account dell'entità di sicurezza di dominio di autenticazione in un controller di dominio. L'autenticazione di un utente locale in un computer locale genera un evento di accesso che viene registrato nel Registro di sicurezza locale. Viene registrato alcun evento di disconnessione di account.  
  
Questa categoria genera molti dati di "rumore" poiché Windows si verificano continuamente gli account di accesso a e si disconnettono dal computer locale e remota durante il normale funzionamento di business. Tuttavia, un piano di protezione deve includere l'esito positivo e negativo di questa categoria di controllo.  
  
##### <a name="audit-account-management"></a>Controlla gestione degli Account  
Questa impostazione di controllo determina se tenere traccia di gestione di utenti e gruppi. Ad esempio, gli utenti e gruppi devono essere rilevati quando viene creato, modificato o eliminato, un utente o account computer, un gruppo di sicurezza o un gruppo di distribuzione Quando un account utente o computer viene rinominato, disabilitato o abilitato. o quando viene modificata una password utente o computer. Per gli utenti o gruppi che vengono aggiunti o rimossi da altri gruppi può essere generato un evento.  
  
##### <a name="audit-directory-service-access"></a>Controlla Accesso al servizio directory  

Questa impostazione criterio determina se per controllare l'accesso dell'entità di sicurezza a un oggetto di Active Directory che ha un proprio elenco di controllo di accesso specificato di sistema (SACL). In generale, questa categoria deve essere abilitata solo nei controller di dominio. Quando abilitata, questa impostazione genera molti dati di "rumore".  
  
##### <a name="audit-logon-events"></a>Controlla eventi di accesso  
Gli eventi di accesso vengono generati quando un'entità di sicurezza locale viene autenticata in un computer locale. Gli eventi di accesso registra gli accessi al dominio che si verificano nel computer locale. Non vengono generati gli eventi di disconnessione di account. Quando abilitata, gli eventi di accesso genera molti dati di "rumore", ma deve essere abilitate per impostazione predefinita in qualsiasi piano di controllo di sicurezza.  
  
##### <a name="audit-object-access"></a>Controlla accesso agli oggetti  
Accesso agli oggetti può generare eventi quando si accede a oggetti definiti successivamente con il controllo è attivato (ad esempio, Opened, lettura, ridenominazione, eliminato o chiuso). Dopo aver abilitata la categoria di controllo principale, l'amministratore deve definire individualmente gli oggetti che sarà il controllo abilitato. Molti oggetti di sistema di Windows forniti con il controllo è attivato, in modo che l'abilitazione di questa categoria inizierà in genere generare gli eventi prima che l'amministratore ha definito uno.  
  
Questa categoria è molto "disturbata" e verrà generati da cinque a dieci eventi per ogni accesso agli oggetti. Può essere difficile per gli amministratori di nuovi per il controllo degli oggetti ottenere informazioni utili. Deve essere abilitato solo quando necessario.  
  
##### <a name="auditing-policy-change"></a>Il controllo di modifica dei criteri  
Questa impostazione criterio consente di controllare tutti gli eventi di una modifica a criteri di assegnazione diritti utente, i criteri di Windows Firewall, criteri di attendibilità o modifiche ai criteri di controllo. Questa categoria deve essere abilitata in tutti i computer. Viene generato poco rumore.  
  
##### <a name="audit-privilege-use"></a>Controlla uso dei privilegi  

Sono disponibili decine di diritti e autorizzazioni in Windows (ad esempio, accesso come processo Batch e agire come parte del sistema operativo). Questa impostazione criterio consente di controllare ogni istanza di un'entità di sicurezza mettendo alla prova un privilegio o diritto utente. Offrendo risultati di questa categoria una quantità elevata di "rumore", ma può essere utile per rilevare gli account dell'entità di sicurezza con privilegi elevati.  
  
##### <a name="audit-process-tracking"></a>Controlla traccia processo  
Questa impostazione criterio consente di controllare il processo dettagliato informazioni di rilevamento per eventi, ad esempio activation programma, uscita dal processo, la duplicazione handle e accesso indiretto agli oggetti. È utile per tenere traccia di utenti malintenzionati e programmi che utilizzano.  
  
Abilitazione Controlla tracciato processo genera un numero elevato di eventi, in genere è impostato su **Nessun controllo**. Questa impostazione, tuttavia, può fornire un grande vantaggio durante un evento imprevisto dal log dettagliato dei processi di avvio e l'ora che sono stati avviati. Per i controller di dominio e altri server dell'infrastruttura solo ruolo, questa categoria può essere attivata in modo sicuro in tutto il tempo. Server singolo ruolo non generano processo molto traffico di rilevamento durante la normale esecuzione delle loro funzioni. Di conseguenza, può essere abilitati per acquisire gli eventi non autorizzati, se si verificano.  
  
##### <a name="system-events-audit"></a>Controllo degli eventi di sistema  

Gli eventi di sistema è quasi una categoria onnicomprensiva generica, la registrazione di diversi eventi che influiscono su computer, la sicurezza del sistema o il Registro di sicurezza. Include gli eventi per arresti e riavvii, interruzioni dell'alimentazione, modifiche in fase di sistema, inizializzazioni di pacchetto di autenticazione, clearings log di controllo, i problemi di rappresentazione e una serie di altri eventi generali. In generale, l'abilitazione di questa categoria audit genera molti dati di "rumore", ma genera eventi sufficienti molto utile che è difficile che mai consigliabile non abilitarlo.  
  
#### <a name="advanced-audit-policies"></a>Criteri di controllo avanzati

A partire da Windows Vista e Windows Server 2008, Microsoft ha migliorato il modo in cui le selezioni delle categorie di log eventi può essere effettuati creando sottocategorie in ogni categoria di controllo principale. Le sottocategorie consentono un controlli molto più granulari di avveniva in caso contrario, tramite le categorie principali. Usano le sottocategorie, è possibile abilitare solo parti di una categoria principale specifica e ignorare la generazione di eventi per cui non si dispone di alcuna utilità. Ogni sottocategoria dei criteri di controllo può essere abilitata per eventi di tipo Esito positivo, Esito negativo o Esito positivo e negativo.  
  
Per elencare tutte le sottocategorie di controllo disponibili, esaminare il contenitore Criteri di controllo avanzati in un oggetto Criteri di gruppo, oppure digitare il comando seguente al prompt dei comandi in qualsiasi computer che esegue Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, Windows 8, Windows 7 o Windows Vista:  
  
`auditpol /list /subcategory:\*`
  
Per ottenere un elenco di sottocategorie di controllo attualmente configurate su un computer che esegue Windows Server 2012, Windows Server 2008 R2 o Windows 2008, digitare quanto segue:  
  
`auditpol /get /category:\*`
  
Lo screenshot seguente mostra un esempio di auditpol.exe Elenca i criteri di controllo corrente.  
  
![monitoraggio di Active Directory](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> Criteri di gruppo non sempre segnalare con precisione lo stato di tutti i criteri di controllo abilitati, mentre auditpol.exe supporta. Visualizzare [ottenere i criteri di controllo valide in Windows 7 e 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) per altri dettagli.  
  
Ogni categoria principale include più sottocategorie. Di seguito è riportato un elenco di categorie, le sottocategorie e una descrizione delle relative funzioni.  
  
### <a name="auditing-subcategories-descriptions"></a>Descrizioni delle sottocategorie di controllo  
Le sottocategorie dei criteri di controllo Abilita i messaggi di log eventi seguenti:  
  
#### <a name="account-logon"></a>Accesso all'account  
  
##### <a name="credential-validation"></a>Convalida delle credenziali  
Questa sottocategoria segnala i risultati dei test di convalida credenziali inviate per una richiesta di accesso di account utente. Questi eventi si verificano nel computer autorevole per le credenziali. Per gli account di dominio, il controller di dominio è autorevole, mentre per gli account locali, il computer locale è autorevole.  
  
Negli ambienti di dominio, la maggior parte degli eventi accesso account vengono registrata nel Registro di sicurezza dei controller di dominio che sono rilevanti per gli account di dominio. Tuttavia, questi eventi possono verificarsi in altri computer nell'organizzazione quando gli account locali vengono utilizzati per eseguire l'accesso.  
  
##### <a name="kerberos-service-ticket-operations"></a>Operazioni di Ticket di servizio Kerberos  
Questa sottocategoria segnala gli eventi generati dai processi di richiesta ticket Kerberos nel controller di dominio autorevole per l'account di dominio.  
  
##### <a name="kerberos-authentication-service"></a>Servizio di autenticazione Kerberos  
Questa sottocategoria segnala gli eventi generati dal servizio di autenticazione Kerberos. Questi eventi si verificano nel computer autorevole per le credenziali.  
  
##### <a name="other-account-logon-events"></a>Altri eventi accesso Account  
Questa sottocategoria segnala gli eventi che si verificano in risposta alle credenziali inviate per una richiesta di accesso di account utente che non riguardano la convalida delle credenziali o i ticket Kerberos. Questi eventi si verificano nel computer autorevole per le credenziali. Per gli account di dominio, il controller di dominio è autorevole, mentre per gli account locali, il computer locale è autorevole.  
  
Negli ambienti di dominio, la maggior parte degli eventi di accesso di account vengono registrati nel Registro di sicurezza dei controller di dominio che sono rilevanti per gli account di dominio. Tuttavia, questi eventi possono verificarsi in altri computer nell'organizzazione quando gli account locali vengono utilizzati per eseguire l'accesso. Gli esempi possono includere quanto segue:  
  
* Disconnessioni di sessione Servizi Desktop remoti  
* Nuove sessioni di Servizi Desktop remoto  
* Il blocco e sblocco di una workstation  
* Richiamare uno screen saver  
* Eliminazione di uno screen saver  
* Attacco di tipo, in cui viene ricevuta due volte una richiesta Kerberos con informazioni identiche replay di rilevamento di Kerberos  
* Accesso a una rete senza fili concessa a un account utente o computer  
* Accesso a un cablate 802.1 x concesso a un account utente o computer di rete  
  
#### <a name="account-management"></a>Gestione account  
  
##### <a name="user-account-management"></a>Gestione account utente  
Questa sottocategoria segnala ogni evento di gestione degli account utente, ad esempio quando un account utente viene creato, modificato o eliminato; un account utente viene rinominato, disabilitato o abilitato. o una password viene impostata o cambiata. Se questa impostazione dei criteri di controllo è abilitata, gli amministratori possono tenere traccia eventi per rilevare la creazione di dannosa, autorizzata e accidentale degli account utente.  
  
##### <a name="computer-account-management"></a>Gestione degli Account computer  
Questa sottocategoria segnala ogni evento di gestione degli account computer, ad esempio quando un account del computer viene creato, modificato, eliminato, rinominato, disabilitato o abilitato.  
  
##### <a name="security-group-management"></a>Gestione dei gruppi di sicurezza  
Questa sottocategoria segnala ogni evento di gestione di gruppo di sicurezza, ad esempio quando un gruppo di sicurezza viene creato, modificato o eliminato oppure quando un membro viene aggiunto o rimosso da un gruppo di sicurezza. Se questa impostazione dei criteri di controllo è abilitata, gli amministratori possono tenere traccia eventi per rilevare la creazione di dannosa, autorizzata e accidentale degli account di gruppo di sicurezza.  
  
##### <a name="distribution-group-management"></a>Gestione dei gruppi di distribuzione  
Questa sottocategoria segnala ogni evento di distribuzione di gestione di gruppo, ad esempio quando un gruppo di distribuzione viene creato, modificato o eliminato oppure quando un membro viene aggiunto o rimosso da un gruppo di distribuzione. Se questa impostazione dei criteri di controllo è abilitata, gli amministratori possono tenere traccia eventi per rilevare dannosa, autorizzata e accidentale la creazione di account di gruppo.  
  
##### <a name="application-group-management"></a>Gestione dei gruppi di applicazioni  
Questa sottocategoria segnala ogni evento di applicazione di gestione di gruppo in un computer, ad esempio quando un gruppo di applicazioni viene creato, modificato o eliminato oppure quando un membro viene aggiunto o rimosso da un gruppo di applicazioni. Se questa impostazione dei criteri di controllo è abilitata, gli amministratori possono tenere traccia eventi per rilevare dannosa, autorizzata e accidentale la creazione di account di gruppo dell'applicazione.  
  
##### <a name="other-account-management-events"></a>Altri eventi di gestione di Account  
Questa sottocategoria segnala altri eventi di gestione di account.  
  
#### <a name="detailed-process-tracking"></a>Traccia del processo dettagliato  
  
##### <a name="process-creation"></a>Creazione del processo  
Questa sottocategoria segnala la creazione di un processo e il nome dell'utente o del programma di cui è stato creato.  
  
##### <a name="process-termination"></a>Terminazione del processo  
Questa sottocategoria segnala quando un processo viene terminato.  
  
##### <a name="dpapi-activity"></a>Attività DPAPI  
Questa sottocategoria segnala crittografare o decrittografare chiama il data protection application programming interface (DPAPI). Consente di proteggere le informazioni segrete, ad esempio la password archiviata e informazioni sulle chiavi DPAPI.  
  
##### <a name="rpc-events"></a>Eventi RPC  
Questa procedura remota report subcategory chiamare gli eventi di connessione (RPC).  
  
#### <a name="directory-service-access"></a>Accesso al servizio directory  
  
##### <a name="directory-service-access"></a>Accesso al servizio directory  
Questa sottocategoria segnala quando si accede a un oggetto di Active Directory Domain Services. Solo gli oggetti con configurati SACL causano eventi di controllo di essere generato e solo quando vi si accede in modo che corrisponda alle voci di elenco SACL. Questi eventi sono simili agli eventi di accesso del servizio di directory nelle versioni precedenti di Windows Server. Questa sottocategoria si applica solo ai controller di dominio.  
  
##### <a name="directory-service-changes"></a>Modifiche servizio directory  
Questa sottocategoria segnala le modifiche apportate agli oggetti in Active Directory Domain Services. I tipi di modifiche che vengono segnalati sono creare, modificare, spostare e annullare l'eliminazione di operazioni eseguite su un oggetto. Directory del servizio controllo della modifica, dove appropriato, indica i vecchi e nuovi valori delle proprietà modificate degli oggetti che sono state modificate. Solo gli oggetti con i SACL fanno gli eventi di controllo essere generato e solo quando vi si accede in modo che corrisponda alle relative voci di elenco SACL. Alcune proprietà e gli oggetti non provocano eventi di controllo deve essere generato a causa delle impostazioni sulla classe oggetto nello schema. Questa sottocategoria si applica solo ai controller di dominio.  
  
##### <a name="directory-service-replication"></a>Replica servizio directory  
Questa sottocategoria segnala quando la replica tra i due controller di dominio inizia e finisce.  
  
##### <a name="detailed-directory-service-replication"></a>Replica dettagliata servizio Directory  
Questa sottocategoria vengono fornite informazioni dettagliate sui dati replicati nei controller di dominio. Questi eventi possono essere elevati nel volume.  
  
#### <a name="logonlogoff"></a>Accesso/fine sessione  
  
##### <a name="logon"></a>Accesso  
Questa sottocategoria segnala quando un utente tenta di accedere al sistema. Questi eventi si verificano nel computer a cui si accede. Per gli accessi interattivi, la generazione di questi eventi si verifica nel computer in cui è connesso. Se un account di accesso di rete viene effettuato l'accesso alla condivisione, generano questi eventi nel computer che ospita la risorsa a cui si accede. Se questa impostazione è configurata per **alcun controllo**, è difficile o impossibile da determinare quale utente ha accesso o ha tentato di accedere ai computer dell'organizzazione.  
  
##### <a name="network-policy-server"></a>Server dei criteri di rete  
Questa sottocategoria segnala gli eventi generati dal raggio (IAS) e protezione accesso rete (NAP) le richieste di accesso utente. Queste richieste possono essere **concessione**, **Deny**, **ignora**, **quarantena**, **blocco**e **Sbloccare**. Il controllo di questa impostazione genererà un volume medio ed elevato di record nei server dei criteri di rete e IAS.  
  
##### <a name="ipsec-main-mode"></a>Modalità principale IPsec  
Questa sottocategoria segnala i risultati del protocollo Internet Key Exchange (IKE) e Authenticated Internet Protocol (AuthIP) durante la negoziazione della modalità principale.  
  
##### <a name="ipsec-extended-mode"></a>Modalità estesa IPsec  
Questa sottocategoria segnala i risultati di AuthIP durante le negoziazioni in modalità estesa.  
  
##### <a name="other-logonlogoff-events"></a>Altri eventi di accesso/fine sessione  
Questa sottocategoria segnala altro accesso e gli eventi correlati alla disconnessione, ad esempio Servizi Desktop remoto della sessione si disconnette e la riconnessione, mediante il comando RunAs per eseguire processi con un account diverso e il blocco e sblocco di una workstation.  
  
##### <a name="logoff"></a>Disconnessione  
Questa sottocategoria segnala quando un utente si disconnette il sistema. Questi eventi si verificano nel computer a cui si accede. Per gli accessi interattivi, la generazione di questi eventi si verifica nel computer in cui è connesso. Se un account di accesso di rete viene effettuato l'accesso alla condivisione, generano questi eventi nel computer che ospita la risorsa a cui si accede. Se questa impostazione è configurata per **alcun controllo**, è difficile o impossibile da determinare quale utente ha accesso o ha tentato di accedere ai computer dell'organizzazione.  
  
##### <a name="account-lockout"></a>Blocco degli account  
Questa sottocategoria segnala quando un account utente è bloccato in seguito a un numero eccessivo di tentativi di accesso.  
  
##### <a name="ipsec-quick-mode"></a>Modalità rapida IPsec  
Questa sottocategoria segnala i risultati del protocollo IKE e AuthIP durante la negoziazione della modalità rapida.  
  
##### <a name="special-logon"></a>Accesso speciale  
Questa sottocategoria segnala quando viene usato un account di accesso speciali. Un account di accesso speciali è un account di accesso con privilegi di amministratore equivalente che può essere utilizzato per elevare un processo a un livello superiore.  
  
#### <a name="policy-change"></a>Modifica dei criteri  
  
##### <a name="audit-policy-change"></a>Controlla Modifica criteri  
Questa sottocategoria segnala le modifiche ai criteri di controllo, incluse le modifiche SACL.  
  
##### <a name="authentication-policy-change"></a>Modifica criteri di autenticazione  
Questa sottocategoria segnala le modifiche nei criteri di autenticazione.  
  
##### <a name="authorization-policy-change"></a>Modifica criteri di autorizzazione  
Questa sottocategoria segnala le modifiche ai criteri di autorizzazione, incluse le modifiche delle autorizzazioni (DACL).  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Modifica dei criteri a livello di regola MPSSVC  
Questa sottocategoria segnala le modifiche apportate nelle regole di criteri utilizzate dal servizio di protezione di Microsoft (MPSSVC.exe). Questo servizio viene utilizzato da Windows Firewall.  
  
##### <a name="filtering-platform-policy-change"></a>Modifica criteri piattaforma filtro  
Questa sottocategoria segnala l'aggiunta e rimozione di oggetti dalla piattaforma filtro Windows, inclusi i filtri di avvio. Questi eventi possono essere elevati nel volume.  
  
##### <a name="other-policy-change-events"></a>Altri eventi di modifica dei criteri  
Questa sottocategoria segnala altri tipi di modifiche ai criteri di sicurezza, ad esempio configurazione del modulo TPM (Trusted Platform) o provider di crittografia.  
  
#### <a name="privilege-use"></a>Uso dei privilegi  
  
##### <a name="sensitive-privilege-use"></a>Utilizzo privilegi sensibili  
Questa sottocategoria segnala quando un account utente o il servizio Usa un privilegio sensibile. Un privilegio sensibile include i diritti utente seguenti: Agisci come parte del sistema operativo, eseguire il backup di file e directory, creare un oggetto del token, il debug dei programmi, Abilita account computer e utente per essere considerato attendibile per la delega, generazione controlli di sicurezza, rappresenta un client dopo l'autenticazione, caricare e scaricare driver di dispositivo, gestire i log di controllo e protezione, modificare i valori di ambiente firmware, sostituzione di token a livello di processo, ripristinare file e directory e diventare proprietari di file o altri oggetti. Il controllo di questa sottocategoria creerà un volume elevato di eventi.  
  
##### <a name="nonsensitive-privilege-use"></a>Uso dei privilegi non riservati  
Questa sottocategoria segnala quando un account utente o il servizio Usa un privilegio non riservato. Privilegio non riservato include i diritti utente seguenti: accedere a Gestione credenziali come chiamante trusted, accedi al computer dalla rete, aggiunta di workstation al dominio, regolazione limite risorse memoria per un processo, Consenti accesso locale, Consenti accesso tramite Remote Desktop Services, ignorare il controllo, incrociato modificare l'ora di sistema, creare un file di paging, creazione oggetti globali, creare oggetti condivisi permanentemente, creare collegamenti simbolici, negare l'accesso al computer dalla rete, Nega accesso come processo batch, Nega accesso come servizio, Nega accesso locale, Nega accesso tramite Servizi Desktop remoto, di arresto forzato da un sistema remoto, di aumentare un working set del processo, aumentare la priorità di pianificazione, di blocco di pagine in memoria, di eseguire l'accesso come processo batch, Accedi come servizio, di modifica delle etichette di oggetti, eseguire volume attività di manutenzione, profilo del singolo processo, profilare le prestazioni del sistema, rimuovere computer dall'alloggiamento di espansione, arrestare il sistema e sincronizzare i dati del servizio directory. Il controllo di questa sottocategoria creerà un volume molto elevato di eventi.  
  
##### <a name="other-privilege-use-events"></a>Altri eventi di uso dei privilegi  
Questa impostazione dei criteri di sicurezza non è attualmente utilizzata.  
  
#### <a name="object-access"></a>Accesso agli oggetti  
  
##### <a name="file-system"></a>File system  
Questa sottocategoria segnala quando si accede a oggetti del file system. Solo oggetti del file system con i SACL causano eventi di controllo di essere generato e solo quando vi si accede in modo corrispondente alle relative voci di elenco SACL. Autonomamente, questa impostazione non comporterà il controllo degli eventi. Determina se controllare l'evento di un utente che accede a un oggetto file system che dispone di un elenco di controllo di accesso di sistema specificato (SACL), in modo efficace l'abilitazione del controllo da eseguire.  
  
Se il controllo dell'accesso agli oggetti impostazione è configurata per **Success**, una voce di controllo viene generata ogni volta che un utente accede a un oggetto con un SACL specificato. Se questa impostazione è configurata per **errore**, ogni volta che un utente non riesce nel tentativo di accedere a un oggetto con un SACL specificato viene generata una voce di controllo.  
  
##### <a name="registry"></a>Registro di sistema  
Questa sottocategoria segnala quando si accede a oggetti del Registro di sistema. Solo gli oggetti del Registro di sistema con i SACL fanno gli eventi di controllo essere generato e solo quando vi si accede in modo corrispondente alle relative voci di elenco SACL. Autonomamente, questa impostazione non comporterà il controllo degli eventi.  
  
##### <a name="kernel-object"></a>Oggetto del kernel  
Questa sottocategoria segnala quando si accede agli oggetti kernel, ad esempio i mutex e processi. Solo gli oggetti del kernel con i SACL causano eventi di controllo di essere generato e solo quando vi si accede in modo corrispondente alle relative voci di elenco SACL. In genere gli oggetti del kernel sono forniti solo SACL se sono abilitate le opzioni di controllo AuditBaseObjects o AuditBaseDirectories.  
  
##### <a name="sam"></a>SAM  
Questa sottocategoria segnala quando si accede a oggetti di database di autenticazione degli account di protezione (SAM) locale.  
  
##### <a name="certification-services"></a>Servizi di certificazione  
Questa sottocategoria segnala quando vengono eseguite operazioni di servizi di certificazione.  
  
##### <a name="application-generated"></a>Applicazione generata  
Questa sottocategoria segnala quando le applicazioni tentano di generare eventi di controllo utilizzando il Windows il controllo application programming interface (API).  
  
##### <a name="handle-manipulation"></a>Manipolazione handle  
Questa sottocategoria segnala quando un handle a un oggetto è aperto o chiuso. Solo gli oggetti con i SACL fanno questi eventi essere generato e solo se l'operazione tentata handle corrisponde le voci di elenco SACL. Gli eventi di manipolazione handle vengono generati solo per i tipi di oggetto in cui è abilitata la sottocategoria di accesso corrispondente oggetto (ad esempio, file system o Registro di sistema).  
  
##### <a name="file-share"></a>Condivisione file  
Questa sottocategoria segnala quando si accede a una condivisione file. Autonomamente, questa impostazione non comporterà il controllo degli eventi. Determina se controllare l'evento di un utente che accede a un oggetto condivisione di file che include un elenco di controllo di accesso di sistema specificato (SACL), in modo efficace l'abilitazione del controllo da eseguire.  
  
##### <a name="filtering-platform-packet-drop"></a>Pacchetti Piattaforma filtro  
Questa sottocategoria segnala quando i pacchetti vengono rimossi dal Windows Filtering Platform (WFP). Questi eventi possono essere elevati nel volume.  
  
##### <a name="filtering-platform-connection"></a>Connessione a piattaforma filtro  
Questa sottocategoria segnala quando le connessioni consentite o bloccate dalla piattaforma filtro Windows. Questi eventi possono essere elevati volume Purchase Program.  
  
##### <a name="other-object-access-events"></a>Altri eventi di accesso di oggetto  
Questa sottocategoria segnala altri eventi relativi all'accesso dell'oggetto, ad esempio i processi dell'utilità di pianificazione di attività e oggetti COM+.  
  
#### <a name="system"></a>Sistema  
  
##### <a name="security-state-change"></a>Modifica dello stato di sicurezza  
Questa sottocategoria segnala le modifiche in stato di sicurezza del sistema, ad esempio quando il sottosistema di sicurezza Avvia e arresta.  
  
##### <a name="security-system-extension"></a>Estensione del sistema di sicurezza  
Questa sottocategoria segnala il caricamento di codice dell'estensione, ad esempio pacchetti di autenticazione dal sottosistema di sicurezza.  
  
##### <a name="system-integrity"></a>Integrità di sistema  
Questa sottocategoria segnala le violazioni di integrità del sottosistema di sicurezza.  
  
Driver IPsec  
  
Questa sottocategoria report sull'attività del driver Internet Protocol security (IPsec).  
  
##### <a name="other-system-events"></a>Altri eventi di sistema  
Questa sottocategoria segnala di altri eventi di sistema.  
  
Per altre informazioni sulle descrizioni sottocategoria, vedere la [dello strumento Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  
  
Ogni organizzazione deve esaminare le categorie precedenti interessate e le sottocategorie e abilitare quelle che meglio si adattano il proprio ambiente. Modifiche ai criteri di controllo è necessario verificare sempre prima della distribuzione in un ambiente di produzione.  
  
## <a name="configuring-windows-audit-policy"></a>Configurazione dei criteri di controllo di Windows

Criteri di controllo di Windows possono essere impostato tramite modifiche del Registro di sistema, auditpol.exe, le API o i criteri di gruppo. I metodi consigliati per la configurazione dei criteri di controllo per la maggior parte delle aziende sono criteri di gruppo o auditpol.exe. L'impostazione di criteri di controllo di un sistema richiede le autorizzazioni a livello di amministratore account o le appropriate autorizzazioni delegate.  
  
> [!NOTE]  
> Il **gestione file registro di controllo e protezione** deve disporre del privilegio alle entità di sicurezza (gli amministratori dispongono, per impostazione predefinita) per consentire la modifica dell'accesso agli oggetti il controllo delle opzioni delle singole risorse, ad esempio file, attivo Oggetti directory e le chiavi del Registro di sistema.  
  
### <a name="setting-windows-audit-policy-by-using-group-policy"></a>Impostazione dei criteri di controllo di Windows tramite criteri di gruppo

Per impostare i criteri di controllo mediante criteri di gruppo, configurare le categorie di controllo appropriata sotto **Configurazione computer\Impostazioni di Windows\Impostazioni di sicurezza\Criteri Locali\criteri** (vedere lo screenshot seguente per un ad esempio dall'Editor criteri di gruppo locali (gpedit. msc)). Ogni categoria di criteri di controllo può essere abilitata per **Success**, **errore**, o **successo** ed eventi di errore.  
  
![monitoraggio di Active Directory](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
Usando i criteri di gruppo locale o Active Directory, è possibile impostare criteri di controllo avanzati. Per impostare criteri avanzati di controllo, configurare le sottocategorie appropriate sotto **Configurazione computer\Impostazioni di Windows\Impostazioni di sicurezza\configurazione avanzata dei criterio** (vedere lo screenshot seguente per un esempio tratto da locale Editor criteri di gruppo (gpedit. msc)). Ogni sottocategoria di criteri di controllo può essere abilitata per **Success**, **errore**, o **successo** e **errore** gli eventi.  
  
![monitoraggio di Active Directory](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
### <a name="setting-windows-audit-policy-using-auditpolexe"></a>Impostazione criteri di controllo di Windows mediante Auditpol.exe

Auditpol.exe (per l'impostazione dei criteri di controllo di Windows) è stata introdotta in Windows Server 2008 e Windows Vista. Inizialmente, solo auditpol.exe può essere utilizzato per impostare criteri avanzati di controllo, ma è possibile utilizzare criteri di gruppo in Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, Windows 8 e Windows 7.  
  
Auditpol.exe è un'utilità della riga di comando. La sintassi è come segue:  
  
`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`
  
Esempi di sintassi Auditpol.exe:  
  
`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"logon" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`
  
> [!NOTE]  
> Auditpol.exe imposta criteri di controllo avanzati in locale. Se è in conflitto dei criteri locali con Active Directory o criteri di gruppo locali, in genere le impostazioni di criteri di gruppo sono prioritari rispetto auditpol.exe impostazioni. Quando sono presenti più gruppo o i conflitti tra criteri locale, un solo criterio farà fede (che è, Sostituisci). I criteri di controllo non esegue l'unione.  
  
#### <a name="scripting-auditpol"></a>Scripting Auditpol

Microsoft fornisce una [script di esempio](https://support.microsoft.com/kb/921469) agli amministratori di impostare criteri di controllo avanzate tramite uno script anziché digitare manualmente in ogni comando auditpol.exe.  
  
**Nota** criteri di gruppo non sempre segnalare con precisione lo stato di tutti i criteri di controllo abilitati, mentre auditpol.exe supporta. Visualizzare [ottenere i criteri di controllo valide in Windows 7 e Windows 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) per altri dettagli.  
  
#### <a name="other-auditpol-commands"></a>Altri comandi Auditpol

Auditpol.exe utilizzabile per salvare e ripristinare un criterio di controllo locale e per visualizzare altri comandi di controllo correlati. Ecco l'altra **auditpol** comandi.  
  
`auditpol /clear` -Viene usato per cancellare e reimpostare i criteri di controllo locale  
  
`auditpol /backup /file:<filename>` -Viene usato per eseguire il backup di un criterio di controllo locale corrente in un file binario  
  
`auditpol /restore /file:<filename>` -Viene usato per importare un file di criteri di controllo salvato in precedenza a un criterio di controllo locale  
  
`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>` -Se questa impostazione dei criteri di controllo è abilitata, fa sì che il sistema si arresta immediatamente (con interruzione: Messaggio {controllo Failed} C0000244) se il controllo di sicurezza non può essere registrato per qualsiasi motivo. In genere, un evento si verifica un errore viene registrato quando il log di controllo di sicurezza è pieno e il metodo di memorizzazione specificato per il log di sicurezza è **non sovrascrivere eventi** oppure **sovrascrivere eventi ogni giorno**. In genere è abilitata solo per gli ambienti che richiedono maggiore certezza che si sta connettendo il Registro di sicurezza. Se abilitata, gli amministratori devono strettamente guardare le dimensioni del log di sicurezza e rotazione dei log in base alle esigenze. È possibile impostarla anche con i criteri di gruppo modificando l'opzione di sicurezza **controllo: Arresto del sistema immediato se non è possibile registrare i controlli di sicurezza** (impostazione predefinita = disabled).  
  
`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>` -Questa impostazione dei criteri di controllo consente di controllare l'accesso degli oggetti di sistema globali. Se questo criterio è abilitato, le dimensioni di oggetti di sistema, ad esempio i mutex, gli eventi, i semafori e DOS dispositivi deve essere creato con un elenco di controllo di accesso del sistema (SACL) predefinito. La maggior parte degli amministratori di prendere in considerazione il controllo degli oggetti di sistema globale per essere troppo "disturbo" e verrà abilitarlo solo se si sospetta un attacco dannoso. Solo gli oggetti denominati vengono assegnati un SACL. Se è abilitata anche il controllo oggetto accesso controllo criteri (o sottocategoria di controllo oggetto Kernel), viene controllato l'accesso a questi oggetti di sistema. Quando si configura questa impostazione di sicurezza, le modifiche verranno rese effettive solo dopo il riavvio di Windows. Questo criterio può essere impostato anche con criteri di gruppo modificando l'opzione di sicurezza controlla l'accesso a oggetti di sistema globale (impostazione predefinita = disabled).  
  
`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>` -Questa impostazione dei criteri di controllo specifica che devono disporre del SACL quando vengono creati gli oggetti denominati del kernel (ad esempio i mutex e semafori). AuditBaseDirectories interessa gli oggetti contenitore mentre AuditBaseObjects questi influiscono sugli oggetti che non possono contenere altri oggetti.  
  
`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>` -Questa impostazione dei criteri di controllo specifica se il client genera un evento quando uno o più di questi privilegi vengono assegnati a un token di sicurezza utente: AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege, TakeOwnershipPrivilege e TcbPrivilege. Se questa opzione non è abilitata (impostazione predefinita = Disabled), non vengono registrati i BackupPrivilege RestorePrivilege dei privilegi di amministratore. L'abilitazione di questa opzione può rendere il registro protezione estremamente fastidiosi (talvolta centinaia di eventi di un secondo) durante un'operazione di backup. Questo criterio può essere impostato anche con criteri di gruppo modificando l'opzione di sicurezza **controllo: Controlla l'utilizzo dei privilegi di Backup e ripristino**.  
  
> [!NOTE]  
> Alcune informazioni disponibili in questa pagina è state create da Microsoft [tipo di opzione di controllo](https://msdn.microsoft.com/library/dd973862(prot.20).aspx) e lo strumento Microsoft SCM.  
  
## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>Applicare il controllo tradizionali o il controllo avanzato

In Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 e Windows Vista, gli amministratori possono scegliere di abilitare le nove categorie tradizionale o utilizzare le sottocategorie. È una scelta binaria che deve essere apportata in ogni sistema Windows. Le categorie principali possono essere abilitate o il subcategoriesit non può essere contemporaneamente.  
  
Per impedire che i criteri di categoria tradizionale legacy sovrascrivendo le sottocategorie dei criteri di controllo, è necessario abilitare la **forza subcategory criteri di controllo (Windows Vista o versioni successive) per eseguire l'override le impostazioni di categoria di criteri di controllo** impostazione dei criteri che si trova sotto **Configurazione computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri Locali\opzioni di protezione**.  
  
È consigliabile che le sottocategorie di essere abilitate e configurate anziché i nove categorie principali. Ciò richiede che un'impostazione di criteri di gruppo sia abilitata (per consentire le sottocategorie eseguire l'override di categorie di controllo) con la configurazione di diverse sottocategorie che supportano i criteri di controllo.  
  
Le sottocategorie di controllo può essere configurato usando diversi metodi, inclusi i criteri di gruppo e il programma della riga di comando, auditpol.exe.  
  
## <a name="next-steps"></a>Passaggi successivi
  
* [Sicurezza avanzata di controllo in Windows 7 e Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
* [Il controllo e conformità di Windows Server 2008](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
* [Come usare criteri di gruppo per configurare le impostazioni per computer basati su Windows Server 2008 e Windows Vista in un dominio di Windows Server 2008, in un dominio di Windows Server 2003 o in un dominio Windows 2000 di controllo della sicurezza dettagliata](https://support.microsoft.com/kb/921469)  
  
* [Guida dettagliata ai criteri di controllo sicurezza avanzata](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
