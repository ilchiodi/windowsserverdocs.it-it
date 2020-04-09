---
ms.assetid: 0abe0976-4b49-45d6-a7b3-81d28bdb8210
title: Suggerimenti per i criteri di controllo
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c25df61246bf6a7c731e08e11cee54fd87d6ae4c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821344"
---
# <a name="audit-policy-recommendations"></a>Suggerimenti per i criteri di controllo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1, Windows 7

Questa sezione illustra le impostazioni dei criteri di controllo predefiniti di Windows, le impostazioni dei criteri di controllo di base consigliate e le raccomandazioni più aggressive da Microsoft per i prodotti workstation e server.  

Le raccomandazioni di base SCM illustrate qui, insieme alle impostazioni consigliate per il rilevamento della compromissione, sono destinate solo a essere una guida di base iniziale per gli amministratori. Ogni organizzazione deve prendere le proprie decisioni in merito alle minacce che affrontano, alle relative tolleranze di rischio accettabili e alle categorie o sottocategorie di criteri di controllo da abilitare. Per ulteriori informazioni sulle minacce, vedere la [Guida alle minacce e alle contromisure](https://technet.microsoft.com/library/hh125921(v=ws.10).aspx). Gli amministratori che non dispongono di criteri di controllo riflessivi sono invitati a iniziare con le impostazioni consigliate in questo articolo e quindi per modificarle e testarle prima di implementare nel proprio ambiente di produzione.  

Le raccomandazioni sono destinate a computer di livello aziendale, che Microsoft definisce come computer con requisiti di sicurezza medi e che richiedono un elevato livello di funzionalità operative. Le entità che richiedono requisiti di sicurezza più elevati devono considerare criteri di controllo più aggressivi.  

> [!NOTE]  
> Le indicazioni predefinite e di base di Microsoft Windows sono state ricavate dallo [strumento Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  

Le impostazioni dei criteri di controllo di base seguenti sono consigliate per i computer di sicurezza normali che non sono noti come attacchi attivi e con successo da parte di avversari determinati o malware.  

## <a name="recommended-audit-policies-by-operating-system"></a>Criteri di controllo consigliati per sistema operativo  
In questa sezione sono contenute le tabelle in cui sono elencate le indicazioni sulle impostazioni di controllo applicabili ai sistemi operativi seguenti:  

-   Windows Server 2016 

-   Windows Server 2012  

-   Windows Server 2012 R2  

-   Windows Server 2008  

-   Windows 10

-   Windows 8.1  

-   Windows 7  

Queste tabelle contengono l'impostazione predefinita di Windows, le raccomandazioni di base e le raccomandazioni più solide per questi sistemi operativi.  

**Legenda tabelle dei criteri di controllo**  

|||  
|-|-|  
|**Notazione**|**Consiglio**|  
|SÌ|Abilita in scenari generali|  
|NO|**Non** abilitare in scenari generali|  
|IF|Abilitare se necessario per uno scenario specifico o se un ruolo o una funzionalità per cui si desidera il controllo è installato nel computer|  
|DC|Abilita nei controller di dominio|  
|Vuoto|Nessuna raccomandazione|  

**Consigli sulle impostazioni di controllo di Windows 10, Windows 8 e Windows 7**  

**Criteri di controllo**  

|Categoria o sottocategoria dei criteri di controllo|Impostazione predefinita di Windows<p>Esito negativo|Raccomandazione Baseline<p>Esito negativo|Raccomandazione più avanzata<p>Esito negativo|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Accesso all'account**||||  
|Controlla Convalida delle credenziali|No, no|Sì No|Sì, sì|  
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
|**Rilevamento dettagliato**||||  
|Controlla Attività DPAPI|||Sì, sì|  
|Controlla Creazione di processi||Sì No|Sì, sì|  
|Controlla Chiusura di processi||||  
|Controlla Eventi RPC||||  
|**Accesso DS**||||  
|Controlla Replica dettagliata servizio directory||||  
|Controlla Accesso al servizio directory||||  
|Controlla Modifiche servizio directory||||  
|Controlla Replica servizio directory||||  
|**Accesso e disconnessione**||||  
|Controlla Blocco account|Sì No||Sì No|  
|Controlla attestazioni utente/dispositivo||||  
|Controlla Modalità estesa IPsec||||  
|Controlla Modalità principale IPsec|||Se|  
|Controlla Modalità rapida IPsec||||  
|Controlla Fine sessione|Sì No|Sì No|Sì No|  
|Controlla accesso <sup>1</sup>|Sì, sì|Sì, sì|Sì, sì|  
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
|Controlla Modifica criteri a livello di regola MPSSVC|||Sì  |  
|Controlla Altri eventi di modifica criteri||||  
|**Uso dei privilegi**||||  
|Controlla Utilizzo privilegi non sensibili||||  
|Controlla Altri eventi di utilizzo dei privilegi||||  
|Controlla Utilizzo privilegi sensibili||||  
|**Sistema**||||  
|Controlla Driver IPSec||Sì, sì|Sì, sì|  
|Controlla Altri eventi di sistema|Sì, sì|||  
|Controlla Modifica stato sicurezza|Sì No|Sì, sì|Sì, sì|  
|Controlla Estensione sistema di sicurezza||Sì, sì|Sì, sì|  
|Controlla Integrità sistema|Sì, sì|Sì, sì|Sì, sì|  
|**Controllo di accesso agli oggetti globali**||||  
|Controlla Driver IPSec||||  
|Controlla Altri eventi di sistema||||  
|Controlla Modifica stato sicurezza||||  
|Controlla Estensione sistema di sicurezza||||  
|Controlla Integrità sistema||||  

<sup>1</sup> a partire da Windows 10 versione 1809, l'accesso di controllo è abilitato per impostazione predefinita in caso di esito positivo e negativo. Nelle versioni precedenti di Windows, per impostazione predefinita viene abilitato solo l'esito positivo.

**Indicazioni sulle impostazioni di controllo di Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008**  

|Categoria o sottocategoria dei criteri di controllo|Impostazione predefinita di Windows<p>Esito negativo|Raccomandazione Baseline<p>Esito negativo|Raccomandazione più avanzata<p>Esito negativo|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Accesso all'account**||||  
|Controlla Convalida delle credenziali|No, no|Sì, sì|Sì, sì|  
|Controlla Servizio di autenticazione Kerberos|||Sì, sì|  
|Controlla Operazioni ticket di servizio Kerberos|||Sì, sì|  
|Controlla Altri eventi di accesso account|||Sì, sì|  
|**Gestione degli account**||||  
|Controlla Gestione gruppi di applicazioni||||  
|Controlla Gestione account computer||Sì DC|Sì, sì|  
|Controlla Gestione gruppi di distribuzione||||  
|Controllo Altri eventi di gestione account||Sì, sì|Sì, sì|  
|Controllo Gestione gruppi di sicurezza||Sì, sì|Sì, sì|  
|Controllo della Gestione account utente|Sì No|Sì, sì|Sì, sì|  
|**Rilevamento dettagliato**||||  
|Controlla Attività DPAPI|||Sì, sì|  
|Controlla Creazione di processi||Sì No|Sì, sì|  
|Controlla Chiusura di processi||||  
|Controlla Eventi RPC||||  
|**Accesso DS**||||  
|Controlla Replica dettagliata servizio directory||||  
|Controlla Accesso al servizio directory||DC DC|DC DC|  
|Controlla Modifiche servizio directory||DC DC|DC DC|  
|Controlla Replica servizio directory||||  
|**Accesso e disconnessione**||||  
|Controlla Blocco account|Sì No||Sì No|  
|Controlla attestazioni utente/dispositivo||||  
|Controlla Modalità estesa IPsec||||  
|Controlla Modalità principale IPsec|||Se|  
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
|Controlla Modifica criteri a livello di regola MPSSVC|||Sì  |  
|Controlla Altri eventi di modifica criteri||||  
|**Uso dei privilegi**||||  
|Controlla Utilizzo privilegi non sensibili||||  
|Controlla Altri eventi di utilizzo dei privilegi||||  
|Controlla Utilizzo privilegi sensibili||||  
|**Sistema**||||  
|Controlla Driver IPSec||Sì, sì|Sì, sì|  
|Controlla Altri eventi di sistema|Sì, sì|||  
|Controlla Modifica stato sicurezza|Sì No|Sì, sì|Sì, sì|  
|Controlla Estensione sistema di sicurezza||Sì, sì|Sì, sì|  
|Controlla Integrità sistema|Sì, sì|Sì, sì|Sì, sì|  
|**Controllo di accesso agli oggetti globali**||||  
|Controlla Driver IPSec||||  
|Controlla Altri eventi di sistema||||  
|Controlla Modifica stato sicurezza||||  
|Controlla Estensione sistema di sicurezza||||  
|Controlla Integrità sistema||||  

## <a name="set-audit-policy-on-workstations-and-servers"></a>Impostare i criteri di controllo nelle workstation e nei server  
Tutti i piani di gestione del registro eventi devono monitorare le workstation e i server. Un errore comune consiste nel monitorare solo i server o i controller di dominio. Poiché le intrusioni dannose si verificano spesso nelle workstation, non è possibile che le workstation di monitoraggio ignorino la fonte di informazioni migliore e meno recente.  

Gli amministratori devono esaminare attentamente e testare i criteri di controllo prima dell'implementazione nell'ambiente di produzione.  

## <a name="events-to-monitor"></a>Eventi da monitorare  
Un ID evento perfetto per generare un avviso di sicurezza deve contenere gli attributi seguenti:  

-   Elevata probabilità che l'occorrenza indichi attività non autorizzate  

-   Numero ridotto di falsi positivi  

-   L'occorrenza dovrebbe causare una risposta investigativa/forense  

È necessario monitorare e avvisare due tipi di eventi:  

1.  Gli eventi in cui anche una singola occorrenza indica attività non autorizzata  

2.  Accumularsi di eventi oltre una baseline prevista e accettata  

Un esempio del primo evento è:  

Se non è consentito eseguire l'accesso a computer che non sono controller di dominio, una singola occorrenza di un membro DA che accede a una workstation dell'utente finale deve generare un avviso ed esaminarla. Questo tipo di avviso è facile da generare utilizzando l'evento di accesso speciale di controllo 4964 (i gruppi speciali sono stati assegnati a un nuovo accesso). Altri esempi di avvisi a istanza singola includono:  

-   Se il server A non deve mai connettersi al server B, avvisare quando si connettono tra loro.  

-   Avviso se un normale account utente finale viene aggiunto in modo imprevisto a un gruppo di sicurezza sensibile.  

-   Se i dipendenti nella sede della fabbrica non lavorano mai a notte, avvisare quando un utente accede a mezzanotte.  

-   Avvisa se un servizio non autorizzato viene installato in un controller di dominio.  

-   Verificare se un normale utente finale tenta di accedere direttamente a una SQL Server per cui non hanno motivo chiaro per eseguire questa operazione.  

-   Se non sono presenti membri nel gruppo DA e qualcuno aggiunge se stessi, verificarlo immediatamente.  

Un esempio del secondo evento è:  

Un numero aberrato di accessi non riusciti potrebbe indicare un attacco di individuazione della password. Per consentire a un'azienda di fornire un avviso per un numero insolitamente elevato di accessi non riusciti, è necessario che i livelli normali degli accessi non riusciti siano compresi nell'ambiente precedente a un evento di sicurezza dannoso.  

Per un elenco completo degli eventi che è necessario includere quando si esegue il monitoraggio dei segnali di compromissione, vedere [Appendice L: eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md).  

## <a name="active-directory-objects-and-attributes-to-monitor"></a>Active Directory gli oggetti e gli attributi da monitorare  
Di seguito sono riportati gli account, i gruppi e gli attributi che è necessario monitorare per facilitare il rilevamento dei tentativi di compromissione dell'installazione del Active Directory Domain Services.  

-   Sistemi per la disabilitazione o la rimozione di software antivirus e antimalware (riavvia automaticamente la protezione quando viene disabilitato manualmente)  

-   Account amministratore per le modifiche non autorizzate  

-   Attività eseguite usando account con privilegi (Rimuovi automaticamente l'account quando le attività sospette vengono completate o il tempo assegnato è scaduto)  

-   Account con privilegi e VIP in servizi di dominio Active Directory. Monitorare le modifiche, in particolare le modifiche apportate agli attributi nella scheda account, ad esempio CN, Name, sAMAccountName, userPrincipalName o userAccountControl. Oltre al monitoraggio degli account, limitare gli utenti che possono modificare gli account in un set ridotto di utenti amministrativi come possibile.  

Vedere l' [Appendice L: eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) per un elenco di eventi consigliati da monitorare, le classificazioni critiche e un riepilogo dei messaggi di evento.  

-   Raggruppare i server in base alla classificazione dei propri carichi di lavoro, consentendo di identificare rapidamente i server che devono essere monitorati più accuratamente e configurati in modo più rigoroso  

-   Modifiche alle proprietà e all'appartenenza dei seguenti gruppi di servizi di dominio Active Directory: Enterprise Admins (EA), Domain Admins (DA), amministratori (BA) e Schema Admins (SA)  

-   Account con privilegi disabilitati (ad esempio account Administrator predefiniti in Active Directory e nei sistemi membro) per abilitare gli account  

-   Account di gestione per registrare tutte le Scritture nell'account  

-   Configurazione guidata impostazioni di sicurezza predefinite per configurare le impostazioni di servizio, registro di sistema, controllo e firewall per ridurre la superficie di attacco del server. Utilizzare questa procedura guidata se si implementano Jump server come parte della strategia host di amministrazione.  

## <a name="additional-information-for-monitoring-active-directory-domain-services"></a>Informazioni aggiuntive per il monitoraggio Active Directory Domain Services  
Per ulteriori informazioni sul monitoraggio di servizi di dominio Active Directory, vedere i collegamenti seguenti:  
  
-   Il [controllo dell'accesso agli oggetti globale è](https://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) una funzionalità che fornisce informazioni sulla configurazione e l'uso della configurazione avanzata dei criteri di controllo aggiunti a Windows 7 e windows Server 2008 R2.  

-   [Introduzione alle modifiche di controllo in windows 2008](https://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) : introduce le modifiche di controllo apportate in Windows 2008.  

-   [Trucchi di controllo interessanti in vista e 2008](https://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) : vengono illustrate le nuove funzionalità interessanti del controllo in Windows Vista e windows Server 2008 che possono essere usate per la risoluzione dei problemi o per vedere cosa accade nell'ambiente.  

-   [Un'unica tappa per il controllo in Windows server 2008 e Windows Vista](https://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) : contiene una compilazione di funzionalità di controllo e informazioni contenute in windows Server 2008 e Windows Vista.  

-   [Guida dettagliata al controllo di servizi di dominio Active Directory](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) : descrive la nuova funzionalità di controllo Active Directory Domain Services (ad DS) in Windows Server 2008. Sono inoltre disponibili procedure per l'implementazione di questa nuova funzionalità.  

## <a name="general-list-of-security-event-id-recommendation-criticalities"></a>Elenco generale di criticità delle raccomandazioni ID evento sicurezza  
Tutte le raccomandazioni relative agli ID evento sono corredate da una valutazione della criticità, come indicato di seguito:  

**Elevato:** Gli ID evento con una classificazione di criticità elevata devono sempre e immediatamente essere avvisati e analizzati.  

**Media:** Un ID evento con una classificazione di criticità media potrebbe indicare attività dannose, ma deve essere accompagnata da altre anomalie, ad esempio un numero insolito che si verifica in un determinato periodo di tempo, occorrenze impreviste o occorrenze in un computer che normalmente non dovrebbe registrare l'evento. Un evento di criticità media può anche essere raccolto come metrica e confrontato nel tempo.  

**Bassa:** E l'ID evento con eventi di criticità bassa non devono raccogliere attenzione o causare avvisi, a meno che non siano correlati a eventi di criticità medio o elevato.  

Questi consigli hanno lo scopo di fornire una guida di base per l'amministratore. Prima dell'implementazione in un ambiente di produzione, è necessario esaminare accuratamente tutti i consigli.  

Fare riferimento all' [Appendice L: eventi da monitorare](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) per un elenco degli eventi consigliati da monitorare, le classificazioni critiche e un riepilogo dei messaggi di evento.  
