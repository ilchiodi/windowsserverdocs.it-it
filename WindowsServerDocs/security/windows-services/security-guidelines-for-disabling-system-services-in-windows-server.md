---
title: Linee guida per la protezione per i servizi di sistema in Windows Server 2016
description: Linee guida per la protezione per la disattivazione dei servizi in Windows Server 2016 con esperienza Desktop
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 5/22/2017
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 8f60d5095a3e4cebdffba5d3c02c69bc06326b3a
ms.sourcegitcommit: 68952ac7a29d0e2f07023958ad949fecc1b783b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2018
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>Indicazioni su come disabilitare i servizi di sistema in Windows Server 2016 con esperienza Desktop

Si applica a: Windows Server 2016

Il sistema operativo Windows include molti servizi di sistema che forniscono funzionalità importanti. Servizi diversi prevede dei criteri di avvio predefinito diverso: alcuni vengono avviati per impostazione predefinita (automatico), alcune quando necessario (manuale) e alcuni sono disabilitati per impostazione predefinita e deve essere abilitati in modo esplicito prima dell'esecuzione. Queste impostazioni predefinite sono stati scelti attentamente per ogni servizio per il bilanciamento delle prestazioni, funzionalità e sicurezza per i clienti tipici.

Tuttavia, alcuni clienti aziendali possono scegliere un saldo più incentrate sulla sicurezza per i PC e Windows Server, che riduce al minimo la superficie di attacco e pertanto consigliabile disabilitare completamente tutti i servizi che non sono necessarie nei propri ambienti specifici. Per i clienti, Microsoft® fornisce le istruzioni di accompagnamento relative ai servizi possono essere disabilitati in modo sicuro a questo scopo.

Le linee guida sono per Windows Server 2016 con esperienza Desktop (a meno che per gli utenti finali, utilizzato per sostituire desktop). Ogni servizio nel sistema viene classificata come indicato di seguito:

-   **Disabilitare:** probabilmente preferiscano un'azienda incentrate sulla sicurezza disabilitare il servizio ed evitare la funzionalità (vedere altri dettagli riportati di seguito).
- **OK per Disable:** questo servizio offre funzionalità utili per alcune, ma non tutte le aziende e alle aziende di protezione che non viene utilizzato in modo sicuro possono disabilitarlo.
- **Non disabilitare:** la disattivazione di questo servizio verrà impatto sulle funzionalità essenziale o impedire funzionalità o ruoli specifici di funzioni correttamente. Pertanto non deve essere disattivata.
-  **(Alcuna indicazione):** l'impatto della disattivazione di questi servizi non è stato valutato completamente. Di conseguenza, la configurazione predefinita di questi servizi non va modificata.


I clienti possono configurare i PC Windows e i server per disabilitare i servizi selezionati utilizzando i modelli di protezione nei criteri di gruppo oppure automazione di PowerShell. In alcuni casi, le linee guida includono impostazioni di criteri di gruppo specifiche di disabilitare la funzionalità del servizio direttamente, in alternativa per disabilitare il servizio stesso.

Microsoft consiglia che i clienti disabilitare i seguenti servizi e le rispettive attività pianificate in Windows Server 2016 con esperienza Desktop:

Servizi: 
1. Xbox Live Auth Manager
2. Xbox Live gioco Salva

Attività pianificate: 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(È anche possibile accedere alle informazioni su tutti i servizi descritti in dettaglio in questo articolo visualizzando il foglio di calcolo di Microsoft Excel associata: [indicazioni per la disabilitazione di servizi di sistema in Windows Server 2016 con esperienza Desktop](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))

<br />

### <a name="disabling-services-not-installed-by-default"></a>La disattivazione dei servizi non è installati per impostazione predefinita

Microsoft sconsiglia l'applicazione dei criteri per disabilitare i servizi che non sono installati per impostazione predefinita.
-  Il servizio è in genere necessaria se è installata la funzionalità. L'installazione del servizio o la funzionalità richiede diritti amministrativi. Non consentire l'installazione delle funzionalità, non l'avvio del servizio.
-  Bloccare il servizio di Microsoft Windows non impedisce un amministratore (o in alcuni casi non amministratore) l'installazione di un equivalente di terze parti simile, ad esempio uno con un rischio di sicurezza più elevato.
-  Una linea di base o benchmark che disabilita un servizio di Windows (ad esempio, W3SVC) non predefiniti offrono alcune revisori l'impressione erroneamente che la tecnologia (ad esempio, IIS) sia sicura e non deve mai essere utilizzata.
-  Se non è mai state installate la funzionalità (e servizio), questo aggiunge semplicemente in blocco non necessario per la linea di base e lavoro verifica.

<br />
Per tutti i servizi di sistema elencati in questo documento, le due tabelle che seguono offrono una spiegazione delle colonne e i suggerimenti di Microsoft per l'abilitazione e disabilitazione di servizi di sistema in Windows Server 2016 con esperienza Desktop: 
 
<br />

### <a name="explanation-of-columns"></a>Spiegazione delle colonne

| | |
|---|---|
|**Descrizione del servizio**|   Descrizione del servizio, da qdescription sc.exe.|
|**Nome** |Nome della chiave (interno) del servizio|
|**Installazione** |Sempre installata: Service si trova su Server Core e Server con esperienza Desktop  <br /> Solo nel Datacenter Edition: servizio è in Server 2016 con esperienza Desktop, ma è ***non*** in Server Core |
|**StartType**  |Tipo di avvio in Windows Server 2016|
|**Raccomandazione** |Microsoft Consiglio/consigli sulla disattivazione di questo servizio in Windows Server 2016 in una distribuzione tipica, ben gestita enterprise e in cui il server non viene utilizzato come un sostituto desktop degli utenti finali.|
|**Commenti** |SPIEGAZIONE aggiuntiva|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Spiegazione delle raccomandazioni di Microsoft

| | |
|---|---|
|**Non disabilitare** |Questo servizio non deve essere disabilitato|
|**Per disabilitare OK**| Questo servizio può essere disattivato se non viene utilizzata la funzionalità che è supportata.|
|**Già disabilitato**|  Questo servizio è disabilitato per impostazione predefinita. non è necessario applicare con criteri di|
|**Deve essere disabilitato** |Questo servizio non deve essere abilitato in un sistema ben gestiti a livello aziendale.|

<br />

Le tabelle seguenti offrono indicazioni Microsoft per la disabilitazione di servizi di sistema in Windows Server 2016 con esperienza Desktop:

<br />

##  <a name="activex-installer-axinstsv"></a>ActiveX Installer (AxInstSV)

| | |
|---|---|
|   **Descrizione del servizio** |   Fornisce la convalida di controllo dell'Account utente per l'installazione dei controlli ActiveX da Internet e consente la gestione di installazione di controlli ActiveX in base alle impostazioni di criteri di gruppo. Questo servizio viene avviato su richiesta e se l'installazione dei controlli ActiveX disabilitato si comporterà in base alle impostazioni predefinite del browser.    |
|   **Nome del servizio**    |   AxInstSV    |
|   **Installazione**    |   Solo nel Datacenter Edition  |
|   **StartType**   |   Manuale  |
|   **Raccomandazione**  |   Per disabilitare OK   |
|   **Commenti**    |   OK per disabilitare se funzionalità non necessarie. |


<br />

## <a name="alljoyn-router-service"></a>Servizio Router AllJoyn   
| | |
|---|---|
|   **Descrizione del servizio** |   Indirizza i messaggi AllJoyn per i client AllJoyn locali. Se questo servizio viene arrestato AllJoyn client che non dispone di propri router bundle sarà possibile eseguire. |
|   **Nome del servizio**    |   AJRouter    |
|   **Installazione**    |   Solo nel Datacenter Edition  |
|   **StartType**   |   Manuale  |
|   **Raccomandazione**  | Nessuna Guida       |
|   **Commenti**    |       |
| | |

<br />

## <a name="app-readiness"></a>Preparazione di App
| | |
|---|---|
**Descrizione del servizio** |   Ottiene le app pronta per l'uso la prima volta che un utente accede a questo PC e durante l'aggiunta di nuove app.
**Nome del servizio**    |   AppReadiness all'indirizzo
**Installazione**    |   Solo nel Datacenter Edition
**StartType**   |   Manuale
**Raccomandazione**  |   Non disabilitare
**Commenti**    |   
| | |

<br />

##  <a name="application-identity"></a>Identità applicazione
| | |       
|---|---|   
**Descrizione del servizio** |   Determina e verifica l'identità di un'applicazione. La disattivazione di questo servizio impedirà AppLocker di imposto.
**Nome del servizio**    |   AppIDSvc
**Installazione**    |   Sempre installata
**StartType**   |   Manuale
**Raccomandazione**  |Nessuna Guida    
**Commenti**    |   
|||     

<br />

##  <a name="application-information"></a>Informazioni sull'applicazione 
| | |       
|---|---|   
|   **Descrizione del servizio** |   Facilita l'esecuzione di applicazioni interattive con privilegi amministrativi aggiuntivi.  Se questo servizio viene arrestato, gli utenti in grado di avviare applicazioni con i privilegi amministrativi aggiuntivi che eventualmente necessari per eseguire attività utente desiderato.
|   **Nome del servizio**    |   Appinfo
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   Supporta l'elevazione dei privilegi di controllo dell'account utente stesso desktop
|||     

<br />

##  <a name="application-layer-gateway-service"></a>Servizio Gateway di livello di applicazione       
| | |           
|---|---|           
|   **Descrizione del servizio** |   Fornisce supporto per il protocollo di terze parti plug-in per la condivisione connessione Internet
|   **Nome del servizio**    |   ALG
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |Nessuna Guida    
|   **Commenti**    |   
|||     

<br />

##  <a name="application-management"></a>Gestione delle applicazioni      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Elabora le richieste di enumerazione, installazione e rimozione di software distribuito tramite criteri di gruppo. Se il servizio è disabilitato, gli utenti in grado di installare, rimuovere o enumerare il software distribuito tramite criteri di gruppo. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   AppMgmt
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="appx-deployment-service-appxsvc"></a>Servizi di distribuzione AppX (AppXSVC)       
| | |           
|---|---|
|   **Descrizione del servizio** |   Fornisce un'infrastruttura di supporto per distribuire le applicazioni di archiviazione. Questo servizio viene avviato su richiesta e se le applicazioni di archiviazione disabilitate non verranno distribuite al sistema e potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   AppXSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="auto-time-zone-updater"></a>Strumento di aggiornamento automatico fuso orario           
| | |           
|---|---|           
|   **Descrizione del servizio** |   Imposta automaticamente il fuso orario di sistema.
|   **Nome del servizio**    |   tzautoupdate
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Disabilitato
|   **Raccomandazione**  |   Già disabilitato
|   **Commenti**    |   
|||         
            
<br />          

## <a name="background-intelligent-transfer-service"></a>Servizio trasferimento intelligente in background          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente di trasferire file in background con larghezza di banda inattiva. Se il servizio è disattivato, tutte le applicazioni che dipendono da BITS, ad esempio Windows Update o MSN Explorer, sarà possibile scaricare automaticamente i programmi e altre informazioni.
|   **Nome del servizio**    |   BITS
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          


## <a name="background-tasks-infrastructure-service"></a>Servizio di infrastruttura di attività in background      
| | |           
|---|---|   
|   **Descrizione del servizio** |   Servizio di infrastruttura di Windows che controlla le attività in background può essere eseguito nel sistema.
|   **Nome del servizio**    |   BrokerInfrastructure
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="base-filtering-engine"></a>Motore di filtraggio di base            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il motore di filtri di Base (BFE) è un servizio che gestisce i criteri firewall e Internet Protocol security (IPsec) e implementa il filtro delle modalità utente. L'arresto o la disabilitazione del servizio BFE riduce notevolmente la sicurezza del sistema. Anche comporterà un comportamento imprevedibile in applicazioni di firewall e IPsec gestione.
|   **Nome del servizio**    |   BFE
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="bluetooth-support-service"></a>Servizio di supporto di Bluetooth            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio Bluetooth supporta l'individuazione e associazione dei dispositivi Bluetooth remoti.  L'arresto o la disattivazione di questo servizio potrebbe già installate ai dispositivi Bluetooth di esito negativo per il corretto funzionamento e impedire nuovi dispositivi viene individuato o associati.
|   **Nome del servizio**    |   BthServ
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   OK per disabilitare se non utilizzato. Un altro meccanismo di disattivazione:https://technet.microsoft.com/en-us/library/dd252791.aspx
|||         
            
<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio utente viene utilizzato per scenari di piattaforme di dispositivi connessi
|   **Nome del servizio**    |   CDPUserSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Modello di servizio utente
|||         
            
<br />          


##  <a name="certificate-propagation"></a>Propagazione certificati     
| | |           
|---|---|
|   **Descrizione del servizio** |   Copia i certificati utente e i certificati radice dalla smart card nell'archivio certificati dell'utente corrente, rileva quando una smart card viene inserita in un lettore di smart card e se necessario, installa il minidriver Plug and Play per smart card.
|   **Nome del servizio**    |   CertPropSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="client-license-service-clipsvc"></a>Servizio licenze client (ClipSVC)        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce un'infrastruttura di supporto per Microsoft Store. Questo servizio viene avviato su richiesta e se le applicazioni disabilitate acquistato tramite Microsoft Store non funzioneranno correttamente.
|   **Nome del servizio**    |   ClipSVC
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="cng-key-isolation"></a>Isolamento chiavi CNG
| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio di isolamento chiavi CNG è ospitato nel processo LSA. Il servizio garantisce l'isolamento di processo fondamentale per le chiavi private e di operazioni di crittografia associate come richiesto in base ai criteri comuni. Il servizio archivia e utilizza le chiavi di lunga durate in un processo di protezione conformi ai requisiti Common Criteria.
|   **Nome del servizio**    |   KeyIso
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="com-event-system"></a>Sistema degli eventi COM+       
| | |           
|---|---|       
|   **Descrizione del servizio** |   Supporta System evento servizio di notifica (SENS), che consente la distribuzione automatica degli eventi per la sottoscrizione componenti modello COM (Component Object). Se il servizio viene arrestato, SENS verrà chiusa e non sarà in grado di fornire le notifiche di accesso e disconnessione. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   EventSystem
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="com-system-application"></a>Applicazione di sistema COM+     
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce la configurazione e registrazione dei componenti + basato su modello COM (Component Object). Se il servizio viene arrestato, la maggior parte dei COM+-componenti di base non funzionerà correttamente. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   COMSysApp
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="computer-browser"></a>Browser del computer        
| | |           
|---|---|       
|   **Descrizione del servizio** |   Mantiene un elenco aggiornato dei computer nella rete e fornisce tale elenco ai computer designati come browser. Se questo servizio viene arrestato, questo elenco non verrà aggiornato o mantenuto. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   Browser
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Disabilitato
|   **Raccomandazione**  |   Già disabilitato
|   **Commenti**    |   
|||         
            
<br />          

## <a name="connected-devices-platform-service"></a>Servizio piattaforma i dispositivi connessi       
| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio viene utilizzato per scenari di dispositivi connessi e vetro universale
|   **Nome del servizio**    |   CDPSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="connected-user-experiences-and-telemetry"></a>Esperienze utente connesse e telemetria     
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio servizio esperienze utente connesse e telemetria Abilita le funzionalità che supportano esperienze utente connesse e nell'applicazione. Inoltre, il servizio gestisce la raccolta basato sugli eventi e la trasmissione delle informazioni di diagnostica e di utilizzo (usate per migliorare la qualità della piattaforma Windows e l'esperienza) quando la diagnostica e le impostazioni delle opzioni di utilizzo della privacy sono abilitate in Feedback e diagnostica.
|   **Nome del servizio**    |   DiagTrack
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="contact-data"></a>Dati di contatto        
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gli indici di dati di contatto per la ricerca rapida contatto. Se si arresta o disabilitare il servizio, i contatti potrebbero essere mancanti dai risultati della ricerca.
|   **Nome del servizio**    |   PimIndexMaintenanceSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Modello di servizio utente
|||         
            
<br />          

## <a name="coremessaging"></a>CoreMessaging            
| | |           
|---|---|           
|   **Descrizione del servizio** |   Gestisce la comunicazione tra i componenti di sistema.
|   **Nome del servizio**    |   CoreMessagingRegistrar
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="credential-manager"></a>Gestione credenziali           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce l'archiviazione sicura e il recupero delle credenziali per gli utenti, applicazioni e pacchetti di servizio di protezione.
|   **Nome del servizio**    |   VaultSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="cryptographic-services"></a>Servizi di crittografia           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce tre servizi di gestione: il servizio Database catalogo, che conferma le firme dei file di Windows e permette di nuovi programmi deve essere installato; Radice il servizio protetto, che aggiunge e rimuove i certificati di autorità di certificazione radice attendibili da questo computer. e automatico del servizio di aggiornamento certificato radice, che recupera i certificati radice da Windows Update e abilitare scenari, ad esempio SSL. Se questo servizio viene arrestato, i servizi di gestione non funzionerà correttamente. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   CryptSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="data-sharing-service"></a>Servizio di condivisione dei dati         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce della trasmissione di dati tra le applicazioni.
|   **Nome del servizio**    |   DsSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio DCP (raccolta dei dati e pubblicazione) supporta le App del produttore per caricare dati nel cloud.
|   **Nome del servizio**    |   DcpSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="dcom-server-process-launcher"></a>Avvio di processo Server DCOM         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio DCOMLAUNCH Avvia server COM e DCOM in risposta a un oggetto le richieste di attivazione. Se questo servizio viene arrestato o disabilitato, i programmi con COM o DCOM non funzionerà correttamente. Si consiglia di disporre l'esecuzione del servizio DCOMLAUNCH.
|   **Nome del servizio**    |   DcomLaunch
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  |Nessuna Guida    
|   **Commenti**    |   
|||         
            
<br />

##  <a name="device-association-service"></a>Servizio associazione dispositivi      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Abilita l'associazione tra il sistema e i dispositivi wireless o cablati.
|   **Nome del servizio**    |   DeviceAssociationService
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          
        
##  <a name="device-install-service"></a>Servizio di installazione di dispositivi      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente a un computer a riconoscere e adattarsi alle modifiche hardware con minima o senza input dell'utente. L'arresto o la disattivazione di questo servizio verrà causare instabilità del sistema.
|   **Nome del servizio**    |   DeviceInstall
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="device-management-enrollment-service"></a>Servizio registrazione dispositivi di gestione        
| | |           
|---|---|       
|   **Descrizione del servizio** |   Esegue le attività di iscrizione di dispositivi per la gestione dei dispositivi
|   **Nome del servizio**    |   DmEnrollmentSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="device-setup-manager"></a>Gestione di installazione di dispositivi         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente il rilevamento, download e installazione del software correlati ai dispositivi. Se questo servizio è disattivato, i dispositivi possono essere configurati con software aggiornato e potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   DsmSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="devquery-background-discovery-broker"></a>Sfondo DevQuery individuazione Broker         
| | |           
|---|---|           
|   **Descrizione del servizio** |   Consente alle app di individuare i dispositivi con un'attività in linea
|   **Nome del servizio**    |   DevQueryBroker
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="dhcp-client"></a>Client DHCP          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Registra e aggiorna indirizzi IP e i record DNS per questo computer. Se questo servizio viene arrestato, il computer non riceverà gli indirizzi IP dinamici e gli aggiornamenti DNS. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   DHCP
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="diagnostic-policy-service"></a>Servizio criteri di diagnostica            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio Criteri di diagnostica consente il rilevamento dei problemi e la risoluzione per i componenti di Windows.  Se questo servizio viene arrestato, diagnostica non funzioneranno più.
|   **Nome del servizio**    |   PUNTI DI DISTRIBUZIONE
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="diagnostic-service-host"></a>Host del servizio di diagnostica     
| | |           
|---|---|       
|   **Descrizione del servizio** |   L'Host del servizio di diagnostica viene utilizzato dal servizio Criteri di diagnostica per diagnostica host che devono essere eseguite in un contesto servizio locale.  Se questo servizio viene arrestato, qualsiasi diagnostica che dipende da esso non funzioneranno più.
|   **Nome del servizio**    |   WdiServiceHost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="diagnostic-system-host"></a>Host di diagnostica di sistema           
| | |           
|---|---|       
|   **Descrizione del servizio** |   La diagnostica di sistema Host viene utilizzata dal servizio Criteri di diagnostica per diagnostica host che devono essere eseguite in un contesto di sistema locale.  Se questo servizio viene arrestato, qualsiasi diagnostica che dipende da esso non funzioneranno più.
|   **Nome del servizio**    |   WdiSystemHost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="distributed-link-tracking-client"></a>Collegamenti distribuiti Client            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Mantiene i collegamenti tra file NTFS all'interno di un computer o in computer in una rete.
|   **Nome del servizio**    |   TrkWks
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="distributed-transaction-coordinator"></a>Distributed Transaction Coordinator     
| | |           
|---|---|   
|   **Descrizione del servizio** |   Coordina le transazioni che si estendono su più gestioni risorse, quali database, code di messaggi e file System. Se questo servizio viene arrestato, queste transazioni avrà esito negativo. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   MSDTC
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />  

##  <a name="dmwappushsvc"></a>dmwappushsvc        
| | |           
|---|---|       
|   **Descrizione del servizio** |   Servizio di Routing WAP Push messaggi
|   **Nome del servizio**    |   dmwappushservice
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Servizio richiesto nei dispositivi client per Intune, MDM e tecnologie simili di gestione e per filtro scrittura unificato. Non è necessaria per Server.
|||         
            
<br />      

##  <a name="dns-client"></a>Client DNS      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio Client DNS (dnscache) memorizza nella cache i nomi di sistema DNS (Domain Name) e registra il nome completo del computer per il computer. Se il servizio viene arrestato, i nomi DNS continuerà a essere risolti. Tuttavia, non verrà memorizzati i risultati di query relative ai nomi DNS e il nome del computer non verrà registrato. Se il servizio è disattivato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   DnsCache
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="downloaded-maps-manager"></a>Mappe scaricate Manager     
| | |           
|---|---|   
|   **Descrizione del servizio** |   Servizio Windows per l'accesso all'applicazione di mappe scaricate. Questo servizio viene avviato su richiesta dall'applicazione l'accesso a scaricata mappe. La disattivazione di questo servizio impedirà le app di accedere alle mappe.
|   **Nome del servizio**    |   MapsBroker
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   La disattivazione di App interruzioni che si basano sul servizio. OK per disabilitare se le app non basarsi su di essa
|||         
            
<br />          

## <a name="embedded-mode"></a>Modalità incorporata            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio modalità incorporata consente gli scenari relativi alle applicazioni in Background.  La disattivazione di questo servizio impedirà applicazioni in Background venga attivata.
|   **Nome del servizio**    |   embeddedmode
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="encrypting-file-system-efs"></a>Crittografia File System (EFS)
| | |                   
|---|---|           
|   **Descrizione del servizio** | È la tecnologia di crittografia file principale utilizzata per archiviare file crittografati nei volumi NTFS. Se questo servizio viene arrestato o disabilitato, le applicazioni in grado di accedere ai file crittografati.            
|   **Nome del servizio**  |  EFS            
|   **Installazione**  |  Sempre installata           
|   **StartType**   |  Manuale           
|   **Raccomandazione**  | Nessuna Guida           
|   **Commenti**   |
|||                 
                            
<br />  

## <a name="enterprise-app-management-service"></a>Servizio di gestione di App aziendali            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente la gestione di applicazioni enterprise.
|   **Nome del servizio**    |   EntAppSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="extensible-authentication-protocol"></a>Extensible Authentication Protocol           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio di protocollo EAP (Extensible Authentication) fornisce l'autenticazione di rete in tali scenari 802.1 x cablate e wireless, VPN e alla protezione di accesso di rete (NAP).  EAP fornisce inoltre application programming interface (API) che vengono utilizzati dai client di accesso di rete, inclusi client wireless e VPN, durante il processo di autenticazione.  Se si disabilita questo servizio, il computer viene impedito l'accesso a reti che richiedono l'autenticazione EAP.
|   **Nome del servizio**    |   EapHost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="function-discovery-provider-host"></a>Host Provider di individuazione funzioni         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio FDPHOST ospita i provider di individuazione di rete l'individuazione funzione (FD). Questi provider FD forniscono servizi di individuazione di rete per i servizi SSDP Simple Discovery Protocol () e i servizi Web - protocollo di individuazione (WS-D). L'arresto o la disabilitazione del servizio FDPHOST disabiliterà l'individuazione di rete per questi protocolli quando si utilizza FD. Quando questo servizio non è disponibile, servizi di rete utilizzando FD e basarsi su questi protocolli di individuazione è riusciti a trovare i dispositivi di rete o risorse.
|   **Nome del servizio**    |   fdPHost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="function-discovery-resource-publication"></a>Pubblicazione risorse per Individuazione funzione      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Pubblica questo computer e le risorse collegate al computer in modo che possono essere individuati in rete.  Se questo servizio viene arrestato, non verranno pubblicate non è più risorse di rete e non verranno individuati da altri computer nella rete.
|   **Nome del servizio**    |   FDResPub
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="geolocation-service"></a>Servizio di Georilevazione          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio consente di monitorare la posizione corrente del sistema e gestisce i recinti virtuali (una posizione geografica con eventi associati).  Se si disattiva il servizio, le applicazioni in grado di utilizzare o ricevere le notifiche di georilevazione o recinti virtuali.
|   **Nome del servizio**    |   lfsvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   La disattivazione di App interruzioni che si basano sul servizio. OK per disabilitare se le app non basarsi su di essa
|||         
            
<br />          

##  <a name="group-policy-client"></a>Client di criteri di gruppo     
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio è responsabile dell'applicazione delle impostazioni configurate dagli amministratori per il computer e utenti tramite il componente Criteri di gruppo. Se il servizio è disattivato, non verranno applicate le impostazioni e applicazioni e componenti non saranno possibile gestire tramite criteri di gruppo. Tutti i componenti o le applicazioni che dipendono dal componente Criteri di gruppo potrebbero non essere funzionali se il servizio è disattivato.
|   **Nome del servizio**    |   gpsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          


## <a name="human-interface-device-service"></a>Servizio Human Interface Device           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Abilita e mantiene l'uso dei pulsanti speciali su tastiere, telecomandi e altri dispositivi multimediali. È consigliabile mantenere questo servizio in esecuzione.
|   **Nome del servizio**    |   Hidserv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="hv-host-service"></a>Servizio Host HV     
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce un'interfaccia per l'hypervisor Hyper-V fornire i contatori delle prestazioni per ogni partizione del sistema operativo host.
|   **Nome del servizio**    |   HvHost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Incrementare la produzione per le macchine virtuali guest. Attualmente non utilizzato, ad eccezione di in modo esplicito popolati macchine virtuali, ma verrà utilizzato in Application Guard
|||         
            
<br />          

## <a name="hyper-v-data-exchange-service"></a>Servizio scambio di dati di Hyper-V        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce un meccanismo per lo scambio di dati tra la macchina virtuale e il sistema operativo in esecuzione nel computer fisico.
|   **Nome del servizio**    |   vmickvpexchange
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />      

## <a name="hyper-v-guest-service-interface"></a>Interfaccia servizio Guest Hyper-V          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce un'interfaccia per l'host Hyper-V interagire con servizi specifici in esecuzione all'interno della macchina virtuale.
|   **Nome del servizio**    |   vmicguestinterface
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Servizio arresto Guest Hyper-V           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce un meccanismo per arrestare il sistema operativo della macchina virtuale dalle interfacce di gestione nel computer fisico.
|   **Nome del servizio**    |   vmicshutdown
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />          
    
## <a name="hyper-v-heartbeat-service"></a>Servizio Heartbeat Hyper-V            
| | |           
|---|---|           
|   **Descrizione del servizio** |   Monitora lo stato della macchina virtuale da segnalazione un heartbeat a intervalli regolari. Questo servizio consente di identificare le macchine virtuali in esecuzione che non rispondono.
|   **Nome del servizio**    |   vmicheartbeat
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />          

## <a name="hyper-v-powershell-direct-service"></a>Servizio Hyper-V PowerShell Direct            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce un meccanismo per gestire la macchina virtuale con PowerShell tramite una sessione di macchina virtuale senza una rete virtuale.
|   **Nome del servizio**    |   vmicvmsession
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Servizio di virtualizzazione Desktop remoto Hyper-V            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce una piattaforma per la comunicazione tra la macchina virtuale e il sistema operativo in esecuzione nel computer fisico.
|   **Nome del servizio**    |   vmicrdv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />          

## <a name="hyper-v-time-synchronization-service"></a>Servizio di sincronizzazione ora Hyper-V         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Sincronizza l'ora di sistema della macchina virtuale con l'ora di sistema del computer fisico.
|   **Nome del servizio**    |   vmictimesync
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Richiedente Copia Shadow del Volume Hyper-V         
| | |           
|---|---|           
|   **Descrizione del servizio** |   Coordina le comunicazioni necessarie per utilizzare il servizio Copia Shadow del Volume per eseguire il backup di applicazioni e ai dati su questa macchina virtuale dal sistema operativo nel computer fisico.
|   **Nome del servizio**    |   vmicvss
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>IPsec IKE e AUTH moduli di impostazione chiavi          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio IKEEXT ospita i Internet Key Exchange (IKE) e Authenticated Internet Protocol (AuthIP) moduli di impostazione chiavi. Questi moduli vengono utilizzati per l'autenticazione e scambio di chiave in Internet Protocol security (IPsec). L'arresto o la disabilitazione del servizio IKEEXT disabiliterà scambio di chiave IKE e AuthIP con computer peer. IPsec è in genere configurata per l'utilizzo di IKE o AuthIP; di conseguenza, l'arresto o la disabilitazione del servizio IKEEXT potrebbe causare un errore di IPsec e potrebbe compromettere la protezione del sistema. Si consiglia di disporre in esecuzione il servizio IKEEXT.
|   **Nome del servizio**    |   IKEEXT
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |    
|||         
            
<br />          

## <a name="interactive-services-detection"></a>Rilevamento servizi interattivi           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Abilita la notifica dell'input utente per i servizi interattivi, che consente l'accesso alle create dai servizi interattivi quando vengono visualizzate le finestre di dialogo. Se questo servizio viene arrestato, le notifiche delle finestre di dialogo nuovo servizio interattivo smetteranno di funzionare e potrebbe non essere accesso per le finestre di dialogo servizio interattivo. Se questo servizio è disabilitato sia le notifiche di accesso per le finestre di dialogo nuovo servizio interattivo non funzioneranno più.
|   **Nome del servizio**    |   UI0Detect
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />  

## <a name="internet-connection-sharing-ics"></a>Condivisione connessione Internet (ICS)            
| | |           
|---|---|           
|   **Descrizione del servizio** |   Fornisce la conversione degli indirizzi, indirizzi, servizi di prevenzione di risoluzione e/o intrusioni nome per una rete di casa o in ufficio.
|   **Nome del servizio**    |   Condivisione connessione Internet
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Obbligatorio per i client utilizzati come hotspot Wi-Fi e anche in entrambe le estremità della proiezione Miracast. Condivisione connessione Internet può essere bloccata con l'impostazione oggetto Criteri di gruppo, "Impedisci l'utilizzo di Condivisione connessione Internet nella rete del dominio DNS"
|||         
            
<br />          

## <a name="ip-helper"></a>Helper IP            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce connettività tunnel utilizzando tecnologie di transizione IPv6 (6to4, Teredo, ISATAP e porta Proxy) e IP-HTTPS. Se questo servizio viene arrestato, il computer non sarà possibile che queste tecnologie offrono i vantaggi di connettività.
|   **Nome del servizio**    |   iphlpsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          


##  <a name="ipsec-policy-agent"></a>Agente criteri IPsec      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Internet Protocol security (IPsec) supporta l'autenticazione peer a livello di rete, l'autenticazione dell'origine dati, l'integrità dei dati, la riservatezza dei dati (crittografia) e protezione contro la riproduzione.  Questo servizio impone i criteri IPsec creati tramite lo snap-in Criteri di sicurezza IP o lo strumento da riga di comando "netsh ipsec".  Se si arresta il servizio, potrebbero verificarsi problemi di connettività di rete se i criteri richiedono che le connessioni utilizzano IPsec.  Inoltre, la gestione remota di Windows Firewall non è disponibile quando questo servizio viene arrestato.
|   **Nome del servizio**    |   PolicyAgent
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />

##  <a name="kdc-proxy-server-service-kps"></a>Servizio Server Proxy KDC (KPS)      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio Server Proxy KDC viene eseguito sui server edge al proxy Kerberos messaggi del protocollo per i controller di dominio nella rete aziendale.
|   **Nome del servizio**    |   KPSSVC
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida    
|   **Commenti**    |   
|||         
            
<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>KtmRm per Distributed Transaction Coordinator            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Coordina le transazioni tra Distributed Transaction Coordinator (MSDTC) e le transazioni Manager (kernel). Se non è necessaria, è consigliabile che restano questo servizio è arrestato. Se necessario, MSDTC sia gestione transazioni kernel verrà avviato il servizio automaticamente. Se questo servizio è disabilitato, qualsiasi transazione MSDTC interagire con Gestione risorse di Kernel avrà esito negativo e i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   KtmRm
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />

##  <a name="link-layer-topology-discovery-mapper"></a>Mapping di individuazione della topologia del livello di collegamento        
| | |       
|---|---|       
|   **Descrizione del servizio** |   Crea una mappa della rete, costituite da PC e informazioni sulla topologia (connettività) di dispositivi e i metadati che descrivono ogni PC e dispositivi.  Se questo servizio è disabilitato, la mappa di rete non funzionerà correttamente.
|   **Nome del servizio**    |   lltdsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   OK per disabilitare se senza dipendenze nella mappa di rete
|||         
            
<br />

## <a name="local-session-manager"></a>Gestione sessioni locali                    
| | |                   
|---|---|   
|   **Descrizione del servizio** |   Servizio Windows core che gestisce le sessioni utente locale. L'arresto o la disattivazione di questo servizio verrà causare instabilità del sistema.    
|   **Nome del servizio**    |   LSM |
|   **Installazione**    |   Sempre installata    |
|   **StartType**   |   Automatico   |
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||                 
                    
<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Agente di raccolta Standard Microsoft (R) diagnostica Hub         
| | |           
|---|---|           
|   **Descrizione del servizio** |   Servizio Windows core che gestisce le sessioni utente locale. L'arresto o la disattivazione di questo servizio verrà causare instabilità del sistema.
|   **Nome del servizio**    |   LSM
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          
            
## <a name="microsoft-account-sign-in-assistant"></a>Assistente per l'accesso Account Microsoft          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente l'accesso dell'utente tramite servizi di identità account Microsoft. Se questo servizio viene arrestato, gli utenti non saranno in grado di accedere al computer con il proprio account Microsoft.
|   **Nome del servizio**    |   wlidsvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Microsoft Accounts sono n/d in Windows Server
|||         
            
<br />          

##  <a name="microsoft-app-v-client"></a>Microsoft App-V Client      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce gli utenti di App-V e le applicazioni virtuali
|   **Nome del servizio**    |   AppVClient
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Disabilitato
|   **Raccomandazione**  |   Già disabilitato
|   **Commenti**    |   
|||         
            
<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Servizio iniziatore iSCSI Microsoft            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce le sessioni iSCSI (Internet SCSI) dal computer per i dispositivi di destinazione iSCSI remoto. Se questo servizio viene arrestato, il computer non sarà in grado di accedere alle destinazioni iSCSI. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   MSiSCSI
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   I dati di diagnostica indicano che viene utilizzato nel client, nonché i server. Non offre vantaggi disabilitando questa.
|||         
            
<br />          

## <a name="microsoft-passport"></a>Microsoft Passport           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce l'isolamento del processo per le chiavi di crittografia utilizzato per l'autenticazione a provider di identità associata dell'utente. Se questo servizio è disabilitato, utilizza tutti e la gestione di queste chiavi non sarà disponibile, che include computer accesso e single sign-in per le App e siti Web. Questo servizio viene avviato e arrestato automaticamente. È consigliabile non riconfigurare il servizio.
|   **Nome del servizio**    |   NgcSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Necessario per gli accessi PIN/Hello, che non sono supportati nel Server
|||         
            
<br />          

## <a name="microsoft-passport-container"></a>Contenitore di Microsoft Passport         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce le chiavi di identità utente locale usate per autenticare l'utente di provider di identità, nonché le smart card virtuali TPM. Se questo servizio è disabilitato, le chiavi di identità utente locale e smart card virtuali TPM non sarà accessibile. È consigliabile non riconfigurare il servizio.
|   **Nome del servizio**    |   NgcCtnrSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Provider di copie Shadow di Software Microsoft          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce le copie shadow di volume basato su software eseguite dal servizio Copia Shadow del Volume. Se questo servizio viene arrestato, le copie shadow del volume basate su software non possono essere gestite. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   SWPRV
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="microsoft-storage-spaces-smp"></a>Spazi di archiviazione Microsoft SMP         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Servizio host per il provider di gestione di spazi di archiviazione Microsoft. Se questo servizio viene arrestato o disabilitato, spazi di archiviazione non può essere gestiti.
|   **Nome del servizio**    |   smphost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Gestione dell'archiviazione API esito negativo senza questo servizio. Esempio: "Get-WmiObject-classe MSFT_Disk - Namespace Root\Microsoft\Windows\Storage".
|||         
            
<br />          

## <a name="nettcp-port-sharing-service"></a>Servizio di condivisione porta Net         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di condividere le porte TCP tramite il protocollo NET.
|   **Nome del servizio**    |   NetTcpPortSharing
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Disabilitato
|   **Raccomandazione**  |   Già disabilitato
|   **Commenti**    |   
|||         
            
<br />          

## <a name="netlogon"></a>Servizio Accesso rete         
| | |           
|---|---|           
|   **Descrizione del servizio** |   Mantiene un canale protetto tra il computer e il controller di dominio per l'autenticazione degli utenti e servizi. Se questo servizio viene arrestato, il computer potrebbe non autenticare gli utenti e servizi e il controller di dominio non può registrare i record DNS. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   Servizio Accesso rete
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="network-connection-broker"></a>Gestore connessione di rete            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Connessioni Broker che consentono alle App Store Microsoft ricevere notifiche da internet.
|   **Nome del servizio**    |   NcbService
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="network-connections"></a>Connessioni di rete         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce gli oggetti nella cartella di rete e connessioni remote, in cui è possibile visualizzare sia rete locale e le connessioni remote.
|   **Nome del servizio**    |   Netman
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="network-connectivity-assistant"></a>Assistente connettività di rete      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce una notifica sullo stato di DirectAccess per i componenti dell'interfaccia utente
|   **Nome del servizio**    |   NcaSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />  

##  <a name="network-list-service"></a>Servizio elenco reti        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Identifica le reti a cui il computer è connesso, raccoglie e archivia le proprietà di queste reti e notifica alle applicazioni quando queste proprietà vengono modificate.
|   **Nome del servizio**    |   netprofm
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="network-location-awareness"></a>Riconoscimento presenza in rete           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Raccoglie e archivia le informazioni di configurazione per la rete e notifica programmi quando queste informazioni sono state modificate. Se questo servizio viene arrestato, le informazioni di configurazione potrebbero essere disponibile. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   NlaSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="network-setup-service"></a>Servizio di configurazione di rete       
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio di configurazione di rete gestisce l'installazione del driver di rete e consente la configurazione delle impostazioni di rete a basso livello.  Se questo servizio viene arrestato, possono essere annullate eventuali installazioni di driver che sono in corso.
|   **Nome del servizio**    |   NetSetupSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="network-store-interface-service"></a>Servizio di interfaccia archivio di rete      
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio offre le notifiche di rete (ad esempio interfaccia aggiunta o l'eliminazione e così via) per i client in modalità utente. L'arresto del servizio causerà la perdita di connettività di rete. Se questo servizio è disabilitato, non riuscirà avviare altri servizi che dipendono da questo servizio in modo esplicito.
|   **Nome del servizio**    |   interfaccia archivio di rete
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="offline-files"></a>File offline            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio file non in linea esegue attività di manutenzione nella cache dei file Offline, risponde gli eventi di disconnessione e l'accesso dell'utente, implementa elementi interni dell'API pubblica e invia eventi interessanti per coloro che sono interessati le attività di file Offline e modifiche allo stato della cache.
|   **Nome del servizio**    |   CscService
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Disabilitato
|   **Raccomandazione**  |   Già disabilitato
|   **Commenti**    |   
|||         
            
<br />          

## <a name="optimize-drives"></a>Ottimizza unità          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente di eseguire in modo più efficiente per l'ottimizzazione dei file nelle unità di archiviazione nel computer.
|   **Nome del servizio**    |   defragsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />

## <a name="performance-counter-dll-host"></a>Host DLL di contatore delle prestazioni         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente agli utenti remoti e processi a 64 bit per ricercare i contatori delle prestazioni forniti da DLL a 32 bit. Se questo servizio viene arrestato, solo gli utenti locali e i processi a 32 bit sarà in grado di eseguire una query i contatori delle prestazioni forniti da DLL a 32 bit.
|   **Nome del servizio**    |   PerfHost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida    
|   **Commenti**    |   
|||         
            
<br />          

## <a name="performance-logs--alerts"></a>Avvisi e registri di prestazioni            
| | |           
|---|---|   
|   **Descrizione del servizio** |   I dati sulle prestazioni raccolti gli avvisi e registri di prestazioni da computer locali o remoti in base ai parametri di pianificazione preconfigurati, quindi scrive i dati in un registro o attiva un avviso. Se questo servizio viene arrestato, non verranno raccolti informazioni sulle prestazioni. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   erano messe a disposizione
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="phone-service"></a>Servizio telefonico       
| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce lo stato di telefonia nel dispositivo
|   **Nome del servizio**    |   PhoneSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Usato per le app moderne VoIP
|||         
            
<br />          

##      <a name="plug-and-play"></a>Plug and Play       
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente a un computer a riconoscere e adattarsi alle modifiche hardware con minima o senza input dell'utente. L'arresto o la disattivazione di questo servizio verrà causare instabilità del sistema.
|   **Nome del servizio**    |   PlugPlay
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="portable-device-enumerator-service"></a>Servizio enumeratore dispositivi mobili           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Applica i criteri di gruppo per i dispositivi di archiviazione di massa rimovibili. Consente alle applicazioni, ad esempio Windows Media Player e importazione guidata immagine per trasferire e sincronizzazione del contenuto utilizzando i dispositivi di archiviazione di massa rimovibili.
|   **Nome del servizio**    |   WPDBusEnum
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="power"></a>Risparmio energia            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce i criteri di risparmio energia e il recapito delle notifiche criteri alimentazione.
|   **Nome del servizio**    |   Risparmio energia
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="print-spooler"></a>Spooler di stampa            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio effettua lo spooling dei processi di stampa e gestisce l'interazione con la stampante.  Se si disattiva il servizio, non sarà in grado di stampare o vedere le stampanti.
|   **Nome del servizio**    |   Spooler
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   OK per disabilitare se non è un server di stampa o un controller di dominio
|   **Commenti**    |   In un controller di dominio, l'installazione del ruolo di controller di dominio viene aggiunto un thread per il servizio spooler che è responsabile per l'esecuzione di eliminazione di stampa – rimuovendo gli oggetti della coda di stampa non aggiornati da Active Directory.  Se il servizio spooler non è in esecuzione in almeno un controller di dominio in ogni sito, l'annuncio non ha alcun modo per rimuovere i vecchi code che non esistono più. https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         
            
<br />          

##  <a name="printer-extensions-and-notifications"></a>Le notifiche e le estensioni della stampante        
| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio consente di aprire le finestre di dialogo stampanti personalizzata e gestisce le notifiche da un server di stampa remoto o una stampante. Se si disattiva il servizio, non sarà in grado di visualizzare notifiche o le estensioni della stampante.
|   **Nome del servizio**    |   PrintNotify
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK se non è un server di stampa
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>Segnalazioni di problemi e soluzioni supporto per il pannello di controllo     
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio offre supporto per la visualizzazione, l'invio e l'eliminazione di segnalazioni di problemi a livello di sistema per il pannello di controllo di segnalazioni di problemi e soluzioni.
|   **Nome del servizio**    |   wercplsupport
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="program-compatibility-assistant-service"></a>Servizio di riconoscimento automatico argomenti compatibilità programma     
| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio offre supporto per il programma compatibilità Assistant (PCA).  PCA consente di monitorare applicazioni installate ed eseguite dall'utente e rileva i problemi di compatibilità noti. Se questo servizio viene arrestato, PCA non funzionerà correttamente.
|   **Nome del servizio**    |   PcaSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="quality-windows-audio-video-experience"></a>Windows Audio Video di qualità      
| | |           
|---|---|   
|   **Descrizione del servizio** |   Windows Audio Video di qualità (qWave) è una piattaforma di rete per l'Audio Video (AV) lo streaming di applicazioni in reti domestiche IP. qWave migliora prestazioni e affidabilità flusso assicurando rete quality of service (QoS) per le applicazioni AV AV. Fornisce meccanismi per il controllo ammissione, eseguire il monitoraggio di tempo e imposizione, il feedback dell'applicazione e priorità al traffico.
|   **Nome del servizio**    |   QWAVE
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Servizio QoS sul lato client
|||         
            
<br />          

##      <a name="radio-management-service"></a>Servizio di gestione di opzione        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Opzione di gestione e il servizio modalità aereo
|   **Nome del servizio**    |   RmSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

## <a name="remote-access-auto-connection-manager"></a>Auto Connection Manager di accesso remoto            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Crea una connessione a una rete remota ogni volta che un programma fa riferimento a un nome DNS o NetBIOS o l'indirizzo remoto.
|   **Nome del servizio**    |   RasAuto
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="remote-access-connection-manager"></a>Gestione connessione di accesso remoto         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce le connessioni remote e virtual private network (VPN) dal computer a Internet o ad altre reti remote. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   RasMan
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="remote-desktop-configuration"></a>Configurazione di Desktop remoto         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Servizio di configurazione di Desktop remoto (RDC) è responsabile per tutti i Servizi Desktop remoto e Desktop remoto correlati alla configurazione e le attività di manutenzione di sessione che richiedono il contesto di sistema. Queste includono cartelle temporanee per sessione, i temi di desktop remoto e i certificati di desktop remoto.
|   **Nome del servizio**    |   SessionEnv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="remote-desktop-services"></a>Servizi Desktop remoto          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente agli utenti di connettersi in modo interattivo a un computer remoto. Desktop remoto e Server Host sessione Desktop remoto dipendono da questo servizio.  Per impedire l'utilizzo del computer remoto, deselezionare le caselle di controllo nella scheda connessione remota dell'elemento del Pannello di controllo di proprietà del sistema.
|   **Nome del servizio**    |   TermService
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="remote-desktop-services-usermode-port-redirector"></a>Redirector porta UserMode di Servizi Desktop remoto        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente il reindirizzamento delle stampanti, unità o porte per le connessioni RDP
|   **Nome del servizio**    |   UmRdpService
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Supporta reindirizzamenti sul lato server della connessione.
|||         
            
<br />          

## <a name="remote-procedure-call-rpc"></a>Chiamata di procedura remota (RPC)          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio RPCSS è Gestione controllo servizi per i server COM e DCOM. Effettua richieste di attivazione di oggetto, le risoluzioni esportatore di oggetto e la garbage collection distribuita per i server COM e DCOM. Se questo servizio viene arrestato o disabilitato, i programmi con COM o DCOM non funzionerà correttamente. Si consiglia di disporre l'esecuzione del servizio RPCSS.
|   **Nome del servizio**    |   RpcSs
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>Individuazione di Remote Procedure Call (RPC)             
| | |               
|---|---|   
|   **Descrizione del servizio** |   In Windows 2003 e versioni precedenti di Windows, il servizio RPC Remote Procedure Call () Locator gestisce il database del servizio RPC nome. In Windows Vista e versioni successive di Windows, questo servizio non offre alcuna funzionalità ed è presente la compatibilità delle applicazioni.   |
|   **Nome del servizio**    |   RpcLocator  |
|   **Installazione**    |   Solo nel Datacenter Edition  |
|   **StartType**   |   Manuale  |
|   **Raccomandazione**  | Nessuna Guida   |
|   **Commenti**    |       |
|||             
                
<br />              

## <a name="remote-registry"></a>Registro di sistema remoto          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente agli utenti remoti modificare le impostazioni del Registro di sistema nel computer. Se questo servizio viene arrestato, è possibile modificare il Registro di sistema solo per gli utenti del computer. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   RemoteRegistry
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="resultant-set-of-policy-provider"></a>Di Provider di criteri risultante            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce un servizio di rete che elabora le richieste per simulare l'applicazione delle impostazioni di criteri di gruppo per un utente di destinazione o un computer in varie situazioni e calcola le impostazioni di criteri risultante.
|   **Nome del servizio**    |   RSoPProv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |Nessuna Guida    
|   **Commenti**    |   
|||         
            
<br />          

## <a name="routing-and-remote-access"></a>Routing e accesso remoto            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Offre servizi di routing per le aziende in ambienti di rete WAN e rete locale.
|   **Nome del servizio**    |   Accesso remoto
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Disabilitato
|   **Raccomandazione**  |   Già disabilitato
|   **Commenti**    |   Già disabilitato
|||         
            
<br />          

## <a name="rpc-endpoint-mapper"></a>Agente mapping Endpoint RPC          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Risolve gli identificatori di interfacce RPC agli endpoint di trasporto. Se questo servizio viene arrestato o disabilitato, i programmi tramite i servizi RPC Remote Procedure Call () non funzionerà correttamente.
|   **Nome del servizio**    |   RpcEptMapper
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="secondary-logon"></a>Accesso secondario     
| | |           
|---|---|       
|   **Descrizione del servizio** |   Abilita l'avvio di processi con credenziali alternative. Se questo servizio viene arrestato, non sarà disponibile questo tipo di accesso. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   seclogon
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>Secure Socket Tunneling Protocol servizio            
| | |               
|---|---|       
|   **Descrizione del servizio** |   Fornisce il supporto per il Tunneling protocollo SSTP (Secure Socket) per connettersi a computer remoti tramite VPN. Se questo servizio è disabilitato, gli utenti non saranno in grado di utilizzare SSTP per accedere a server remoti.    |
|   **Nome del servizio**    |   SstpSvc |
|   **Installazione**    |   Sempre installata    |
|   **StartType**   |   Manuale  |
|   **Raccomandazione**  |   Non disabilitare  |
|   **Commenti**    |   Disabilitare le interruzioni di RRAS   |
|||             
                
<br />              

## <a name="security-accounts-manager"></a>Gestione account di protezione            
| | |           
|---|---|       
|   **Descrizione del servizio** |   L'avvio del servizio segnala ad altri servizi che è pronto per accettare le richieste di gestione account di sicurezza (SAM).  La disattivazione di questo servizio impedirà altri servizi nel sistema di notifiche quando il database SAM è pronto, che a sua volta può causare tali servizi a non vengono avviati correttamente. Questo servizio non deve essere disabilitato.
|   **Nome del servizio**    |   SamSs
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Non disabilitare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="sensor-data-service"></a>Servizio di sensore dati  
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce i dati da una vasta gamma di sensori
|   **Nome del servizio**    |   SensorDataService
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />  

## <a name="sensor-monitoring-service"></a>Servizio di monitoraggio sensore            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di monitorare diversi sensori per esporre i dati e si adattano allo stato di sistema e dell'utente.  Se questo servizio viene arrestato o disabilitato, la luminosità dello schermo non si adatteranno a condizioni di luce. L'arresto del servizio potrebbe influire altre funzionalità del sistema e le funzionalità oltre.
|   **Nome del servizio**    |   SensrSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          
        
## <a name="sensor-service"></a>Servizio sensore           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Un servizio per i sensori che gestisce la funzionalità di diversi sensori. Gestisce l'orientamento di dispositivo semplice (SDO) e la cronologia per i sensori. Carica il sensore SDO che segnala modifiche di orientamento del dispositivo.  Se questo servizio viene arrestato o disabilitato, non verrà caricato il sensore SDO e pertanto la rotazione automatica non verrà eseguita. Raccolta della cronologia dei sensori verrà arrestato.
|   **Nome del servizio**    |   SensorService
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

## <a name="server"></a>Server           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Supporta file, stampa e named pipe la condivisione in rete per questo computer. Se questo servizio viene arrestato, queste funzioni non saranno disponibile. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   LanmanServer
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Necessario per la gestione remota, IPC$, la condivisione file SMB
|||         
            
<br />          

## <a name="shell-hardware-detection"></a>Rilevamento Hardware shell             
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce le notifiche di eventi hardware AutoPlay.
|   **Nome del servizio**    |   ShellHWDetection
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

## <a name="smart-card"></a>Smart Card           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce l'accesso alle smart card lette dal computer. Se questo servizio viene arrestato, il computer in grado di leggere le smart card. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   SCardSvr
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Disabilitato
|   **Raccomandazione**  |   Già disabilitato
|   **Commenti**    |   
|||         
            
<br />          

## <a name="smart-card-device-enumeration-service"></a>Servizio di enumerazione dispositivi Smart Card                    
| | |               
|---|---|       
|   **Descrizione del servizio** |   Crea i nodi del dispositivo software per tutti i lettori di smart card accessibili per una determinata sessione. Se questo servizio è disabilitato, non sarà in grado di enumerare i lettori di smart card APIs WinRT.   |
|   **Nome del servizio**    |   ScDeviceEnum    |
|   **Installazione**    |   Sempre installata    |
|   **StartType**   |   Manuale  |
|   **Raccomandazione**  |   Per disabilitare OK   |
|   **Commenti**    |   Necessario quasi esclusivamente per le app a WinRT    |
|||             
                
<br />              

## <a name="smart-card-removal-policy"></a>Criterio di rimozione della Smart Card        
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente al sistema di essere configurato per bloccare il desktop utente al momento della rimozione della smart card.
|   **Nome del servizio**    |   SCPolicySvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="snmp-trap"></a>Trap SNMP            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Riceve messaggi trap generati da agenti Simple Network Management Protocol (SNMP) locali o remoti e inoltra i messaggi per i programmi di gestione SNMP in esecuzione nel computer. Se questo servizio viene arrestato, i programmi basati su SNMP su questo computer non riceverà messaggi trap SNMP. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   SNMPTRAP
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="software-protection"></a>Protezione da software             
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente il download, installazione e l'imposizione di licenze digitali per Windows e Windows applicazioni. Se il servizio è disabilitato, il sistema operativo e le applicazioni con licenza possono essere eseguite in una modalità di notifica. È consigliabile non disattivare il servizio di protezione Software.
|   **Nome del servizio**    |   sppsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="special-administration-console-helper"></a>Helper Console di amministrazione speciale        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente agli amministratori di accedere in modalità remota un prompt dei comandi utilizzando servizi di gestione emergenze.
|   **Nome del servizio**    |   Sacsvr
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="spot-verifier"></a>Campione Verifier            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Verifica potenziali danneggiamenti del file system.
|   **Nome del servizio**    |   svsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="ssdp-discovery"></a>Individuazione SSDP           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente di individuare i dispositivi di rete e servizi che utilizzano il protocollo di individuazione SSDP, ad esempio dispositivi UPnP. Annuncia anche i dispositivi vengono utilizzati SSDP e servizi in esecuzione nel computer locale. Se questo servizio viene arrestato, non i dispositivi basati su SSDP verranno individuati. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   SSDPSRV
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

## <a name="state-repository-service"></a>Repository di stato servizio         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Supporta l'infrastruttura necessaria per il modello di applicazione.
|   **Nome del servizio**    |   StateRepository
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="still-image-acquisition-events"></a>Gli eventi di acquisizione di acquisizione immagini
| | |           
|---|---|   
|   **Descrizione del servizio** |   Avvia applicazioni associate ancora gli eventi di acquisizione immagine.
|   **Nome del servizio**    |   WiaRpc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />  

## <a name="storage-service"></a>Servizio di archiviazione          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce servizi di attivazione per le impostazioni di archiviazione e di archiviazione esterna di espansione
|   **Nome del servizio**    |   StorSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="storage-tiers-management"></a>Gestione livelli di archiviazione        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente di ottimizzare la posizione dei dati in livelli di archiviazione su tutti gli spazi di archiviazione a livelli nel sistema.
|   **Nome del servizio**    |   TieringEngineService
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="superfetch"></a>SuperFetch          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Mantiene e migliora le prestazioni del sistema nel tempo.
|   **Nome del servizio**    |   SysMain
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="sync-host"></a>Host di sincronizzazione            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio Sincronizza mail, contatti, calendario e diversi altri dati utente. Posta e altre applicazioni che dipende da questa funzionalità non funzionerà correttamente quando questo servizio non è in esecuzione.
|   **Nome del servizio**    |   OneSyncSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Modello di servizio utente
|||         
            
<br />          

## <a name="system-event-notification-service"></a>Servizio di notifica eventi di sistema            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Controlla eventi di sistema e notifica ai sottoscrittori di sistema degli eventi COM+ di questi eventi.
|   **Nome del servizio**    |   SENS
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="system-events-broker"></a>Gestore di eventi di sistema             
| | |           
|---|---|       
|   **Descrizione del servizio** |   Coordina l'esecuzione di attività in background per WinRT applicazione. Se questo servizio viene arrestato o disabilitato, attività in background potrebbe non essere attivata.
|   **Nome del servizio**    |   SystemEventsBroker
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Anche se la descrizione indica che è solo per le app WinRT, è necessario per l'utilità di pianificazione, servizio infrastruttura gestore e altri componenti interni.
|||         
            
<br />          

## <a name="task-scheduler"></a>Utilità di pianificazione           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente all'utente di configurare e pianificare attività automatizzate in questo computer. Il servizio ospita anche più attività di sistema critici di Windows. Se questo servizio viene arrestato o disabilitato, non verrà eseguite queste attività all'ora volte. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   Pianificazione
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="tcpip-netbios-helper"></a>NetBIOS Helper TCP/IP            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Offre supporto per NetBIOS su TCP/IP (NetBT) e risoluzione dei nomi NetBIOS per i client della rete, pertanto consentendo agli utenti di condividere file, stampa e accedere alla rete. Se questo servizio viene arrestato, queste funzioni potrebbero essere disponibile. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   LMHOSTS
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="telephony"></a>Telefonia           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce il supporto delle API TAPI (Telephony) per i programmi che controllano le periferiche nel computer locale e, tramite la rete LAN, nei server che eseguono il servizio di telefonia.
|   **Nome del servizio**    |   TapiSrv
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Disabilitare le interruzioni di RRAS
|||         
            
<br />          

## <a name="themes"></a>Temi           
| | |           
|---|---|
|   **Descrizione del servizio** |   Fornisce la gestione di tema esperienza utente.
|   **Nome del servizio**    |   Temi
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Impossibile impostare i temi di accessibilità quando questo servizio è disattivato
|||         
            
<br />  

## <a name="tile-data-model-server"></a>Riquadro server di modello di dati           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Riquadro Server per gli aggiornamenti dei riquadri.
|   **Nome del servizio**    |   tiledatamodelsvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Avviare le interruzioni di menu se questo servizio è disabilitato
|||         
            
<br />          

##  <a name="time-broker"></a>Gestore di tempo     
| | |           
|---|---|       
|   **Descrizione del servizio** |   Coordina l'esecuzione di attività in background per WinRT applicazione. Se questo servizio viene arrestato o disabilitato, attività in background potrebbe non essere attivata.
|   **Nome del servizio**    |   TimeBrokerSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Anche se la descrizione indica che è solo per le app WinRT, è necessario per l'utilità di pianificazione, servizio infrastruttura gestore e altri componenti interni.
|||         
            
<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>La tastiera virtuale e il servizio di riconoscimento della grafia pannello         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Abilitare la funzionalità tastiera virtuale e pannello per la grafia della penna e input penna
|   **Nome del servizio**    |   TabletInputService
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>Servizio agente di orchestrazione di aggiornamento per Windows Update           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce gli aggiornamenti di Windows. Se è stato arrestato, i dispositivi non sarà in grado di scaricare e installare gli aggiornamenti più recenti.
|   **Nome del servizio**    |   UsoSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Descrizione del servizio non era presente in v1607; Windows Update (incluso Windows Server Update Services) dipende questo servizio.
|||         
            
<br />          

## <a name="upnp-device-host"></a>Host di dispositivi UPnP         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente di essere ospitato in questo computer dispositivi UPnP. Se questo servizio viene arrestato, eventuali dispositivi UPnP ospitati smetteranno di funzionare e non altri dispositivi ospitati possono essere aggiunti. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   UPnPHost
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

## <a name="user-access-logging-service"></a>Servizio registrazione accessi utente          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio registra le richieste di accesso client univoci, sotto forma di indirizzi IP e nomi utente, dei ruoli nel server locale e i prodotti installati. Queste informazioni è possibile eseguire query, tramite Powershell, dagli amministratori di dover quantificare richiesta client del software server di gestione licenze CAL (Client Access License) non in linea. Se il servizio è disabilitato, le richieste client non verranno registrate e non sarà possibile recuperare tramite query di Powershell. L'arresto del servizio non avrà effetto sulle query dei dati cronologici (vedere la documentazione per la procedura eliminare i dati cronologici di supporto). L'amministratore di sistema locale è necessario consultare, il suo, condizioni di licenza di Windows Server per determinare il numero di licenze di accesso client che sono necessari per il software server di una licenza in modo appropriato; Utilizzare il servizio registrazione accesso utenti e i dati non modifica l'obbligo.
|   **Nome del servizio**    |   UALSVC
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="user-data-access"></a>Accesso ai dati utente        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce alle app di accedere ai dati utente strutturato, incluse le informazioni di contatto, calendari, messaggi e altri contenuti. Se si arresta o disabilitare il servizio, le app che usano questi dati potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   UserDataSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Modello di servizio utente
|||         
            
<br />          

## <a name="user-data-storage"></a>Archiviazione dei dati utente            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce l'archiviazione dei dati utente strutturato, incluse le informazioni di contatto, calendari, messaggi e altri contenuti. Se si arresta o disabilitare il servizio, le app che usano questi dati potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   UnistoreSvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Modello di servizio utente
|||         
            
<br />          

## <a name="user-experience-virtualization-service"></a>Servizio di virtualizzazione esperienza utente           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce supporto per applicazioni e roaming delle impostazioni del sistema operativo
|   **Nome del servizio**    |   UevAgentService
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Disabilitato
|   **Raccomandazione**  |   Già disabilitato
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="user-manager"></a>Gestione utenti        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Utente Manager fornisce i componenti di runtime necessari per l'interazione multi dell'utente di.  Se questo servizio viene arrestato, alcune applicazioni potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   UserManager
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="user-profile-service"></a>Servizio profili utente         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio è responsabile per caricare e scaricare i profili utente. Se questo servizio viene arrestato o disabilitato, gli utenti non saranno più in grado di accedere correttamente o App potrebbero avere problemi di acquisizione di dati degli utenti e componenti registrati per ricevere le notifiche di eventi profilo non ricevano.
|   **Nome del servizio**    |   ProfSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="virtual-disk"></a>Disco virtuale             
| | |           
|---|---|   
|   **Descrizione del servizio** |   Offre servizi di gestione per i dischi, volumi, file System e gli array di archiviazione.
|   **Nome del servizio**    |   VDS
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Nessuna Guida
|   **Commenti**    |   
|||         
            
<br />          

## <a name="volume-shadow-copy"></a>Copia Shadow del volume           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce e implementa copie Shadow del Volume utilizzato per il backup e altri scopi. Se questo servizio viene arrestato, le copie shadow non sarà disponibile per il backup e il backup potrebbe non riuscire. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   VSS
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Nessuna Guida
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="walletservice"></a>WalletService           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Oggetti host utilizzati dai client di nel portafoglio
|   **Nome del servizio**    |   WalletService
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-audio"></a>Audio di Windows            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce l'audio per i programmi basati su Windows.  Se questo servizio viene arrestato, effetti e periferiche audio non funzionerà correttamente.  Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare
|   **Nome del servizio**    |   AudioSrv
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-audio-endpoint-builder"></a>Generatore Endpoint Audio di Windows           
| | |           
|---|---|
|   **Descrizione del servizio** |   Gestisce i dispositivi audio per il servizio Windows Audio.  Se questo servizio viene arrestato, effetti e periferiche audio non funzionerà correttamente.  Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare
|   **Nome del servizio**    |   AudioEndpointBuilder
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-biometric-service"></a>Servizio di biometria            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio di biometria offre la possibilità di acquisire, confrontare, modificare e archiviare dati biometrici senza accedere direttamente a qualsiasi hardware biometrico o esempi alle applicazioni client. Il servizio è ospitato in un processo SVCHOST privilegiato.
|   **Nome del servizio**    |   WbioSrvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="windows-camera-frame-server"></a>Fotogrammi della fotocamera di Windows Server         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente a più client di accedere a fotogrammi video da fotocamere.
|   **Nome del servizio**    |   FrameServer
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-connection-manager"></a>Gestione connessioni Windows           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Rende automatica connettono/disconnettono decisioni in base alle opzioni di connettività di rete attualmente disponibili per il PC e consente la gestione della connettività di rete in base alle impostazioni di criteri di gruppo.
|   **Nome del servizio**    |   Wcmsvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-defender-network-inspection-service"></a>Servizio controllo di rete di Windows Defender          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Proteggere il PC da tentativi di intrusione vulnerabilità note e individuate di recente nei protocolli di rete di destinazione
|   **Nome del servizio**    |   WdNisSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida    
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-defender-service"></a>Servizio Windows Defender         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di proteggere gli utenti dal malware e altro software potenzialmente indesiderato
|   **Nome del servizio**    |   WinDefend
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows Driver Foundation - Framework Driver in modalità utente           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Crea e gestisce i processi di driver in modalità utente. Non può essere arrestato.
|   **Nome del servizio**    |   wudfsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-encryption-provider-host-service"></a>Servizio Host Provider di crittografia Windows     
| | |           
|---|---|   
|   **Descrizione del servizio** |   Servizio Host Provider di crittografia Windows Broker crittografia correlati funzionalità dal provider di crittografia di terze parti a processi che è necessario valutare e applicare i criteri EAS. L'arresto a questo compromette i controlli di conformità EAS che sono stati stabiliti dall'account di posta connesso
|   **Nome del servizio**    |   WEPHOSTSVC
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-error-reporting-service"></a>Servizio di segnalazione errori Windows          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di errori da quando i programmi smettono di funzionare o non risponde e soluzioni esistenti devono essere recapitati. Consente inoltre di generare per diagnostica registri e ripristinare i servizi. Se questo servizio viene arrestato, segnalazione errori potrebbero non funzionare correttamente e risultati di servizi di diagnostica e di operazioni di ripristino potrebbero non essere visualizzati.
|   **Nome del servizio**    |   WerSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Raccoglie e invia dati si arresti anomali o blocchi utilizzati da MS sia gli ISV/IHV di terze parti. I dati vengono utilizzati per individuare bug indurre arresto anomalo del sistema, che può includere gli errori di protezione. Necessario anche per segnalazione errori
|||         
            
<br />          

## <a name="windows-event-collector"></a>Raccolta eventi Windows          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio gestisce sottoscrizioni permanenti per gli eventi da origini remote che supportano il protocollo WS-Management. Sono inclusi i registri eventi, hardware e le origini eventi inoltrati in Windows Vista. Servizio memorizza inoltrati eventi nel registro eventi locale. Se questo servizio viene arrestato o disabilitato, non è possibile creare sottoscrizioni di eventi ed eventi inoltrati non possono essere accettati.
|   **Nome del servizio**    |   Wecsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Raccoglie gli eventi ETW (inclusi gli eventi di protezione) per la gestibilità, diagnostica.  Molte funzionalità e gli strumenti di terze parti si basano su di esso, inclusi gli strumenti di controllo di sicurezza
|||         
            
<br />          

## <a name="windows-event-log"></a>Registro eventi di Windows            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio gestisce gli eventi e registri eventi. Supporta gli eventi di registrazione, eventi, la sottoscrizione di eventi, l'archiviazione dei registri eventi e la gestione dei metadati di eventi di query. È possibile visualizzare gli eventi in formato XML o testo normale. L'arresto del servizio può compromettere la sicurezza e l'affidabilità del sistema.
|   **Nome del servizio**    |   Registro eventi
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-firewall"></a>Windows Firewall         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Windows Firewall consente di proteggere il computer, impedendo agli utenti non autorizzati di accedere al computer tramite Internet o una rete.
|   **Nome del servizio**    |   MpsSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="windows-font-cache-service"></a>Servizio di Windows tipi di carattere Cache      
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente di ottimizzare le prestazioni delle applicazioni per la memorizzazione nella cache i dati di uso comune. Se non è già in esecuzione applicazioni avvia il servizio. È possibile disattivarlo, anche se verrà peggiorare le prestazioni delle applicazioni.
|   **Nome del servizio**    |   FontCache
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-image-acquisition-wia"></a>Acquisizione di immagini di Windows (WIA)          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce i servizi di acquisizione immagini per scanner e fotocamere digitali
|   **Nome del servizio**    |   stisvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="windows-insider-service"></a>Servizio di Windows Insider     
| | |           
|---|---|   
|   **Descrizione del servizio** |   wisvc
|   **Nome del servizio**    |   wisvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Server non supporta versioni di anteprima, pertanto è no-op nel Server. Funzionalità può anche essere disabilitata tramite criteri di gruppo.
|||         
            
<br />          

##  <a name="windows-installer"></a>Windows Installer       
| | |           
|---|---|
|   **Descrizione del servizio** |   Aggiunge, modifica e rimuove applicazioni fornite come un pacchetto Windows Installer (MSI, *.msp). Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   MSIServer
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-license-manager-service"></a>Servizio gestione licenze di Windows          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce un'infrastruttura di supporto per Microsoft Store.  Questo servizio viene avviato su richiesta e se disabilitata quindi contenuto acquistato tramite Microsoft Store non funzionerà correttamente.
|   **Nome del servizio**    |   LicenseManager
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-management-instrumentation"></a>Strumentazione gestione Windows       
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce un modello comune di interfaccia e un oggetto di informazioni di gestione accesso sul sistema operativo, i dispositivi, applicazioni e servizi. Se questo servizio viene arrestato, la maggior parte dei software basati su Windows non funzionerà correttamente. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   Winmgmt
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="windows-mobile-hotspot-service"></a>Servizio Hotspot Mobile di Windows          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Offre la possibilità di condividere una connessione alla rete dati con un altro dispositivo.
|   **Nome del servizio**    |   icssvc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-modules-installer"></a>I moduli di Windows Installer        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente l'installazione, modifica e rimozione di aggiornamenti di Windows e componenti facoltativi. Se questo servizio è disabilitato, installare o disinstallare di mancata riuscita degli aggiornamenti per il computer Windows.
|   **Nome del servizio**    |   TrustedInstaller
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-push-notifications-system-service"></a>Servizio di sistema le notifiche Push            
| | |           
|---|---|
|   **Descrizione del servizio** |   Questo servizio viene eseguito nella sessione 0 e ospita il provider di piattaforma e connessione notifica che gestisce la connessione tra il dispositivo e il server WNS.
|   **Nome del servizio**    |   WpnService
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Necessario per i riquadri animati e altre funzionalità
|||         
            
<br />      

## <a name="windows-push-notifications-user-service"></a>Windows Push Notification Service utente          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio ospita piattaforma di notifica di Windows che fornisce il supporto per locale e le notifiche push. Le notifiche supportate sono riquadro, avviso popup e non elaborati.
|   **Nome del servizio**    |   WpnUserService
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Per disabilitare OK
|   **Commenti**    |   Modello di servizio utente
|||         
            
<br />          
    
## <a name="windows-remote-management-ws-management"></a>Gestione remota Windows (WS-Management)            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Servizio gestione remota Windows (WinRM) implementa il protocollo WS-Management per la gestione remota. WS-Management è un protocollo di servizi web standard usato per la gestione remota di hardware e software. Il servizio WinRM è in ascolto sulla rete per le richieste di WS-Management e le elabora. Il WinRM Service deve essere configurato con un listener tramite lo strumento da riga di comando WinRM o tramite criteri di gruppo in modo che per l'ascolto in rete. Il servizio WinRM fornisce l'accesso ai dati WMI e consente la raccolta di eventi. Raccolta di eventi e la sottoscrizione a eventi richiedono che il servizio sia in esecuzione. Messaggi di gestione remota Windows utilizzano HTTP e HTTPS come trasporto. Il servizio WinRM non dipende da IIS, ma è preconfigurato per la condivisione di una porta con IIS nello stesso computer.  Il servizio WinRM si riserva il prefisso URL utilizzino. Per evitare conflitti con IIS, gli amministratori devono assicurarsi che qualsiasi sito Web ospitato in IIS non utilizzano il prefisso URL utilizzino.
|   **Nome del servizio**    |   Gestione remota Windows
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Necessario per la gestione remota
|||         
            
<br />          

##  <a name="windows-search"></a>Windows Search      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce l'indicizzazione del contenuto, la memorizzazione nella cache di proprietà e i risultati della ricerca per i file di posta elettronica e altri contenuti.
|   **Nome del servizio**    |   WSearch
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Disabilitato
|   **Raccomandazione**  |   Già disabilitato
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="windows-time"></a>Ora di Windows        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Mantiene la sincronizzazione di data e ora in tutti i client e server nella rete. Se questo servizio viene arrestato, la sincronizzazione di data e ora non sarà disponibile. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   W32Time
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-update"></a>Windows Update           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente il rilevamento, download e installazione degli aggiornamenti per Windows e altri programmi. Se questo servizio è disabilitato, gli utenti del computer non saranno in grado di utilizzare Windows Update o la funzione di aggiornamento automatico e i programmi non sarà in grado di usare l'agente di Windows Update (WUA) API.
|   **Nome del servizio**    |   wuauserv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>Servizio di WinHTTP Web Proxy Auto-Discovery         
| | |           
|---|---|   
|   **Descrizione del servizio** |   WinHTTP implementa lo stack di client HTTP e offre agli sviluppatori con un'API Win32 e il componente di automazione COM per inviare le richieste HTTP e la ricezione delle risposte. Inoltre, WinHTTP fornisce supporto per l'individuazione automatica di una configurazione del proxy tramite l'implementazione del protocollo Web Proxy Auto-Discovery (WPAD).
|   **Nome del servizio**    |   WinHttpAutoProxySvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Tutto ciò che utilizza lo stack di rete può avere una dipendenza di funzionalità in questo servizio. Molte organizzazioni si basano su questa opzione per configurare il proxy HTTP delle reti interne routing.  In caso contrario, le connessioni HTTP provenienti internamente a Internet tutti non riuscirà.
|||         
            
<br />          

## <a name="wired-autoconfig"></a>Configurazione automatica reti cablate         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio configurazione automatica reti cablate (DOT3SVC) è responsabile per l'esecuzione di IEEE 802.1 X autenticazione su interfacce Ethernet. Se la distribuzione di rete cablata corrente impone l'autenticazione 802.1 X, il servizio DOT3SVC deve essere configurato per l'esecuzione per stabilire la connettività di livello 2 e/o che fornisca accesso alle risorse di rete. Reti cablate che non si applicano l'autenticazione 802.1 X non vengono influenzate dal servizio DOT3SVC.
|   **Nome del servizio**    |   dot3svc
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="wmi-performance-adapter"></a>Scheda prestazioni WMI          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce informazioni della libreria delle prestazioni dai provider di Strumentazione gestione Windows (WMI) per i client della rete. Questo servizio viene eseguito solo quando viene attivata Helper dati sulle prestazioni.
|   **Nome del servizio**    |   wmiApSrv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manuale
|   **Raccomandazione**  | Nessuna Guida       
|   **Commenti**    |   
|||         
            
<br />          

## <a name="workstation"></a>Workstation          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Crea e gestisce le connessioni di rete client ai server remoti utilizzando il protocollo SMB. Se questo servizio viene arrestato, queste connessioni sarà disponibile. Se questo servizio è disabilitato, tutti i servizi che dipendono esplicitamente sarà possibile avviare.
|   **Nome del servizio**    |   LanmanWorkstation
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Nessuna Guida       
|   **Commenti**    |   
|||         
            
<br />

## <a name="xbox-live-auth-manager"></a>Xbox Live Auth Manager           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Offre servizi di autenticazione e autorizzazione per l'interazione con Xbox Live. Se questo servizio viene arrestato, alcune applicazioni potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   XblAuthManager
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Deve essere disabilitato
|   **Commenti**    |   
|||         
            
<br />          

## <a name="xbox-live-game-save"></a>Xbox Live gioco Salva          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Le sincronizzazioni questo servizio salvare i dati per Xbox Live di salvataggio giochi abilitati.  Se questo servizio viene arrestato, gioco Salva i dati non caricare o scaricare da Xbox Live.
|   **Nome del servizio**    |   XblGameSave
|   **Installazione**    |   Solo nel Datacenter Edition
|   **StartType**   |   Manuale
|   **Raccomandazione**  |   Deve essere disabilitato
|   **Commenti**    |   Le sincronizzazioni questo servizio salvare i dati per Xbox Live di salvataggio giochi abilitati.  Se questo servizio viene arrestato, gioco Salva i dati non caricare o scaricare da Xbox Live.
|||         
                
<br /> 
<br /> 

