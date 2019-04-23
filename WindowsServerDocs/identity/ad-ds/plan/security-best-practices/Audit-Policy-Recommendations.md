---
ms.assetid: 0abe0976-4b49-45d6-a7b3-81d28bdb8210
title: Suggerimenti per i criteri di controllo
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 343a9a7aedf22e9c021249f00fb628f871a2ce1f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835752"
---
# <a name="audit-policy-recommendations"></a>Suggerimenti per i criteri di controllo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1, Windows 7

Questa sezione vengono illustrati le impostazioni di controllo predefinito Windows, di base consigliati delle impostazioni di criteri di controllo e i consigli più aggressivi da Microsoft, per i prodotti server e workstation.  

Le indicazioni di base di Gestione controllo servizi riportate di seguito, con le impostazioni consigliate per semplificare il rilevamento di compromissione, servono solo a essere una Guida iniziale della linea di base per gli amministratori. Ogni organizzazione deve proprio decisioni riguardanti le minacce da affrontare, la tolleranza di rischio accettabile e quali categorie di criteri di controllo o le sottocategorie consentono. Per altre informazioni sulle minacce, consultare il [Guida alle minacce e contromisure](https://technet.microsoft.com/library/hh125921(v=ws.10).aspx). Gli amministratori senza un criterio di controllo attente posto sono invitati a iniziare con le impostazioni consigliate in questo caso e quindi modificare e testare, prima dell'implementazione nell'ambiente di produzione.  

Le raccomandazioni sono per i computer di livello aziendale, che Microsoft definisce come i computer che hanno requisiti di sicurezza medio e richiedono un elevato livello di funzionalità operativa. Criteri di controllo entità che richiedono una maggiore sicurezza requisiti devono prendere in considerazione più aggressivi.  

> [!NOTE]  
> Microsoft Windows per impostazione predefinita e indicazioni di base sono stati ricavati dal [dello strumento Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  

Le impostazioni di criteri di controllo della linea di base seguenti sono consigliate per i computer normale sicurezza che non sono noti per essere sotto attacco attivo, ha esito positivo da antagonisti o malware.  

## <a name="recommended-audit-policies-by-operating-system"></a>I criteri di controllo dal sistema operativo consigliato  
In questa sezione contiene le tabelle che elencano le impostazioni consigliate di controllo che si applicano ai sistemi operativi seguenti:  

-   Windows Server 2016 

-   Windows Server 2012  

-   Windows Server 2012 R2  

-   Windows Server 2008  

-   Windows 10

-   Windows 8.1  

-   Windows 7  

Queste tabelle contengono l'impostazione predefinita di Windows, le indicazioni di base e i consigli più avanzati per questi sistemi operativi.  

**Controllo criteri tabelle legenda**  

|||  
|-|-|  
|**Notazione**|**Raccomandazione**|  
|SÌ|In genere consentono scenari|  
|NO|Effettuare **non** consentono in genere scenari|  
|IF|Attivare se è necessario per uno scenario specifico, o se un ruolo o funzionalità per cui il controllo è richiesto è installato nel computer|  
|DC|Abilita nei controller di dominio|  
|[Vuoto]|Nessuna raccomandazione|  

**Windows 10, Windows 8 e consigli sulle impostazioni di Windows 7 di controllo**  

**Criteri di controllo**  

|Categoria di criteri di controllo o una sottocategoria|Windows Default<br /><br />Errore di operazione riuscita|Indicazione della linea di base<br /><br />Errore di operazione riuscita|Raccomandazione più avanzati<br /><br />Errore di operazione riuscita|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Accesso all'account**||||  
|Controlla Convalida delle credenziali|No    No|Sì No|Sì, sì|  
|Controlla Servizio di autenticazione Kerberos|||Sì, sì|  
|Controlla Operazioni ticket di servizio Kerberos|||Sì, sì|  
|Controlla Altri eventi di accesso account|||Sì, sì|  
|**Gestione degli account**||||  
|Controlla Gestione gruppi di applicazioni||||  
|Controlla Gestione account computer||Sì No|Sì, sì|  
|Controlla Gestione gruppi di distribuzione||||  
|Controllo Altri eventi di gestione account||Sì No|Sì, sì|  
|Controllo Gestione gruppi di sicurezza||Sì No|Sì, sì|  
|Controllo della Gestione account utente|Sì No|Sì No|Sì, sì|  
|**Analisi dettagliata**||||  
|Controlla Attività DPAPI|||Sì, sì|  
|Controlla Creazione di processi||Sì No|Sì, sì|  
|Controlla Chiusura di processi||||  
|Controlla Eventi RPC||||  
|**Accesso al servizio directory**||||  
|Controlla Replica dettagliata servizio directory||||  
|Controlla Accesso al servizio directory||||  
|Controlla Modifiche servizio directory||||  
|Controlla Replica servizio directory||||  
|**Accesso e disconnessione**||||  
|Controlla Blocco account|Sì No||Sì No|  
|Controlla attestazioni utente/dispositivo||||  
|Controlla Modalità estesa IPsec||||  
|Controlla Modalità principale IPsec|||IF     IF|  
|Controlla Modalità rapida IPsec||||  
|Controlla Fine sessione|Sì No|Sì No|Sì No|  
|Controllare l'accesso <sup>1</sup>|Sì, sì|Sì, sì|Sì, sì|  
|Controlla Server dei criteri di rete|Sì, sì|||  
|Controlla Altri eventi di accesso/fine sessione||||  
|Controlla Accesso speciale|Sì No|Sì No|Sì, sì|  
|**Accesso agli oggetti**||||  
|Controlla Generato dall'applicazione||||  
|Controlla Servizi di certificazione||||  
|Controlla condivisione file dettagliata||||  
|Controlla Condivisione file||||  
|Controlla File System||||  
|Controlla Connessione a Piattaforma filtro Windows||||  
|Controlla Mancata elaborazione pacchetti Piattaforma filtro Windows||||  
|Controlla Modifica handle||||  
|Controlla Oggetto kernel||||  
|Controlla Altri eventi di accesso agli oggetti||||  
|Controlla Registro di sistema||||  
|Controlla Archivio rimovibile||||  
|Controlla SAM||||  
|Controlla Gestione temporanea Criteri di accesso centrale||||  
|**Modifica dei criteri**||||  
|Controlla Controlla modifica ai criteri|Sì No|Sì, sì|Sì, sì|  
|Controlla Modifica criteri di autenticazione|Sì No|Sì No|Sì, sì|  
|Controlla Modifica criteri di autorizzazione||||  
|Controlla Modifica criteri Piattaforma filtro Windows||||  
|Controlla Modifica criteri a livello di regola MPSSVC|||Yes  |  
|Controlla Altri eventi di modifica criteri||||  
|**Uso dei privilegi**||||  
|Controlla Utilizzo privilegi non sensibili||||  
|Controlla Altri eventi di utilizzo dei privilegi||||  
|Controlla Utilizzo privilegi sensibili||||  
|**System**||||  
|Controlla Driver IPSec||Sì, sì|Sì, sì|  
|Controlla Altri eventi di sistema|Sì, sì|||  
|Controlla Modifica stato sicurezza|Sì No|Sì, sì|Sì, sì|  
|Controlla Estensione sistema di sicurezza||Sì, sì|Sì, sì|  
|Controlla Integrità sistema|Sì, sì|Sì, sì|Sì, sì|  
|**Accesso agli oggetti globale di controllo**||||  
|Controlla Driver IPSec||||  
|Controlla Altri eventi di sistema||||  
|Controlla Modifica stato sicurezza||||  
|Controlla Estensione sistema di sicurezza||||  
|Controlla Integrità sistema||||  

<sup>1</sup> a partire da Windows 10 versione 1809, controllo dell'accesso è abilitata per impostazione predefinita per entrambi esito positivo e negativo. Nelle versioni precedenti di Windows, solo operazioni riuscite sono abilitata per impostazione predefinita.

**Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e consigli sulle impostazioni di controllo di Windows Server 2008**  

|Categoria di criteri di controllo o una sottocategoria|Windows Default<br /><br />Errore di operazione riuscita|Indicazione della linea di base<br /><br />Errore di operazione riuscita|Raccomandazione più avanzati<br /><br />Errore di operazione riuscita|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Accesso all'account**||||  
|Controlla Convalida delle credenziali|No    No|Sì, sì|Sì, sì|  
|Controlla Servizio di autenticazione Kerberos|||Sì, sì|  
|Controlla Operazioni ticket di servizio Kerberos|||Sì, sì|  
|Controlla Altri eventi di accesso account|||Sì, sì|  
|**Gestione degli account**||||  
|Controlla Gestione gruppi di applicazioni||||  
|Controlla Gestione account computer||Sì controller di dominio|Sì, sì|  
|Controlla Gestione gruppi di distribuzione||||  
|Controllo Altri eventi di gestione account||Sì, sì|Sì, sì|  
|Controllo Gestione gruppi di sicurezza||Sì, sì|Sì, sì|  
|Controllo della Gestione account utente|Sì No|Sì, sì|Sì, sì|  
|**Analisi dettagliata**||||  
|Controlla Attività DPAPI|||Sì, sì|  
|Controlla Creazione di processi||Sì No|Sì, sì|  
|Controlla Chiusura di processi||||  
|Controlla Eventi RPC||||  
|**Accesso al servizio directory**||||  
|Controlla Replica dettagliata servizio directory||||  
|Controlla Accesso al servizio directory||DC    DC|DC    DC|  
|Controlla Modifiche servizio directory||DC    DC|DC    DC|  
|Controlla Replica servizio directory||||  
|**Accesso e disconnessione**||||  
|Controlla Blocco account|Sì No||Sì No|  
|Controlla attestazioni utente/dispositivo||||  
|Controlla Modalità estesa IPsec||||  
|Controlla Modalità principale IPsec|||IF     IF|  
|Controlla Modalità rapida IPsec||||  
|Controlla Fine sessione|Sì No|Sì No|Sì No|  
|Controlla Accesso|Sì, sì|Sì, sì|Sì, sì|  
|Controlla Server dei criteri di rete|Sì, sì|||  
|Controlla Altri eventi di accesso/fine sessione|||Sì, sì|  
|Controlla Accesso speciale|Sì No|Sì No|Sì, sì|  
|**Accesso agli oggetti**||||  
|Controlla Generato dall'applicazione||||  
|Controlla Servizi di certificazione||||  
|Controlla condivisione file dettagliata||||  
|Controlla Condivisione file||||  
|Controlla File System||||  
|Controlla Connessione a Piattaforma filtro Windows||||  
|Controlla Mancata elaborazione pacchetti Piattaforma filtro Windows||||  
|Controlla Modifica handle||||  
|Controlla Oggetto kernel||||  
|Controlla Altri eventi di accesso agli oggetti||||  
|Controlla Registro di sistema||||  
|Controlla Archivio rimovibile||||  
|Controlla SAM||||  
|Controlla Gestione temporanea Criteri di accesso centrale||||  
|**Modifica dei criteri**||||  
|Controlla Controlla modifica ai criteri|Sì No|Sì, sì|Sì, sì|  
|Controlla Modifica criteri di autenticazione|Sì No|Sì No|Sì, sì|  
|Controlla Modifica criteri di autorizzazione||||  
|Controlla Modifica criteri Piattaforma filtro Windows||||  
|Controlla Modifica criteri a livello di regola MPSSVC|||Yes  |  
|Controlla Altri eventi di modifica criteri||||  
|**Uso dei privilegi**||||  
|Controlla Utilizzo privilegi non sensibili||||  
|Controlla Altri eventi di utilizzo dei privilegi||||  
|Controlla Utilizzo privilegi sensibili||||  
|**System**||||  
|Controlla Driver IPSec||Sì, sì|Sì, sì|  
|Controlla Altri eventi di sistema|Sì, sì|||  
|Controlla Modifica stato sicurezza|Sì No|Sì, sì|Sì, sì|  
|Controlla Estensione sistema di sicurezza||Sì, sì|Sì, sì|  
|Controlla Integrità sistema|Sì, sì|Sì, sì|Sì, sì|  
|**Accesso agli oggetti globale di controllo**||||  
|Controlla Driver IPSec||||  
|Controlla Altri eventi di sistema||||  
|Controlla Modifica stato sicurezza||||  
|Controlla Estensione sistema di sicurezza||||  
|Controlla Integrità sistema||||  

## <a name="set-audit-policy-on-workstations-and-servers"></a>Impostare i criteri di controllo sulle workstation e server  
Tutti i piani di gestione di log eventi devono monitorare le workstation e server. Un errore comune è di monitorare solo i controller di dominio o server. Poiché spesso inizialmente pirateria informatica dannosi si verifica nelle workstation, non esegue il monitoraggio workstation ignora la fonte migliore e più vecchia di informazioni.  

Gli amministratori devono scrupoloso esaminare e testare qualsiasi criterio di controllo prima dell'implementazione nell'ambiente di produzione.  

## <a name="events-to-monitor"></a>Eventi da monitorare  
Un ID evento perfetto per generare un avviso di sicurezza deve contenere gli attributi seguenti:  

-   È probabile che tale occorrenza indica attività non autorizzate  

-   Numero ridotto di falsi positivi  

-   Occorrenza dovrebbe restituire una risposta investigativa/forense  

Due tipi di eventi devono essere monitorati e ricevere un avviso:  

1.  Tali eventi in cui anche una singola occorrenza indica attività non autorizzate  

2.  Accumularsi di eventi oltre una baseline prevista e accettata  

È un esempio del primo evento:  

Se gli amministratori di dominio (DAs) non è consentiti di accedere al computer che non sono controller di dominio, una singola occorrenza di un membro dall'accesso a una workstation dell'utente finale deve generare un avviso e analizzare. Questo tipo di avviso è facile da generare utilizzando l'evento Controlla accesso speciale 4964 (gruppi speciali sono stati assegnati a un nuovo account di accesso). Altri esempi di avvisi di istanza singola:  

-   Se un Server non si connetterà mai al Server B, l'avviso quando si connettono tra loro.  

-   Genera un avviso se un account normale per l'utente finale viene aggiunto in modo imprevisto a un gruppo di sicurezza sensibili.  

-   Se i dipendenti nella posizione della factory oggetto non funzionano mai durante la notte, avviso quando un utente accede a mezzanotte.  

-   Genera un avviso se un servizio non autorizzato viene installato in un controller di dominio.  

-   Verificare se un utente finale regolare tenta di accedere direttamente a un Server SQL per cui non dispongono di alcun motivo chiaro per questa operazione.  

-   Se non presentano membri del gruppo DA, e un utente aggiunge stessi non esiste, estrarlo immediatamente.  

È un esempio del secondo evento:  

Un numero di accessi non riusciti pericolosi potrebbe indicare un'attacco di individuazione delle password. Per un'azienda fornire un avviso per un numero insolitamente elevato di accessi non riusciti, è innanzitutto necessario comprendere i normali livelli di accessi non riusciti nell'ambiente prima di un evento di sicurezza malintenzionato.  

Per un elenco completo degli eventi che è necessario includere quando esegue il monitoraggio dei segnali di compromissione, vedere [appendice l: Eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md).  

## <a name="active-directory-objects-and-attributes-to-monitor"></a>Oggetti Active Directory e gli attributi di monitoraggio  
Di seguito è gli account, gruppi e gli attributi che è necessario monitorare per semplificare il rilevamento di tentativi di violazione l'installazione di Active Directory Domain Services.  

-   Sistemi per la disabilitazione o la rimozione del software antivirus e antimalware (automaticamente protezione riavvio quando viene disabilitata manualmente)  

-   Account di amministratore per le modifiche non autorizzate  

-   Attività eseguite con account con privilegi (automaticamente account di rimozione quando le attività sospette sono state completate o assegnate tempo scaduto)  

-   Con privilegi e VIP account in Active Directory Domain Services. Monitorare eventuali modifiche, in particolare le modifiche agli attributi nella scheda Account (ad esempio, cn, nome, sAMAccountName, userPrincipalName o userAccountControl). Oltre al monitoraggio gli account, limitare chi può modificare l'account come piccole un set di utenti con privilegi amministrativi possibili.  

Fare riferimento a [appendice l: Eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) per un elenco degli eventi consigliati per monitoraggio, le valutazioni di criticità e un riepilogo di messaggio di evento.  

-   Gruppo server per la classificazione di carichi di lavoro, che consente di identificare rapidamente i server che devono essere il più strettamente monitorati e configurati più rigoroso  

-   Modifiche alle proprietà e l'appartenenza dei seguenti gruppi di Active Directory Domain Services: Enterprise Admins (EA), Domain Admins (DA), gli amministratori (BA) e Schema Admins (SA)  

-   Disabilitato gli account con privilegi (ad esempio, l'account amministratore predefiniti in Active Directory e nei sistemi di membro) per abilitare gli account  

-   Account di gestione per registrare tutte le operazioni di scrittura all'account  

-   Predefinito configurazione guidata sicurezza per configurare le impostazioni del firewall per ridurre la superficie di attacco del server, del Registro di sistema, controllo e servizio. Utilizzare questa procedura guidata se si implementa jump server come parte della strategia di host amministrativi.  

## <a name="additional-information-for-monitoring-active-directory-domain-services"></a>Informazioni aggiuntive per il monitoraggio di Active Directory Domain Services  
Esaminare i collegamenti seguenti per altre informazioni sul monitoraggio di Active Directory Domain Services:  
  
-   [Controllo dell'accesso agli oggetti globale è Magic](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -fornisce informazioni su configurazione e uso di controllo configurazione avanzata dei criteri che è stato aggiunto a Windows 7 e Windows Server 2008 R2.  

-   [Introduzione a controllo delle modifiche in Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -introduce il controllo le modifiche apportate in Windows 2008.  

-   [Ad accesso sporadico suggerimenti per il controllo in Vista e 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -spiega nuove e interessanti funzionalità di controllo in Windows Vista e Windows Server 2008 che può essere utilizzato per la risoluzione dei problemi o per visualizzare ciò che accade nell'ambiente in uso.  

-   [Posizione centralizzata per il controllo in Windows Server 2008 e Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contiene una raccolta di controllo, le funzionalità e le informazioni contenute in Windows Server 2008 e Windows Vista.  

-   [Guida dettagliata il controllo di AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -descrive la nuova funzionalità di controllo Active Directory Domain Services (AD DS) in Windows Server 2008. Fornisce inoltre le procedure per implementare questa nuova funzionalità.  

## <a name="general-list-of-security-event-id-recommendation-criticalities"></a>Elenco generale di sicurezza evento ID raccomandazione criticità  
Tutte le raccomandazioni di ID evento sono accompagnate da una criticità rating come indicato di seguito:  

**Elevata:** ID evento con un livello di importanza elevata deve sempre e immediatamente da un avviso e analizzato.  

**Media:** ID evento con una classificazione Media criticità potrebbe indicare attività dannose, ma devono essere accompagnato da alcune altre anomalia (ad esempio, un numero insolito che si verificano in un determinato periodo di tempo, le occorrenze impreviste o le occorrenze in un computer che in genere non dovrebbe registrare l'evento.). Potrebbe inoltre un evento di criticità di supporto di r da raccolte come metrica e confrontare nel corso del tempo.  

**Bassa:** E l'ID evento con eventi di importanza bassa non dovrebbe smaliziati attenzione o generano avvisi, a meno che non riconducibili a eventi di criticità medio o alto.  

Queste indicazioni sono concepite per fornire una Guida di base per l'amministratore. Prima dell'implementazione in un ambiente di produzione è necessario esaminare attentamente tutte le raccomandazioni.  

Fare riferimento a [appendice l: Eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) per un elenco degli eventi consigliati per monitoraggio, le valutazioni di criticità e un riepilogo di messaggio di evento.  
