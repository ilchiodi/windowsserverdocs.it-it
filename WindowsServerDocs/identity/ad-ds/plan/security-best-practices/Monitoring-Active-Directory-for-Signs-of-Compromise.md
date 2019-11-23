---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: Monitoraggio dei segnali di compromissione di Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ba67a5fcc127bbe6ffce9454ff98fd3bc3725e55
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367706"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitoraggio dei segnali di compromissione di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Legge numero cinque: la vigilanza eterna è il prezzo di sicurezza.* - [10 leggi non modificabili dell'amministrazione della sicurezza](https://technet.microsoft.com/library/cc722488.aspx)  
  
Un sistema di monitoraggio dei log eventi è una parte essenziale di qualsiasi progetto di Active Directory protetto. Molti compromessi per la sicurezza del computer potrebbero essere individuati prima dell'evento se le vittime hanno applicato il monitoraggio e gli avvisi appropriati del log eventi. La conclusione dei report indipendenti è a lungo supportata. Ad esempio, il [report di violazione dei dati di 2009 Verizon](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) indica:  
  
"L'inefficacia apparente del monitoraggio degli eventi e dell'analisi dei log continua a essere un enigma. La possibilità di rilevamento è disponibile; gli investigatori hanno notato che il 66% delle vittime aveva un'evidenza sufficiente disponibile nei log per individuare la violazione che era stata più diligente nell'analisi di tali risorse ".  
  
Questa mancanza di monitoraggio dei registri eventi attivi rimane un punto di debolezza coerente nei piani di difesa della sicurezza di molte aziende. Il [report di violazione dei dati di 2012 Verizon](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf) ha rilevato che anche se il 85% delle violazioni ha richiesto alcune settimane, il 84% delle vittime ha rilevato la violazione nei log eventi.  
  
## <a name="windows-audit-policy"></a>Criteri di controllo di Windows

Di seguito sono riportati i collegamenti al Blog di supporto Microsoft Official Enterprise. Il contenuto di questi blog fornisce consigli, indicazioni e consigli sul controllo che consentiranno di migliorare la sicurezza dell'infrastruttura di Active Directory e rappresentano una risorsa preziosa per la progettazione di un criterio di controllo.  
  
* Il [controllo dell'accesso agli oggetti globale è](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) un meccanismo di controllo denominato configurazione avanzata dei criteri di controllo, che è stato aggiunto a Windows 7 e windows Server 2008 R2, che consente di impostare i tipi di dati da controllare in modo semplice e non manipolare gli script e Auditpol. exe.  
* [Introduzione alle modifiche di controllo in windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) : introduce le modifiche di controllo apportate in windows Server 2008.  
* [Trucchi di controllo interessanti in vista e 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) : illustra le funzionalità di controllo interessanti di Windows Vista e windows Server 2008 che possono essere usate per la risoluzione dei problemi o per vedere cosa accade nell'ambiente.  
* [Un'unica tappa per il controllo in Windows server 2008 e Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) : contiene una compilazione di funzionalità di controllo e informazioni contenute in windows Server 2008 e Windows Vista.  
  
I collegamenti seguenti forniscono informazioni sui miglioramenti apportati al controllo di Windows in Windows 8 e Windows Server 2012 e informazioni sul controllo di servizi di dominio Active Directory in Windows Server 2008.  
  
* Novità del [controllo della sicurezza](https://technet.microsoft.com/library/hh849638.aspx) : offre una panoramica delle nuove funzionalità di controllo della sicurezza in Windows 8 e windows Server 2012.  
* [Guida dettagliata al controllo di servizi di dominio Active Directory](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) : descrive la nuova funzionalità di controllo Active Directory Domain Services (ad DS) in Windows Server 2008. Sono inoltre disponibili procedure per l'implementazione di questa nuova funzionalità.  
  
### <a name="windows-audit-categories"></a>Categorie di controllo di Windows

Prima di Windows Vista e Windows Server 2008, Windows aveva solo nove categorie di criteri di controllo del registro eventi:  
  
* Eventi di accesso account  
* Gestione account  
* Accesso al servizio directory  
* Eventi di accesso  
* Accesso agli oggetti  
* Modifica dei criteri  
* Uso dei privilegi  
* Rilevamento processi  
* Eventi di sistema  
  
Queste nove categorie di controllo tradizionali comprendono un criterio di controllo. Ogni categoria di criteri di controllo può essere abilitata per eventi di esito positivo, negativo o esito positivo e negativo. Le descrizioni sono incluse nella sezione successiva.  
  
#### <a name="audit-policy-category-descriptions"></a>Descrizioni delle categorie dei criteri di controllo  
Le categorie di criteri di controllo abilitano i tipi di messaggi del registro eventi seguenti.  
  
##### <a name="audit-account-logon-events"></a>Controlla eventi di accesso account  
Segnala ogni istanza di un'entità di sicurezza (ad esempio, utente, computer o account del servizio) che esegue l'accesso o la disconnessione da un computer in cui viene usato un altro computer per convalidare l'account. Gli eventi di accesso all'account vengono generati quando un account dell'entità di sicurezza del dominio viene autenticato in un controller di dominio. L'autenticazione di un utente locale in un computer locale genera un evento di accesso che viene registrato nel registro di sicurezza locale. Nessun evento di disconnessione account registrato.  
  
Questa categoria genera un numero elevato di "rumori", perché Windows continua ad accedere e disattivare i computer locali e remoti durante il normale svolgimento delle attività. Tuttavia, qualsiasi piano di sicurezza deve includere l'esito positivo e negativo di questa categoria di controllo.  
  
##### <a name="audit-account-management"></a>Controllare la gestione degli account  
Questa impostazione di controllo determina se tenere traccia della gestione di utenti e gruppi. È ad esempio possibile tenere traccia di utenti e gruppi quando un account utente o computer, un gruppo di sicurezza o un gruppo di distribuzione viene creato, modificato o eliminato. Quando un account utente o computer viene rinominato, disabilitato o abilitato; oppure quando viene modificata la password di un utente o di un computer. Un evento può essere generato per utenti o gruppi aggiunti o rimossi da altri gruppi.  
  
##### <a name="audit-directory-service-access"></a>Controlla Accesso al servizio directory  

Questa impostazione criterio consente di determinare se controllare l'accesso dell'entità di sicurezza a un oggetto Active Directory che dispone di un proprio elenco di controllo di accesso di sistema (SACL) specificato. In generale, questa categoria deve essere abilitata solo nei controller di dominio. Se abilitata, questa impostazione genera molto rumore.  
  
##### <a name="audit-logon-events"></a>Controlla eventi di accesso  
Gli eventi di accesso vengono generati quando un'entità di sicurezza locale viene autenticata in un computer locale. Gli eventi di accesso registrano gli accessi al dominio che si verificano nel computer locale. Gli eventi di disconnessione dell'account non vengono generati. Se abilitata, gli eventi di accesso generano molto rumore, ma devono essere abilitati per impostazione predefinita in qualsiasi piano di controllo della sicurezza.  
  
##### <a name="audit-object-access"></a>Controllare l'accesso agli oggetti  
L'accesso agli oggetti può generare eventi quando viene eseguito l'accesso a oggetti definiti successivamente con controllo abilitato, ad esempio aperto, letto, rinominato, eliminato o chiuso. Una volta abilitata la categoria di controllo principale, l'amministratore deve definire individualmente gli oggetti per i quali sarà abilitato il controllo. Molti oggetti di sistema Windows sono abilitati per il controllo, pertanto l'abilitazione di questa categoria inizia in genere a generare eventi prima che l'amministratore ne abbia definito uno.  
  
Questa categoria è molto "rumorosa" e genererà da cinque a dieci eventi per ogni accesso agli oggetti. Può essere difficile per gli amministratori che non hanno familiarità con il controllo degli oggetti per ottenere informazioni utili. Deve essere abilitato solo quando necessario.  
  
##### <a name="auditing-policy-change"></a>Modifica dei criteri di controllo  
Questa impostazione dei criteri determina se controllare ogni incidenza di una modifica ai criteri di assegnazione dei diritti utente, ai criteri di Windows Firewall, ai criteri di attendibilità o alle modifiche apportate ai criteri di controllo. Questa categoria deve essere abilitata in tutti i computer. Genera un rumore molto ridotto.  
  
##### <a name="audit-privilege-use"></a>Controllare l'uso dei privilegi  

Sono disponibili dozzine di diritti utente e autorizzazioni in Windows (ad esempio, l'accesso come processo batch e agiscono come parte del sistema operativo). Questa impostazione dei criteri determina se controllare ogni istanza di un'entità di sicurezza esercitando un diritto utente o un privilegio. L'abilitazione di questa categoria comporta una grande quantità di "rumore", ma può essere utile per tenere traccia degli account dell'entità di sicurezza utilizzando privilegi elevati.  
  
##### <a name="audit-process-tracking"></a>Controllo del rilevamento dei processi  
Questa impostazione di criteri determina se controllare le informazioni dettagliate sul rilevamento dei processi per eventi quali l'attivazione del programma, l'uscita del processo, la duplicazione degli handle e l'accesso indiretto agli oggetti. È utile per tenere traccia di utenti malintenzionati e dei programmi che usano.  
  
L'abilitazione del rilevamento del processo di controllo genera un numero elevato di eventi, pertanto viene in genere impostato su **Nessun controllo**. Tuttavia, questa impostazione può offrire un notevole vantaggio durante una risposta agli eventi imprevisti dal log dettagliato dei processi avviati e dal momento in cui sono stati avviati. Per i controller di dominio e altri server di infrastruttura a ruolo singolo, questa categoria può essere attivata in modo sicuro ogni volta. I server con ruolo singolo non generano molto traffico di rilevamento dei processi durante il normale svolgimento delle proprie mansioni. Di conseguenza, possono essere abilitati per acquisire eventi non autorizzati se si verificano.  
  
##### <a name="system-events-audit"></a>Controllo degli eventi di sistema  

Gli eventi di sistema sono quasi una categoria generica catch-all, che registra vari eventi che influiscano sul computer, sulla relativa sicurezza del sistema o sul Registro di sicurezza. Sono inclusi gli eventi per arresti e riavvii del computer, interruzioni dell'alimentazione, modifiche dell'ora di sistema, inizializzazioni dei pacchetti di autenticazione, cancellazioni dei log di controllo, problemi di rappresentazione e un host di altri eventi generali. In generale, l'abilitazione di questa categoria di controllo genera molti "rumori", ma genera un numero sufficiente di eventi molto utili che è difficile consigliarvi di non abilitarlo.  
  
#### <a name="advanced-audit-policies"></a>Criteri di controllo avanzati

A partire da Windows Vista e Windows Server 2008, Microsoft ha migliorato il modo in cui le selezioni delle categorie del registro eventi possono essere apportate creando sottocategorie in ogni categoria di controllo principale. Le sottocategorie consentono un controllo molto più granulare rispetto a quello che potrebbe altrimenti usare le categorie principali. Utilizzando le sottocategorie, è possibile abilitare solo parti di una categoria principale specifica e ignorare la generazione di eventi per i quali non si utilizza. Ogni sottocategoria dei criteri di controllo può essere abilitata per eventi di tipo Esito positivo, Esito negativo o Esito positivo e negativo.  
  
Per elencare tutte le sottocategorie di controllo disponibili, esaminare il contenitore dei criteri di controllo avanzati in un oggetto Criteri di gruppo o digitare quanto segue al prompt dei comandi in qualsiasi computer che esegue Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, Windows 8, Windows 7 o Windows Vista:  
  
`auditpol /list /subcategory:*`
  
Per ottenere un elenco delle sottocategorie di controllo attualmente configurate in un computer che esegue Windows Server 2012, Windows Server 2008 R2 o Windows 2008, digitare quanto segue:  
  
`auditpol /get /category:*`
  
Lo screenshot seguente mostra un esempio di Auditpol. exe che elenca i criteri di controllo correnti.  
  
![monitoraggio di Active Directory](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> Criteri di gruppo non segnala sempre accuratamente lo stato di tutti i criteri di controllo abilitati, mentre Auditpol. exe lo esegue. Per altri dettagli, vedere [ottenere i criteri di controllo effettivi in Windows 7 e 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) .  
  
Ogni categoria principale ha più sottocategorie. Di seguito è riportato un elenco di categorie, le rispettive sottocategorie e una descrizione delle relative funzioni.  
  
### <a name="auditing-subcategories-descriptions"></a>Descrizioni delle sottocategorie di controllo  
Le sottocategorie dei criteri di controllo abilitano i tipi di messaggi del registro eventi seguenti:  
  
#### <a name="account-logon"></a>Accesso all'account  
  
##### <a name="credential-validation"></a>Convalida delle credenziali  
Questa sottocategoria riporta i risultati dei test di convalida sulle credenziali inviate per una richiesta di accesso all'account utente. Questi eventi si verificano nel computer autorevole per le credenziali. Per gli account di dominio, il controller di dominio è autorevole, mentre per gli account locali il computer locale è autorevole.  
  
Negli ambienti di dominio la maggior parte degli eventi di accesso agli account viene registrata nel registro di protezione dei controller di dominio autorevoli per gli account di dominio. Tuttavia, questi eventi possono verificarsi in altri computer dell'organizzazione quando vengono utilizzati account locali per l'accesso.  
  
##### <a name="kerberos-service-ticket-operations"></a>Operazioni del ticket di servizio Kerberos  
Questa sottocategoria segnala gli eventi generati dai processi di richiesta ticket Kerberos nel controller di dominio autorevole per l'account di dominio.  
  
##### <a name="kerberos-authentication-service"></a>Servizio di autenticazione Kerberos  
Questa sottocategoria segnala gli eventi generati dal servizio di autenticazione Kerberos. Questi eventi si verificano nel computer autorevole per le credenziali.  
  
##### <a name="other-account-logon-events"></a>Altri eventi di accesso account  
Questa sottocategoria segnala gli eventi che si verificano in risposta alle credenziali inviate per una richiesta di accesso all'account utente che non si riferiscono alla convalida delle credenziali o ai ticket Kerberos. Questi eventi si verificano nel computer autorevole per le credenziali. Per gli account di dominio, il controller di dominio è autorevole, mentre per gli account locali il computer locale è autorevole.  
  
Negli ambienti di dominio la maggior parte degli eventi di accesso agli account viene registrata nel registro di protezione dei controller di dominio autorevoli per gli account di dominio. Tuttavia, questi eventi possono verificarsi in altri computer dell'organizzazione quando vengono utilizzati account locali per l'accesso. Gli esempi possono includere quanto segue:  
  
* Disconnessione Servizi Desktop remoto sessione  
* Nuove sessioni di Servizi Desktop remoto  
* Bloccare e sbloccare una workstation  
* Richiamo di un screen saver  
* Chiusura di un screen saver  
* Rilevamento di un attacco di riproduzione Kerberos, in cui una richiesta Kerberos con informazioni identiche viene ricevuta due volte  
* Accesso a una rete wireless concessa a un account utente o computer  
* Accesso a una rete 802.1 x cablata concessa a un account utente o computer  
  
#### <a name="account-management"></a>Gestione account  
  
##### <a name="user-account-management"></a>Gestione account utente  
Questa sottocategoria segnala ogni evento di gestione degli account utente, ad esempio quando un account utente viene creato, modificato o eliminato. un account utente è stato rinominato, disabilitato o abilitato. in alternativa, viene impostata o modificata una password. Se questa impostazione dei criteri di controllo è abilitata, gli amministratori possono tenere traccia degli eventi per rilevare la creazione dannosa, accidentale e autorizzata degli account utente.  
  
##### <a name="computer-account-management"></a>Gestione degli account computer  
Questa sottocategoria segnala ogni evento di gestione degli account computer, ad esempio quando un account computer viene creato, modificato, eliminato, rinominato, disabilitato o abilitato.  
  
##### <a name="security-group-management"></a>Gestione dei gruppi di sicurezza  
Questa sottocategoria segnala ogni evento di gestione dei gruppi di sicurezza, ad esempio quando viene creato, modificato o eliminato un gruppo di sicurezza o quando un membro viene aggiunto o rimosso da un gruppo di sicurezza. Se questa impostazione dei criteri di controllo è abilitata, gli amministratori possono tenere traccia degli eventi per rilevare la creazione dannosa, accidentale e autorizzata degli account del gruppo di sicurezza.  
  
##### <a name="distribution-group-management"></a>Gestione dei gruppi di distribuzione  
Questa sottocategoria segnala ogni evento di gestione dei gruppi di distribuzione, ad esempio quando un gruppo di distribuzione viene creato, modificato o eliminato o quando un membro viene aggiunto o rimosso da un gruppo di distribuzione. Se questa impostazione dei criteri di controllo è abilitata, gli amministratori possono tenere traccia degli eventi per rilevare la creazione di account di gruppo dannosi, accidentali e autorizzati.  
  
##### <a name="application-group-management"></a>Gestione dei gruppi di applicazioni  
Questa sottocategoria segnala ogni evento di gestione dei gruppi di applicazioni in un computer, ad esempio quando viene creato, modificato o eliminato un gruppo di applicazioni o quando un membro viene aggiunto o rimosso da un gruppo di applicazioni. Se questa impostazione dei criteri di controllo è abilitata, gli amministratori possono tenere traccia degli eventi per rilevare la creazione dannosa, accidentale e autorizzata degli account del gruppo di applicazioni.  
  
##### <a name="other-account-management-events"></a>Altri eventi di gestione degli account  
Questa sottocategoria segnala altri eventi di gestione degli account.  
  
#### <a name="detailed-process-tracking"></a>Rilevamento dettagliato dei processi  
  
##### <a name="process-creation"></a>Creazione di processi  
Questa sottocategoria segnala la creazione di un processo e il nome dell'utente o del programma che l'ha creata.  
  
##### <a name="process-termination"></a>Terminazione processo  
Questa sottocategoria segnala l'interruzione di un processo.  
  
##### <a name="dpapi-activity"></a>Attività DPAPI  
Questa sottocategoria segnala la crittografia o la decrittografia delle chiamate in Data Protection Application Programming Interface (DPAPI). DPAPI viene usato per proteggere le informazioni segrete, ad esempio le informazioni sulla password archiviata e la chiave.  
  
##### <a name="rpc-events"></a>Eventi RPC  
Questa sottocategoria segnala gli eventi di connessione RPC (Remote Procedure Call).  
  
#### <a name="directory-service-access"></a>Accesso al servizio directory  
  
##### <a name="directory-service-access"></a>Accesso al servizio directory  
Questa sottocategoria segnala quando si accede a un oggetto servizi di dominio Active Directory. Solo gli oggetti con SACL configurati comportano la generazione di eventi di controllo e solo quando sono accessibili in un modo che corrisponda alle voci SACL. Questi eventi sono simili agli eventi di accesso al servizio directory nelle versioni precedenti di Windows Server. Questa sottocategoria si applica solo ai controller di dominio.  
  
##### <a name="directory-service-changes"></a>Modifiche al servizio directory  
Questa sottocategoria segnala le modifiche agli oggetti in servizi di dominio Active Directory. I tipi di modifiche segnalati sono le operazioni di creazione, modifica, spostamento e annullamento dell'eliminazione eseguite su un oggetto. Il controllo delle modifiche del servizio directory, ove appropriato, indica i valori vecchi e nuovi delle proprietà modificate degli oggetti modificati. Solo gli oggetti con SACL comportano la generazione di eventi di controllo e solo quando vengono utilizzati in modo che corrispondano alle relative voci SACL. Alcuni oggetti e proprietà non causano la generazione di eventi di controllo a causa delle impostazioni della classe di oggetti nello schema. Questa sottocategoria si applica solo ai controller di dominio.  
  
##### <a name="directory-service-replication"></a>Replica del servizio directory  
Questa sottocategoria segnala quando la replica tra due controller di dominio inizia e termina.  
  
##### <a name="detailed-directory-service-replication"></a>Replica dettagliata del servizio directory  
Questa sottocategoria riporta informazioni dettagliate sulle informazioni replicate tra i controller di dominio. Questi eventi possono essere molto elevati nel volume.  
  
#### <a name="logonlogoff"></a>Accesso/disconnessione  
  
##### <a name="logon"></a>Accesso  
Questa sottocategoria segnala quando un utente tenta di accedere al sistema. Questi eventi si verificano nel computer a cui si accede. Per gli accessi interattivi, la generazione di questi eventi si verifica nel computer connesso a. Se si verifica un accesso di rete per accedere a una condivisione, questi eventi vengono generati nel computer che ospita la risorsa a cui si accede. Se questa impostazione è configurata su **Nessun controllo**, è difficile o Impossibile determinare quale utente ha effettuato l'accesso o ha tentato di accedere ai computer dell'organizzazione.  
  
##### <a name="network-policy-server"></a>Server dei criteri di rete  
Questa sottocategoria segnala gli eventi generati dalle richieste di accesso utente RADIUS (IAS) e protezione accesso alla rete (NAP). Queste richieste possono essere **Grant**, **Deny**, **scarto**, **quarantena**, **Lock**e **Unlock**. Il controllo di questa impostazione comporterà un volume medio o elevato di record nei server NPS e IAS.  
  
##### <a name="ipsec-main-mode"></a>Modalità principale IPsec  
Questa sottocategoria segnala i risultati del protocollo Internet Key Exchange (IKE) e Authenticated Internet Protocol (AuthIP) durante le negoziazioni in modalità principale.  
  
##### <a name="ipsec-extended-mode"></a>Modalità estesa IPsec  
Questa sottocategoria segnala i risultati di AuthIP durante le negoziazioni in modalità estesa.  
  
##### <a name="other-logonlogoff-events"></a>Altri eventi di accesso/disconnessione  
Questa sottocategoria segnala altri eventi di accesso e di disconnessione, ad esempio la disconnessione e la riconnessione di Servizi Desktop remoto sessione, l'utilizzo di RunAs per l'esecuzione di processi con un account diverso e il blocco e lo sblocco di una workstation.  
  
##### <a name="logoff"></a>Disconnessione  
Questa sottocategoria segnala quando un utente si disconnette dal sistema. Questi eventi si verificano nel computer a cui si accede. Per gli accessi interattivi, la generazione di questi eventi si verifica nel computer connesso a. Se si verifica un accesso di rete per accedere a una condivisione, questi eventi vengono generati nel computer che ospita la risorsa a cui si accede. Se questa impostazione è configurata su **Nessun controllo**, è difficile o Impossibile determinare quale utente ha effettuato l'accesso o ha tentato di accedere ai computer dell'organizzazione.  
  
##### <a name="account-lockout"></a>Blocco account  
Questa sottocategoria segnala quando un account utente viene bloccato in seguito a un numero eccessivo di tentativi di accesso non riusciti.  
  
##### <a name="ipsec-quick-mode"></a>Modalità rapida IPsec  
Questa sottocategoria segnala i risultati del protocollo IKE e AuthIP durante le negoziazioni in modalità rapida.  
  
##### <a name="special-logon"></a>Accesso speciale  
Questa sottocategoria segnala quando viene utilizzato un accesso speciale. Un accesso speciale è un accesso con privilegi di amministratore equivalenti e può essere usato per elevare un processo a un livello superiore.  
  
#### <a name="policy-change"></a>Modifica dei criteri  
  
##### <a name="audit-policy-change"></a>Modifica dei criteri di controllo  
Questa sottocategoria segnala le modifiche ai criteri di controllo, incluse le modifiche SACL.  
  
##### <a name="authentication-policy-change"></a>Modifica dei criteri di autenticazione  
Questa sottocategoria segnala le modifiche ai criteri di autenticazione.  
  
##### <a name="authorization-policy-change"></a>Modifica dei criteri di autorizzazione  
Questa sottocategoria segnala le modifiche apportate ai criteri di autorizzazione, incluse le autorizzazioni (DACL).  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Modifica dei criteri a livello di regola MPSSVC  
Questa sottocategoria segnala le modifiche apportate alle regole dei criteri utilizzate da Microsoft Protection Service (MPSSVC. exe). Questo servizio viene utilizzato da Windows Firewall.  
  
##### <a name="filtering-platform-policy-change"></a>Modifica dei criteri della piattaforma di filtro  
Questa sottocategoria segnala l'aggiunta e la rimozione di oggetti da WFP, inclusi i filtri di avvio. Questi eventi possono essere molto elevati nel volume.  
  
##### <a name="other-policy-change-events"></a>Altri eventi di modifica dei criteri  
Questa sottocategoria segnala altri tipi di modifiche ai criteri di sicurezza, ad esempio la configurazione del Trusted Platform Module (TPM) o dei provider di crittografia.  
  
#### <a name="privilege-use"></a>Uso dei privilegi  
  
##### <a name="sensitive-privilege-use"></a>Uso dei privilegi sensibili  
Questa sottocategoria segnala quando un account utente o un servizio utilizza un privilegio sensibile. Un privilegio sensibile include i diritti utente seguenti: agire come parte del sistema operativo, eseguire il backup di file e directory, creare un oggetto token, eseguire il debug di programmi, abilitare account computer e utente come trusted per la delega, generare controlli di sicurezza, rappresenta un client dopo l'autenticazione, il caricamento e lo scaricamento dei driver di dispositivo, la gestione del registro di controllo e di sicurezza, la modifica dei valori di ambiente firmware, la sostituzione di un token a livello di processo, il ripristino di file e directory e la proprietà di file o altri oggetti. Il controllo di questa sottocategoria creerà un volume elevato di eventi.  
  
##### <a name="nonsensitive-privilege-use"></a>Uso dei privilegi non sensibili  
Questa sottocategoria segnala quando un account utente o un servizio utilizza un privilegio non sensibile. Un privilegio non sensibile include i diritti utente seguenti: accedere a gestione credenziali come un chiamante attendibile, accedere al computer dalla rete, aggiungere workstation al dominio, modificare le quote di memoria per un processo, consentire l'accesso locale, consentire l'accesso tramite remoto Servizi Desktop, ignorare controllo incrociato, modificare l'ora di sistema, creare un file di paging, creare oggetti globali, creare oggetti condivisi permanenti, creare collegamenti simbolici, negare l'accesso al computer dalla rete, negare l'accesso come processo batch, negare l'accesso come servizio, Nega accesso locale, Nega accesso tramite Servizi Desktop remoto, forza arresto da un sistema remoto, aumenta un processo working set, aumenta priorità di pianificazione, blocca le pagine in memoria, accedi come processo batch, accedi come servizio, modifica etichetta oggetto, Esegui volume attività di manutenzione, profilo singolo processo, profilatura delle prestazioni del sistema, rimozione del computer da docking station, arresto del sistema e sincronizzazione dei dati del servizio directory. Il controllo di questa sottocategoria creerà un volume molto elevato di eventi.  
  
##### <a name="other-privilege-use-events"></a>Altri eventi di utilizzo dei privilegi  
Questa impostazione dei criteri di sicurezza non è attualmente in uso.  
  
#### <a name="object-access"></a>Accesso agli oggetti  
  
##### <a name="file-system"></a>File system  
Questa sottocategoria segnala quando si accede a file system oggetti. Solo file system oggetti con SACL possono generare eventi di controllo e solo quando vengono utilizzati in modo analogo alle relative voci SACL. Questa impostazione di criteri non provocherà il controllo di alcun evento. Determina se controllare l'evento di un utente che accede a un oggetto file system che dispone di un elenco di controllo di accesso di sistema (SACL) specificato, abilitando in modo efficace il controllo.  
  
Se l'impostazione di controllo dell'accesso agli oggetti è configurata su **operazione riuscita**, viene generata una voce di controllo ogni volta che un utente accede correttamente a un oggetto con un SACL specificato. Se questa impostazione di criteri è configurata su non **riuscita**, viene generata una voce di controllo ogni volta che un utente non riesce nel tentativo di accedere a un oggetto con un SACL specificato.  
  
##### <a name="registry"></a>Registro di sistema  
Questa sottocategoria segnala quando viene eseguito l'accesso agli oggetti del registro di sistema. Solo gli oggetti del registro di sistema con SACL provocano la generazione di eventi di controllo e solo quando vengono utilizzati in modo analogo alle relative voci SACL. Questa impostazione di criteri non provocherà il controllo di alcun evento.  
  
##### <a name="kernel-object"></a>Oggetto kernel  
Questa sottocategoria segnala quando si accede a oggetti kernel, ad esempio processi e mutex. Solo gli oggetti kernel con SACL comportano la generazione di eventi di controllo e solo quando vengono utilizzati in modo analogo alle relative voci SACL. In genere gli oggetti kernel vengono assegnati solo ai SACL se le opzioni di controllo AuditBaseObjects o AuditBaseDirectories sono abilitate.  
  
##### <a name="sam"></a>SAM  
Questa sottocategoria segnala quando si accede a oggetti di database di autenticazione SAM (Local Security Accounts Manager).  
  
##### <a name="certification-services"></a>Servizi di certificazione  
Questa sottocategoria segnala quando vengono eseguite le operazioni dei servizi di certificazione.  
  
##### <a name="application-generated"></a>Applicazione generata  
Questa sottocategoria segnala quando le applicazioni tentano di generare eventi di controllo usando le API (Application Programming Interface) di controllo di Windows.  
  
##### <a name="handle-manipulation"></a>Gestione della manipolazione  
Questa sottocategoria segnala quando un handle per un oggetto viene aperto o chiuso. Solo gli oggetti con SACL provocano la generazione di questi eventi e solo se l'operazione di handle tentata corrisponde alle voci SACL. Gli eventi di manipolazione degli handle vengono generati solo per i tipi di oggetto in cui è abilitata la sottocategoria di accesso agli oggetti corrispondente (ad esempio file system o Registry).  
  
##### <a name="file-share"></a>Condivisione file  
Questa sottocategoria segnala quando si accede a una condivisione file. Questa impostazione di criteri non provocherà il controllo di alcun evento. Determina se controllare l'evento di un utente che accede a un oggetto condivisione file che dispone di un elenco di controllo di accesso di sistema (SACL) specificato, abilitando in modo efficace il controllo.  
  
##### <a name="filtering-platform-packet-drop"></a>Eliminazione di pacchetti di piattaforma di filtro  
Questa sottocategoria segnala quando i pacchetti vengono eliminati da Windows Filtering Platform (PAM). Questi eventi possono essere molto elevati nel volume.  
  
##### <a name="filtering-platform-connection"></a>Connessione alla piattaforma di filtro  
Questa sottocategoria segnala quando le connessioni sono consentite o bloccate da WFP. Questi eventi possono essere elevati nel volume.  
  
##### <a name="other-object-access-events"></a>Altri eventi di accesso agli oggetti  
Questa sottocategoria segnala altri eventi correlati all'accesso agli oggetti, ad esempio Utilità di pianificazione processi e gli oggetti COM+.  
  
#### <a name="system"></a>Sistema  
  
##### <a name="security-state-change"></a>Modifica dello stato di sicurezza  
Questa sottocategoria segnala le modifiche allo stato di sicurezza del sistema, ad esempio quando viene avviato e interrotto il sottosistema di sicurezza.  
  
##### <a name="security-system-extension"></a>Estensione del sistema di sicurezza  
Questa sottocategoria segnala il caricamento del codice di estensione, ad esempio i pacchetti di autenticazione da parte del sottosistema di sicurezza.  
  
##### <a name="system-integrity"></a>Integrità del sistema  
Questa sottocategoria segnala le violazioni dell'integrità del sottosistema di sicurezza.  
  
Driver IPsec  
  
Questa sottocategoria segnala le attività del driver Internet Protocol Security (IPsec).  
  
##### <a name="other-system-events"></a>Altri eventi di sistema  
Questa sottocategoria segnala gli altri eventi di sistema.  
  
Per ulteriori informazioni sulle descrizioni della sottocategoria, consultare lo [strumento Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  
  
Ogni organizzazione deve esaminare le categorie e le sottocategorie descritte in precedenza e abilitare quelle che meglio si adattano all'ambiente. Le modifiche ai criteri di controllo devono sempre essere testate prima della distribuzione in un ambiente di produzione.  
  
## <a name="configuring-windows-audit-policy"></a>Configurazione dei criteri di controllo di Windows

I criteri di controllo di Windows possono essere impostati tramite criteri di gruppo, Auditpol. exe, API o modifiche al registro di sistema. I metodi consigliati per la configurazione dei criteri di controllo per la maggior parte delle aziende sono Criteri di gruppo o Auditpol. exe. L'impostazione di un criterio di controllo del sistema richiede autorizzazioni dell'account a livello di amministratore o autorizzazioni delegate appropriate.  
  
> [!NOTE]  
> Il privilegio **gestione del registro di controllo e di protezione** deve essere assegnato alle entità di sicurezza (per impostazione predefinita, gli amministratori) per consentire la modifica delle opzioni di controllo dell'accesso agli oggetti delle singole risorse, ad esempio file, oggetti Active Directory e chiavi del registro di sistema.  
  
### <a name="setting-windows-audit-policy-by-using-group-policy"></a>Impostazione dei criteri di controllo di Windows tramite Criteri di gruppo

Per impostare i criteri di controllo mediante criteri di gruppo, configurare le categorie di controllo appropriate situate in **Configurazione computer\Impostazioni di Windows\Impostazioni protezione\Criteri locali\Criteri controllo criterio** (vedere lo screenshot seguente per un esempio di Editor criteri di gruppo locali (gpedit. msc)). Ogni categoria di criteri di controllo può essere abilitata per eventi di **esito positivo**, **negativo**o **esito positivo** e negativo.  
  
![monitoraggio di Active Directory](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
I criteri di controllo avanzati possono essere impostati utilizzando Active Directory o criteri di gruppo locali. Per impostare i criteri di controllo avanzati, configurare le sottocategorie appropriate situate in **Configurazione computer\Impostazioni di Windows\Impostazioni sicurezza\Configurazione avanzata dei di controllo** (vedere lo screenshot seguente per un esempio della Editor criteri di gruppo locali (gpedit. msc)). Ogni sottocategoria dei criteri di controllo può essere abilitata per eventi di **esito positivo**, **negativo**o **esito positivo** e **negativo** .  
  
![monitoraggio di Active Directory](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
### <a name="setting-windows-audit-policy-using-auditpolexe"></a>Impostazione dei criteri di controllo di Windows tramite Auditpol. exe

Auditpol. exe (per l'impostazione dei criteri di controllo di Windows) è stato introdotto in Windows Server 2008 e Windows Vista. Inizialmente, è possibile usare solo Auditpol. exe per impostare criteri di controllo avanzati, ma è possibile usare Criteri di gruppo in Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, Windows 8 e Windows 7.  
  
Auditpol. exe è un'utilità della riga di comando. La sintassi è la seguente:  
  
`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`
  
Esempi di sintassi di Auditpol. exe:  
  
`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"logon" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`
  
> [!NOTE]  
> Auditpol. exe imposta localmente i criteri di controllo avanzati. Se i criteri locali sono in conflitto con Active Directory o Criteri di gruppo locali, le impostazioni di Criteri di gruppo in genere prevalgono sulle impostazioni di Auditpol. exe. Quando esistono più conflitti tra gruppi o criteri locali, solo un criterio prevarrà (ovvero Sostituisci). I criteri di controllo non vengono uniti.  
  
#### <a name="scripting-auditpol"></a>Creazione di script per auditpol

In Microsoft è disponibile uno [script di esempio](https://support.microsoft.com/kb/921469) per gli amministratori che desiderano impostare criteri di controllo avanzati utilizzando uno script anziché digitare manualmente ogni comando Auditpol. exe.  
  
**Nota** Criteri di gruppo non segnala sempre accuratamente lo stato di tutti i criteri di controllo abilitati, mentre Auditpol. exe lo esegue. Per altri dettagli, vedere [ottenere i criteri di controllo effettivi in Windows 7 e windows 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) .  
  
#### <a name="other-auditpol-commands"></a>Altri comandi auditpol

Auditpol. exe può essere usato per salvare e ripristinare un criterio di controllo locale e per visualizzare altri comandi correlati al controllo. Ecco gli altri comandi di **auditpol** .  
  
`auditpol /clear`: usato per cancellare e reimpostare i criteri di controllo locali  
  
`auditpol /backup /file:<filename>`: utilizzato per eseguire il backup di un criterio di controllo locale corrente in un file binario  
  
`auditpol /restore /file:<filename>`: usato per importare un file dei criteri di controllo salvato in precedenza in un criterio di controllo locale  
  
`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>`-se questa impostazione dei criteri di controllo è abilitata, il sistema arresta immediatamente (con il messaggio STOP: c0000244 {audit failed}) se non è possibile registrare un controllo di sicurezza per qualsiasi motivo. In genere, un evento non viene registrato quando il registro di controllo di sicurezza è pieno e il metodo di conservazione specificato per il registro di sicurezza **non sovrascrive gli eventi** o **sovrascrive gli eventi in base ai giorni**. In genere viene abilitato solo da ambienti che necessitano di una maggiore sicurezza per la registrazione del log di sicurezza. Se abilitata, gli amministratori devono controllare attentamente le dimensioni del registro di sicurezza e ruotare i log secondo le necessità. Può anche essere impostato con Criteri di gruppo modificando l'opzione di sicurezza **controllo: Arresta sistema immediatamente se non** è possibile registrare i controlli di sicurezza (impostazione predefinita = disabilitato).  
  
`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>`: questa impostazione dei criteri di controllo determina se controllare l'accesso degli oggetti di sistema globali. Se questi criteri sono abilitati, gli oggetti di sistema, quali mutex, eventi, semafori e dispositivi DOS, verranno creati con un elenco di controllo di accesso di sistema (SACL) predefinito. La maggior parte degli amministratori considera il controllo degli oggetti di sistema globali troppo "rumorosi" e li Abilita solo se si sospetta un attacco dannoso. Ai soli oggetti denominati viene assegnato un SACL. Se è abilitato anche il criterio di controllo dell'accesso agli oggetti di controllo (o la sottocategoria controllo oggetto kernel), viene controllato l'accesso a questi oggetti di sistema. Quando si configura questa impostazione di sicurezza, le modifiche diverranno effettive solo dopo il riavvio di Windows. Questo criterio può essere impostato anche con Criteri di gruppo modificando l'opzione di sicurezza controlla l'accesso degli oggetti di sistema globali (impostazione predefinita = disabilitata).  
  
`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>`: questa impostazione dei criteri di controllo specifica che gli oggetti kernel denominati, ad esempio i mutex e i semafori, devono essere assegnati ai SACL quando vengono creati. AuditBaseDirectories influiscono sugli oggetti contenitore mentre AuditBaseObjects influiscono sugli oggetti che non possono contenere altri oggetti.  
  
`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>`-questa impostazione dei criteri di controllo specifica se il client genera un evento quando uno o più di questi privilegi sono assegnati a un token di sicurezza utente: AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege, TakeOwnershipPrivilege e TcbPrivilege. Se questa opzione non è abilitata (impostazione predefinita = disabilitata), i privilegi BackupPrivilege e RestorePrivilege non vengono registrati. L'abilitazione di questa opzione può rendere il registro di sicurezza estremamente rumoroso (talvolta centinaia di eventi al secondo) durante un'operazione di backup. Questo criterio può essere impostato anche con Criteri di gruppo modificando l'opzione di sicurezza **audit: Audit the use of backup and Restore privilege**.  
  
> [!NOTE]  
> Di seguito sono riportate alcune informazioni fornite dal [tipo di opzione di controllo](https://msdn.microsoft.com/library/dd973862(prot.20).aspx) Microsoft e dallo strumento Microsoft SCM.  
  
## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>Applicazione del controllo tradizionale o del controllo avanzato

In Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 e Windows Vista, gli amministratori possono scegliere di abilitare le nove categorie tradizionali o di usare le sottocategorie. Si tratta di una scelta binaria che deve essere eseguita in ogni sistema Windows. È possibile abilitare le categorie principali oppure subcategoriesit.  
  
Per impedire che i criteri di categoria tradizionali legacy sovrascrivano le sottocategorie dei criteri di controllo, è necessario abilitare le impostazioni della sottocategoria **Force audit policy (Windows Vista o versione successiva) per sostituire le impostazioni di categoria dei criteri di controllo** in configurazione **computer\Impostazioni di Windows\Impostazioni protezione\Criteri locali\Opzioni opzioni**.  
  
È consigliabile abilitare e configurare le sottocategorie anziché le nove categorie principali. Questa operazione richiede l'abilitazione di un'impostazione di Criteri di gruppo (per consentire alle sottocategorie di eseguire l'override delle categorie di controllo) insieme alla configurazione delle diverse sottocategorie che supportano i criteri di controllo.  
  
Le sottocategorie di controllo possono essere configurate usando diversi metodi, tra cui Criteri di gruppo e il programma da riga di comando, Auditpol. exe.  
  
## <a name="next-steps"></a>Passaggi successivi
  
* [Controllo avanzato della sicurezza in Windows 7 e Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
* [Controllo e conformità in Windows Server 2008](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
* [Come usare Criteri di gruppo per configurare le impostazioni di controllo della sicurezza dettagliate per computer basati su Windows Vista e Windows Server 2008 in un dominio Windows Server 2008, in un dominio Windows Server 2003 o in un dominio Windows 2000](https://support.microsoft.com/kb/921469)  
  
* [Guida dettagliata ai criteri avanzati di controllo della sicurezza](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
