---
ms.assetid: 0abe0976-4b49-45d6-a7b3-81d28bdb8210
title: Suggerimenti per i criteri di controllo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d03d38834f89f8cda80b7af147e2bd3e31f4f990
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="audit-policy-recommendations"></a>Suggerimenti per i criteri di controllo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questa sezione riguarda le impostazioni di criteri di controllo predefinito di Windows, base consigliabile impostazioni criteri di controllo e i consigli più aggressivi da Microsoft, per i prodotti server e workstation.  

Requisiti di base SCM illustrati di seguito, insieme a queste impostazioni consentono di rilevare una compromissione, servono solo per una Guida di base iniziale per gli amministratori. Ogni organizzazione deve il proprio decisioni riguardanti le minacce che si trovano ad affrontare la tolleranza del rischio accettabile e le categorie dei criteri di controllo o le sottocategorie consentono. Per ulteriori informazioni sulle minacce, vedere il [Guida alle minacce e contromisure](https://technet.microsoft.com/library/hh125921(v=ws.10).aspx). Gli amministratori senza un criterio di controllo ponderato presenti sono invitati a iniziare con le impostazioni consigliate qui e quindi modificare e testare, prima dell'implementazione nel proprio ambiente di produzione.  

I consigli sono per i computer aziendali, che Microsoft definisce come i computer che presentano requisiti di protezione Media e che richiedono un elevato livello di funzionalità operativa. Criteri di controllo entità che richiedono una maggiore sicurezza requisiti considerare più aggressivi.  

> [!NOTE]  
> Impostazioni predefinite di Microsoft Windows e consigli di base sono state il [strumento Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  

Le seguenti impostazioni di criteri di controllo di base sono consigliate per i computer sicurezza normale che non sono noti all'attacco ha esito positivo, attiva da antagonisti o malware.  

## <a name="recommended-audit-policies-by-operating-system"></a>Criteri di controllo dal sistema operativo consigliato  
Questa sezione contiene le tabelle che elenca le impostazioni consigliate di controllo che si applicano ai sistemi operativi seguenti:  

-   Windows Server 2012  

-   Windows Server 2012 R2  

-   Windows Server 2008  

-   Windows 8  

-   Windows 7  

Queste tabelle contengono l'impostazione predefinita di Windows, requisiti di base e i consigli più forte per questi sistemi operativi.  

**Legenda tabelle dei criteri di controllo**  

|||  
|-|-|  
|**Notazione**|**Raccomandazione**|  
|SÌ|In generale abilitare scenari|  
|No|Eseguire **non** in generale abilitare scenari|  
|SE|Abilitare, se necessario per uno scenario specifico o se un ruolo o una funzionalità per cui il controllo desiderato è installato nel computer|  
|CONTROLLER DI DOMINIO|Abilitare nei controller di dominio|  
|[Vuoto]|Alcuna indicazione|  

**Windows 8 e Windows 7 controllo impostazioni consigli**  

**Criteri di controllo**  

|Criteri di controllo, categoria o sottocategoria|Impostazioni predefinite di Windows<br /><br />Errore di operazione riuscita|Indicazione di base<br /><br />Errore di operazione riuscita|Raccomandazione più forte<br /><br />Errore di operazione riuscita|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Accesso account**||||  
|Controlla convalida credenziali|No, no|No Sì|Sì, sì|  
|Controllo servizio di autenticazione Kerberos|||Sì, sì|  
|Controlla operazioni Ticket di servizio Kerberos|||Sì, sì|  
|Controlla altri eventi accesso Account|||Sì, sì|  
|**Gestione degli account**||||  
|Gestione di gruppo di applicazioni di controllo||||  
|Controlla gestione degli Account Computer||No Sì|Sì, sì|  
|Gestione di gruppo di controllo distribuzione||||  
|Controlla altri eventi di gestione di Account||No Sì|Sì, sì|  
|Gestione di gruppo di sicurezza di controllo||No Sì|Sì, sì|  
|Controlla gestione degli Account utente|No Sì|No Sì|Sì, sì|  
|**Analisi dettagliata**||||  
|Controlla attività DPAPI|||Sì, sì|  
|Creazione del processo di controllo||No Sì|Sì, sì|  
|Chiusura del processo di controllo||||  
|Controlla eventi RPC||||  
|**Accesso al servizio directory**||||  
|Controlla replica dettagliata servizio Directory||||  
|Controlla accesso al servizio Directory||||  
|Controlla modifiche servizio Directory||||  
|Controlla replica servizio Directory||||  
|**Accesso e disconnessione**||||  
|Controlla blocco Account|No Sì||No Sì|  
|Attestazioni utente o dispositivo di controllo||||  
|Controlla modalità estesa IPsec||||  
|Modalità principale IPsec di controllo|||SE SE|  
|Controlla modalità rapida IPsec||||  
|Controlla fine sessione|No Sì|No Sì|No Sì|  
|Controllo dell'accesso|No Sì|No Sì|Sì, sì|  
|Server dei criteri di controllo rete|Sì, sì|||  
|Controlla altri eventi di accesso/fine sessione||||  
|Controlla accesso speciale|No Sì|No Sì|Sì, sì|  
|**Accesso agli oggetti**||||  
|Controlla generato dall'applicazione||||  
|Controlla servizi di certificazione||||  
|Controlla condivisione File dettagliata||||  
|Controlla condivisione File||||  
|Controlla File System||||  
|Connessione a piattaforma filtro di controllo||||  
|Controlla mancata elaborazione pacchetti Piattaforma filtro||||  
|Controlla modifica Handle||||  
|Controlla oggetto Kernel||||  
|Controlla altri eventi di accesso di oggetto||||  
|Controlla Registro di sistema||||  
|Controllo archivi rimovibili||||  
|Controlla SAM||||  
|Controlla gestione temporanea criteri di accesso centrale||||  
|**Modifica dei criteri**||||  
|Controlla Modifica criteri di controllo|No Sì|Sì, sì|Sì, sì|  
|Controlla Modifica criteri di autenticazione|No Sì|No Sì|Sì, sì|  
|Controlla Modifica criteri di autorizzazione||||  
|Controlla Modifica criteri piattaforma filtro||||  
|Controlla Modifica criteri a livello di regola MPSSVC|||Sì  |  
|Controlla altri eventi di modifica dei criteri||||  
|**Uso dei privilegi**||||  
|Controlla utilizzo privilegi Non sensibili||||  
|Controlla altri eventi di utilizzo privilegi||||  
|Controlla utilizzo privilegi sensibili||||  
|**Sistema**||||  
|Controlla Driver IPsec||Sì, sì|Sì, sì|  
|Controlla altri eventi di sistema|Sì, sì|||  
|Controlla modifica stato sicurezza|No Sì|Sì, sì|Sì, sì|  
|Controlla estensione sistema di sicurezza||Sì, sì|Sì, sì|  
|Controlla integrità sistema|Sì, sì|Sì, sì|Sì, sì|  
|**Accesso agli oggetti globale di controllo**||||  
|Controlla Driver IPsec||||  
|Controlla altri eventi di sistema||||  
|Controlla modifica stato sicurezza||||  
|Controlla estensione sistema di sicurezza||||  
|Controlla integrità sistema||||  

**Consigli per le impostazioni controllo di Windows Server 2008, Windows Server 2008 R2 e Windows Server 2012**  

|Criteri di controllo, categoria o sottocategoria|Impostazioni predefinite di Windows<br /><br />Errore di operazione riuscita|Indicazione di base<br /><br />Errore di operazione riuscita|Raccomandazione più forte<br /><br />Errore di operazione riuscita|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Accesso account**||||  
|Controlla convalida credenziali|No, no|Sì, sì|Sì, sì|  
|Controllo servizio di autenticazione Kerberos|||Sì, sì|  
|Controlla operazioni Ticket di servizio Kerberos|||Sì, sì|  
|Controlla altri eventi accesso Account|||Sì, sì|  
|**Gestione degli account**||||  
|Gestione di gruppo di applicazioni di controllo||||  
|Controlla gestione degli Account Computer||Sì controller di dominio|Sì, sì|  
|Gestione di gruppo di controllo distribuzione||||  
|Controlla altri eventi di gestione di Account||Sì, sì|Sì, sì|  
|Gestione di gruppo di sicurezza di controllo||Sì, sì|Sì, sì|  
|Controlla gestione degli Account utente|No Sì|Sì, sì|Sì, sì|  
|**Analisi dettagliata**||||  
|Controlla attività DPAPI|||Sì, sì|  
|Creazione del processo di controllo||No Sì|Sì, sì|  
|Chiusura del processo di controllo||||  
|Controlla eventi RPC||||  
|**Accesso al servizio directory**||||  
|Controlla replica dettagliata servizio Directory||||  
|Controlla accesso al servizio Directory||CONTROLLER DI DOMINIO CONTROLLER DI DOMINIO|CONTROLLER DI DOMINIO CONTROLLER DI DOMINIO|  
|Controlla modifiche servizio Directory||CONTROLLER DI DOMINIO CONTROLLER DI DOMINIO|CONTROLLER DI DOMINIO CONTROLLER DI DOMINIO|  
|Controlla replica servizio Directory||||  
|**Accesso e disconnessione**||||  
|Controlla blocco Account|No Sì||No Sì|  
|Attestazioni utente o dispositivo di controllo||||  
|Controlla modalità estesa IPsec||||  
|Modalità principale IPsec di controllo|||SE SE|  
|Controlla modalità rapida IPsec||||  
|Controlla fine sessione|No Sì|No Sì|No Sì|  
|Controllo dell'accesso|No Sì|Sì, sì|Sì, sì|  
|Server dei criteri di controllo rete|Sì, sì|||  
|Controlla altri eventi di accesso/fine sessione|||Sì, sì|  
|Controlla accesso speciale|No Sì|No Sì|Sì, sì|  
|**Accesso agli oggetti**||||  
|Controlla generato dall'applicazione||||  
|Controlla servizi di certificazione||||  
|Controlla condivisione File dettagliata||||  
|Controlla condivisione File||||  
|Controlla File System||||  
|Connessione a piattaforma filtro di controllo||||  
|Controlla mancata elaborazione pacchetti Piattaforma filtro||||  
|Controlla modifica Handle||||  
|Controlla oggetto Kernel||||  
|Controlla altri eventi di accesso di oggetto||||  
|Controlla Registro di sistema||||  
|Controllo archivi rimovibili||||  
|Controlla SAM||||  
|Controlla gestione temporanea criteri di accesso centrale||||  
|**Modifica dei criteri**||||  
|Controlla Modifica criteri di controllo|No Sì|Sì, sì|Sì, sì|  
|Controlla Modifica criteri di autenticazione|No Sì|No Sì|Sì, sì|  
|Controlla Modifica criteri di autorizzazione||||  
|Controlla Modifica criteri piattaforma filtro||||  
|Controlla Modifica criteri a livello di regola MPSSVC|||Sì  |  
|Controlla altri eventi di modifica dei criteri||||  
|**Uso dei privilegi**||||  
|Controlla utilizzo privilegi Non sensibili||||  
|Controlla altri eventi di utilizzo privilegi||||  
|Controlla utilizzo privilegi sensibili||||  
|**Sistema**||||  
|Controlla Driver IPsec||Sì, sì|Sì, sì|  
|Controlla altri eventi di sistema|Sì, sì|||  
|Controlla modifica stato sicurezza|No Sì|Sì, sì|Sì, sì|  
|Controlla estensione sistema di sicurezza||Sì, sì|Sì, sì|  
|Controlla integrità sistema|Sì, sì|Sì, sì|Sì, sì|  
|**Accesso agli oggetti globale di controllo**||||  
|Controlla Driver IPsec||||  
|Controlla altri eventi di sistema||||  
|Controlla modifica stato sicurezza||||  
|Controlla estensione sistema di sicurezza||||  
|Controlla integrità sistema||||  

## <a name="set-audit-policy-on-workstations-and-servers"></a>Impostare criteri di controllo sulle workstation e server  
Tutti i piani di gestione del registro eventi è necessario monitorare le workstation e server. Un errore comune consiste nel monitorare solo i controller di dominio o server. Poiché violazioni dannoso spesso inizialmente si verifica nelle workstation, workstation di monitoraggio non ignora la fonte migliore e più recente di informazioni.  

Gli amministratori devono scrupolosa esaminare e testare qualsiasi criterio di controllo prima dell'implementazione nel proprio ambiente di produzione.  

## <a name="events-to-monitor"></a>Eventi da monitorare  
Un ID evento perfetta per generare un avviso di sicurezza deve contenere gli attributi seguenti:  

-   Molto probabile che occorrenza indica attività non autorizzata  

-   Numero ridotto di falsi positivi  

-   Occorrenza dovrebbe tradursi in una risposta investigativa/forense  

Due tipi di eventi devono essere monitorati e ricevere un avviso:  

1.  Questi eventi in cui anche una singola occorrenza indica attività non autorizzata  

2.  Accumularsi di eventi oltre una baseline prevista e accettata  

Un esempio del primo evento è:  

Se Domain Admins (DAs) non è consentito di accedere al computer che non sono controller di dominio, una singola occorrenza di un membro dall'accesso a una workstation utente finale deve essere generato un avviso e richiede ulteriori ricerche. Questo tipo di avviso è facile generare l'evento 4964 Controlla accesso speciale (gruppi speciali sono stati assegnati a un nuovo accesso). Altri esempi di avvisi a istanza singola:  

-   Se il Server A mai deve connettersi al Server B, avviso, quando si connettono a vicenda.  

-   Avvisa se un account gestito dall'utente normale in modo imprevisto viene aggiunto a un gruppo di sicurezza sensibili.  

-   Se i dipendenti in un percorso di factory mai lavorano notte, avviso quando un utente accede a mezzanotte.  

-   Avvisa se un servizio non autorizzato è installato su un controller di dominio.  

-   Verificare se un utente standard tenta di accedere direttamente a un Server SQL per cui non dispongono alcun motivo per questa operazione Cancella.  

-   Se non si dispone di alcun membro del gruppo DA, e qualcuno aggiunge stessi sono, controllare subito.  

È un esempio del secondo evento:  

Un numero pericolosi di accessi non riusciti può indicare un'attacco di individuazione delle password. Per un'organizzazione fornire un avviso per un numero insolitamente elevato di accessi non riusciti, è innanzitutto necessario comprendere i normali livelli di accessi non riusciti nel proprio ambiente prima di un evento di sicurezza dannoso.  

Per un elenco completo degli eventi che è necessario includere quando esegue il monitoraggio dei segnali di compromissione, vedere [appendice l: eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md).  

## <a name="active-directory-objects-and-attributes-to-monitor"></a>Oggetti Active Directory e gli attributi da monitorare  
Di seguito sono l'account, gruppi e gli attributi che è necessario monitorare che consentono di rilevare i tentativi di compromettere l'installazione di servizi di dominio Active Directory.  

-   Sistemi per la disabilitazione o la rimozione del software antivirus e antimalware (automaticamente il riavvio di protezione quando è disattivato manualmente)  

-   Account amministratore per modifiche non autorizzate  

-   Attività che vengono eseguite utilizzando l'account con privilegi (automaticamente Rimuovi account quando le attività sospette vengono completate o assegnate tempo scaduto)  

-   Con privilegi e VIP account di dominio Active Directory. Monitorare le modifiche, in particolare le modifiche agli attributi nella scheda Account (ad esempio, cn, nome, sAMAccountName, userPrincipalName o userAccountControl). Oltre a monitorare gli account, limitare chi può modificare l'account come piccole un set di utenti con privilegi amministrativi possibili.  

Fare riferimento a [appendice l: eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) per un elenco di eventi consigliati da monitoraggio, le classificazioni di criticità e un riepilogo di messaggio di evento.  

-   Gruppo server mediante la classificazione di carichi di lavoro, che consente di identificare rapidamente i server che devono corrispondere al meglio monitorato e più rigorosamente configurato  

-   Imposta le proprietà e l'appartenenza di gruppi di dominio Active Directory seguente: Enterprise Admins (EA), Domain Admins (DA), Administrators (BA) e Schema Admins (SA)  

-   Account con privilegi (ad esempio, l'account amministratore predefinito in Active Directory e nei sistemi di membro) per abilitare gli account  

-   Account di gestione per tutte le scritture di accedere all'account  

-   Predefinito configurazione guidata per configurare le impostazioni del firewall per ridurre la superficie di attacco del server, del Registro di sistema, controllo e del servizio. Utilizzare questa procedura guidata, se si implementa jump server come parte della strategia di host amministrativi.  

## <a name="additional-information-for-monitoring-active-directory-domain-services"></a>Informazioni aggiuntive per il monitoraggio dei servizi di dominio Active Directory  
Esaminare i seguenti collegamenti per ulteriori informazioni sul monitoraggio di dominio Active Directory:  
  
-   [Controllo dell'accesso oggetto globale è magia](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -fornisce informazioni sulla configurazione e l'utilizzo di configurazione avanzata di controllo dei criteri che è stato aggiunto a Windows 7 e Windows Server 2008 R2.  

-   [Introduzione di controllo delle modifiche in Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -introduce il controllo le modifiche apportate in Windows 2008.  

-   [Controllo trucchi in Vista e 2008 interessante](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -spiega nuove e interessanti funzionalità di controllo in Windows Vista e Windows Server 2008 che può essere utilizzato per la risoluzione dei problemi o vedere ciò che accade nel proprio ambiente.  

-   [Una posizione centralizzata per il controllo in Windows Server 2008 e Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contiene un elenco di controllo delle funzionalità e le informazioni contenute in Windows Server 2008 e Windows Vista.  

-   [Guida dettagliata di controllo di Active Directory](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -descrive la nuova funzionalità di controllo servizi di dominio Active Directory (AD DS) in Windows Server 2008. Fornisce inoltre le procedure per implementare questa nuova funzionalità.  

## <a name="general-list-of-security-event-id-recommendation-criticalities"></a>Elenco generale di sicurezza evento ID Consiglio Criticalities  
Tutti i suggerimenti di ID evento sono accompagnati da una criticità classificazione come indicato di seguito:  

**Alto:** ID evento con un livello di importanza elevata deve sempre essere immediatamente essere generato un avviso e ricercare.  

**Medio:** ID evento con un livello di criticità di medie dimensioni possono indicare attività dannose, ma deve essere accompagnata da alcuni altri anomalia (ad esempio, un numero insolito che si verificano in un determinato periodo di tempo, le occorrenze impreviste o le occorrenze in un computer in cui è in genere non dovrebbe registrare l'evento.). Può anche un evento medio criticità r raccolti come una metrica e confrontati nel tempo.  

**Basso:** e ID evento con un eventi criticità bassa non dovrebbe acquisire l'attenzione o generano avvisi, a meno che non correlato con gli eventi di criticità medio o alto.  

Questi suggerimenti sono concepiti per fornire una Guida di base per l'amministratore. Tutte le indicazioni devono essere controllate attentamente prima dell'implementazione in un ambiente di produzione.  

Fare riferimento a [appendice l: eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) per un elenco degli eventi da monitorare, le classificazioni di criticità e un riepilogo di messaggio di evento consigliati.  
