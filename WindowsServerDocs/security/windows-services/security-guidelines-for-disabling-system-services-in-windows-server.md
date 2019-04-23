---
title: Linee guida sulla sicurezza per i servizi di sistema in Windows Server 2016
description: Linee guida di sicurezza per la disattivazione dei servizi in Windows Server 2016 con esperienza Desktop
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 323985cf316bda2fa6ab6a1721e2b6316450391a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844472"
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>Materiale sussidiario sulla disabilitazione di servizi di sistema in Windows Server 2016 con esperienza Desktop

Si applica a: Windows Server 2016

Il sistema operativo Windows include molti servizi di sistema che forniscono importanti funzionalità. Servizi differenti presentano criteri di avvio predefinito diverso: alcuni vengono avviati per impostazione predefinita (automatico), alcuni quando necessario (manuale) e alcuni sono disabilitati per impostazione predefinita e deve essere abilitate in modo esplicito prima che possano eseguire. Questi valori predefiniti sono stati scelti con attenzione per ogni servizio bilanciare prestazioni e funzionalità di sicurezza per i clienti tipici.

Tuttavia, alcuni clienti aziendali può essere preferibile un saldo più incentrato sulla sicurezza per i PC Windows e server, che riduce l'attacco al minimo della superficie di attacco e pertanto può essere opportuno disabilitare completamente tutti i servizi non necessari nelle specifiche ambienti. Per i clienti, Microsoft® consiste nel fornire le istruzioni di accompagnamento relative ai servizi in modo sicuro possono essere disabilitati per questo scopo.

Le linee guida sono solo per Windows Server 2016 con esperienza Desktop (solo se usato in sostituzione del desktop per gli utenti finali). A partire da Windows Server 2019, queste indicazioni sono configurate per impostazione predefinita. Ogni servizio nel sistema è stato categorizzato come indicato di seguito:

-   **Necessario disabilitare:** Un'organizzazione incentrata sulla protezione preferiranno probabilmente disabilitare questo servizio e rinunciare relativa funzionalità (vedere dettagli aggiuntivi riportato di seguito).
- **OK per disattivare:** Questo servizio offre funzionalità che è utile per alcuni ma non tutte le aziende e aziende incentrato sulla sicurezza che non usano in modo sicuro possono disabilitarlo.
- **Non disabilitare:** Se si disabilita questo servizio verrà influire sulle funzionalità essenziali o impedire le funzionalità o ruoli specifici corretto funzionamento. Di conseguenza non deve essere disabilitata.
-  **(Nessuna indicazione):** L'impatto della disattivazione di questi servizi non è stato valutato completamente. Pertanto, la configurazione predefinita di questi servizi non deve essere modificata.


I clienti possono configurare i PC Windows e i server per disabilitare i servizi selezionati usando i modelli di protezione nei loro criteri di gruppo o tramite automazione di PowerShell. In alcuni casi, il materiale sussidiario include specifiche impostazioni di criteri di gruppo che disabilitano la funzionalità del servizio direttamente, come alternativa alla disabilitazione del servizio stesso.

Microsoft consiglia che i clienti di disabilitare i servizi seguenti e le rispettive attività pianificate in Windows Server 2016 con esperienza Desktop:

Servizi: 
1. Xbox Live Auth Manager
2. Salva gioco con Xbox Live

Attività pianificate: 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(È anche possibile accedere alle informazioni su tutti i servizi descritti in dettaglio in questo articolo, visualizzare il foglio di calcolo di Microsoft Excel associato: [Materiale sussidiario sulla disabilitazione di servizi di sistema in Windows Server 2016 con esperienza Desktop](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))

<br />

### <a name="disabling-services-not-installed-by-default"></a>La disabilitazione dei servizi non è installati per impostazione predefinita

Microsoft sconsiglia l'applicazione dei criteri per disabilitare i servizi che non sono installati per impostazione predefinita.
-  Il servizio è in genere necessario se è installata la funzionalità. L'installazione del servizio o la funzionalità richiede diritti amministrativi. Non consentire l'installazione delle funzionalità, non l'avvio del servizio.
-  Bloccando il servizio di Microsoft Windows non arresta un amministratore o senza privilegi di amministratore in alcuni casi da installazione di un equivalente di terze parti simile, ad esempio uno con un rischio di sicurezza più elevato.
-  Una linea di base o di benchmark che disabilita un servizio di Windows diverso (ad esempio, W3SVC) fornirà alcuni revisori l'impressione errata che la tecnologia (ad esempio IIS) è intrinsecamente non sicura e non deve essere mai utilizzata.
-  Se la funzionalità (e un servizio) non è installati, verrà aggiunta solo bulk non necessari per la linea di base e di lavoro di verifica.

<br />
Per tutti i servizi di sistema indicati in questo documento, le due tabelle seguenti offrono una spiegazione delle colonne e i suggerimenti di Microsoft per l'abilitazione e disabilitazione dei servizi di sistema in Windows Server 2016 con esperienza Desktop: 
 
<br />

### <a name="explanation-of-columns"></a>Spiegazione delle colonne

| | |
|---|---|
|**Descrizione del servizio**|   Descrizione del servizio, da sc.exe qdescription.|
|**Name** |Nome della chiave (interna) del servizio|
|**Installazione** |Sempre installati: Servizio è in Server Core e Server con esperienza Desktop  <br /> Solo con esperienza Desktop: Servizio in Windows Server 2016 con esperienza Desktop, ma ***non*** in Server Core |
|**StartType**  |Tipo di avvio in Windows Server 2016|
|**Raccomandazione** |Raccomandazione Microsoft/consigli sulla disabilitazione di questo servizio in Windows Server 2016 in una distribuzione aziendale tipica e ben gestita e in cui il server non viene utilizzato come una sostituzione del desktop dell'utente finale.|
|**Commenti** |SPIEGAZIONE aggiuntiva|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Spiegazioni delle raccomandazioni di Microsoft

| | |
|---|---|
|**Non disabilitare** |Questo servizio non deve essere disabilitato|
|**OK per disattivare**| Questo servizio può essere disabilitato se non viene utilizzata la funzionalità che supporta.|
|**Già disabilitata**|  Il servizio viene disabilitato per impostazione predefinita. non è necessario applicare con i criteri|
|**Deve essere disabilitato** |Questo servizio non deve mai essere abilitato in un sistema ben gestiti a livello aziendale.|

<br />

Nelle tabelle seguenti offrono indicazioni di Microsoft sulla disabilitazione dei servizi di sistema in Windows Server 2016 con esperienza Desktop:

<br />

##  <a name="activex-installer-axinstsv"></a>Programma di installazione ActiveX (AxInstSV)

| | |
|---|---|
|   **Descrizione del servizio** |   Fornisce la convalida di controllo dell'Account utente per l'installazione dei controlli ActiveX da Internet e consente la gestione dell'installazione del controllo ActiveX in base alle impostazioni di criteri di gruppo. Questo servizio viene avviato su richiesta e se disabilitata l'installazione dei controlli ActiveX si comporterà in base alle impostazioni del browser predefinito.    |
|   **Nome del servizio**    |   AxInstSV    |
|   **Installazione**    |   Solo con esperienza Desktop    |
|   **StartType**   |   Manual  |
|   **Raccomandazione**  |   OK per disattivare   |
|   **Commenti**    |   OK per disabilitare se non sono necessarie funzionalità |


<br />

## <a name="alljoyn-router-service"></a>AllJoyn Router Service   
| | |
|---|---|
|   **Descrizione del servizio** |   Indirizza i messaggi di AllJoyn per i client di AllJoyn locali. Se questo servizio viene arrestato i client di AllJoyn che non sono presenti i propri router in dotazione sarà possibile eseguire. |
|   **Nome del servizio**    |   AJRouter    |
|   **Installazione**    |   Solo con esperienza Desktop    |
|   **StartType**   |   Manual  |
|   **Raccomandazione**  | Alcuna indicazione       |
|   **Commenti**    |       |
| | |

<br />

## <a name="app-readiness"></a>Conformità delle App
| | |
|---|---|
**Descrizione del servizio** |   Ottiene le app pronte per l'uso la prima volta che un utente accede a questo computer e durante l'aggiunta di nuove app.
**Nome del servizio**    |   AppReadiness
**Installazione**    |   Solo con esperienza Desktop
**StartType**   |   Manual
**Raccomandazione**  |   Non disabilitare
**Commenti**    |   
| | |

<br />

##  <a name="application-identity"></a>Identità dell'applicazione
| | |       
|---|---|   
**Descrizione del servizio** |   Determina e verifica l'identità di un'applicazione. Se si disabilita questo servizio, non potrà AppLocker viene applicata.
**Nome del servizio**    |   AppIDSvc
**Installazione**    |   Sempre installata
**StartType**   |   Manual
**Raccomandazione**  |Alcuna indicazione    
**Commenti**    |   
|||     

<br />

##  <a name="application-information"></a>Informazioni sull'applicazione 
| | |       
|---|---|   
|   **Descrizione del servizio** |   Facilita l'esecuzione di applicazioni interattive con privilegi amministrativi aggiuntivi.  Se questo servizio viene arrestato, gli utenti sarà possibile avviare le applicazioni con i privilegi amministrativi aggiuntivi che possono richiedere per eseguire le attività utente desiderate.
|   **Nome del servizio**    |   Appinfo
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   Supporta l'elevazione dei privilegi di controllo dell'account utente stesso desktop
|||     

<br />

##  <a name="application-layer-gateway-service"></a>Gateway di livello applicazione       
| | |           
|---|---|           
|   **Descrizione del servizio** |   Fornisce supporto per il protocollo di terze parti plug-in per la condivisione connessione Internet
|   **Nome del servizio**    |   ALG
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |Alcuna indicazione    
|   **Commenti**    |   
|||     

<br />

##  <a name="application-management"></a>Gestione applicazioni      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Elabora le richieste di enumerazione, installazione e rimozione di software distribuito tramite criteri di gruppo. Se il servizio è disabilitato, gli utenti sarà possibile installare, rimuovere o enumerazione software distribuito tramite criteri di gruppo. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   AppMgmt
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="appx-deployment-service-appxsvc"></a>Servizi di distribuzione AppX (AppXSVC)       
| | |           
|---|---|
|   **Descrizione del servizio** |   Fornisce un'infrastruttura di supporto per la distribuzione delle applicazioni Store. Questo servizio viene avviato su richiesta e se disabilitato Store applicazioni non verranno distribuite il sistema e potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   AppXSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="auto-time-zone-updater"></a>Aggiornamento del fuso orario automatico           
| | |           
|---|---|           
|   **Descrizione del servizio** |   Imposta automaticamente il fuso orario del sistema.
|   **Nome del servizio**    |   tzautoupdate
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Disabled
|   **Raccomandazione**  |   Già disabilitata
|   **Commenti**    |   
|||         
            
<br />          

## <a name="background-intelligent-transfer-service"></a>Servizio trasferimento intelligente in background          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente di trasferire file in background usando larghezza di banda inattiva. Se il servizio è disabilitato, tutte le applicazioni che dipendono dal bit, ad esempio Windows Update o MSN Explorer, sarà possibile scaricare automaticamente i programmi e altre informazioni.
|   **Nome del servizio**    |   BITS
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          


## <a name="background-tasks-infrastructure-service"></a>Servizio di infrastruttura di attività in background      
| | |           
|---|---|   
|   **Descrizione del servizio** |   Servizio di infrastruttura Windows che controlla quali attività in background può essere eseguito nel sistema.
|   **Nome del servizio**    |   BrokerInfrastructure
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="base-filtering-engine"></a>Motore di filtraggio di base            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Base Filtering Engine (BFE) è un servizio che gestisce i criteri per firewall e Internet Protocol security (IPsec) e implementa il filtraggio in modalità utente. L'arresto o la disattivazione del servizio BFE ridurrà in modo significativo la sicurezza del sistema. Si verificherà anche un comportamento imprevedibile in applicazioni firewall e IPsec management.
|   **Nome del servizio**    |   BFE
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="bluetooth-support-service"></a>Servizio di supporto Bluetooth            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio Bluetooth supporta l'individuazione e associazione dei dispositivi Bluetooth remoti.  L'arresto o la disabilitazione di questo servizio potrebbe essere già installati i dispositivi Bluetooth a non funzionare correttamente e impedire che i nuovi dispositivi venga individuato o associati.
|   **Nome del servizio**    |   bthserv
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   OK per disattivare, se non utilizzato. Un altro meccanismo di disattivazione: https://technet.microsoft.com/library/dd252791.aspx
|||         
            
<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio utente viene usato per gli scenari di piattaforma di dispositivi connessi
|   **Nome del servizio**    |   CDPUserSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Modello di servizio utente
|||         
            
<br />          


##  <a name="certificate-propagation"></a>Propagazione certificati     
| | |           
|---|---|
|   **Descrizione del servizio** |   Copia i certificati utente e i certificati radice da smart card nell'archivio certificati dell'utente corrente, rileva quando viene inserita una smart card nel lettore di smart card e se necessario, installa il minidriver di smart card Plug and Play.
|   **Nome del servizio**    |   CertPropSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="client-license-service-clipsvc"></a>Servizio di licenze client (ClipSVC)        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce un'infrastruttura di supporto per i Microsoft Store. Questo servizio viene avviato su richiesta e se le applicazioni disabilitate acquistata tramite Microsoft Store non funzioneranno correttamente.
|   **Nome del servizio**    |   ClipSVC
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="cng-key-isolation"></a>Isolamento chiavi CNG
| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio isolamento chiavi CNG è ospitato nel processo LSA. Il servizio fornisce l'isolamento dei processi principali per le chiavi private e sulle operazioni di crittografia associate come richiesto da criteri comuni. Il servizio archivia e Usa le chiavi di lunga durate in un processo protetto conformi ai requisiti dei criteri comuni.
|   **Nome del servizio**    |   KeyIso
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="com-event-system"></a>Sistema di eventi COM+       
| | |           
|---|---|       
|   **Descrizione del servizio** |   Supporta System evento servizio di notifica (SENS), che consente la distribuzione automatica di eventi da sottoscrivere componenti modello COM (Component Object). Se il servizio viene arrestato, SENS verrà chiusa e non sarà in grado di fornire le notifiche di accesso e disconnessione. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   EventSystem
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="com-system-application"></a>Applicazione di sistema COM+     
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce la configurazione e registrazione dei componenti di base + modello COM (Component Object). Se il servizio viene arrestato, la maggior parte delle applicazioni COM+: componenti di base non funzionerà correttamente. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   COMSysApp
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="computer-browser"></a>Browser di computer        
| | |           
|---|---|       
|   **Descrizione del servizio** |   Mantiene un elenco aggiornato dei computer nella rete e fornisce questo elenco per i computer designati come i browser. Se questo servizio viene arrestato, questo elenco non verrà aggiornato o mantenuto. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   Browser
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Disabled
|   **Raccomandazione**  |   Già disabilitata
|   **Commenti**    |   
|||         
            
<br />          

## <a name="connected-devices-platform-service"></a>Servizio di piattaforma di dispositivi connessi       
| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio viene usato per gli scenari di dispositivi connessi e vetro universale
|   **Nome del servizio**    |   CDPSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="connected-user-experiences-and-telemetry"></a>Esperienze utente connesse e telemetria     
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio esperienze utente connesse e telemetria Abilita le funzionalità che supportano esperienze utente connesse e nell'applicazione. Inoltre, il servizio gestisce la raccolta basata su eventi e la trasmissione di informazioni di diagnostica e di utilizzo (utilizzate per migliorare l'esperienza e la qualità della piattaforma Windows) quando vengono abilitate la diagnostica e le impostazioni delle opzioni sulla privacy dell'utilizzo con Commenti e diagnostica.
|   **Nome del servizio**    |   DiagTrack
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="contact-data"></a>Dati di contatto        
| | |           
|---|---|       
|   **Descrizione del servizio** |   Dati di contatto degli indici per la ricerca veloce contatto. Se si arresta o si disabilita questo servizio, i contatti potrebbero mancare dai risultati della ricerca.
|   **Nome del servizio**    |   PimIndexMaintenanceSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
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
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="credential-manager"></a>Gestione credenziali           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Offre archiviazione protetta e il recupero delle credenziali per gli utenti, applicazioni e pacchetti di servizi di sicurezza.
|   **Nome del servizio**    |   VaultSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="cryptographic-services"></a>Servizi di crittografia           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce tre servizi di gestione: Servizio Database di catalogo, che verifica le firme dei file di Windows e consente a nuovi programmi da installare; Radice il servizio protetto, che aggiunge e rimuove i certificati di autorità di certificazione radice attendibile da questo computer. e automatico i servizio di aggiornamento, certificato radice che recupera i certificati radice da Windows Update e abilita scenari come SSL. Se questo servizio viene arrestato, i servizi di gestione non funzionerà correttamente. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   CryptSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="data-sharing-service"></a>Servizio di condivisione dei dati         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce la mediazione dei dati tra le applicazioni.
|   **Nome del servizio**    |   DsSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio DCP (raccolta dei dati e pubblicazione) supporta App proprietarie per caricare i dati nel cloud.
|   **Nome del servizio**    |   DcpSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="dcom-server-process-launcher"></a>Utilità di avvio processo Server DCOM         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio DCOMLAUNCH Avvia server COM e DCOM in risposta alle richieste di attivazione dell'oggetto. Se questo servizio viene arrestato o disabilitato, programmi che utilizzano COM o DCOM non funzionerà correttamente. Si consiglia di disporre di DCOMLAUNCH il servizio in esecuzione.
|   **Nome del servizio**    |   DcomLaunch
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  |Alcuna indicazione    
|   **Commenti**    |   
|||         
            
<br />

##  <a name="device-association-service"></a>Servizio associazione dispositivi      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente l'associazione tra il sistema e i dispositivi wireless o cablati.
|   **Nome del servizio**    |   DeviceAssociationService
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          
        
##  <a name="device-install-service"></a>Servizio di installazione di dispositivi      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente a un computer riconoscere e adattarsi alle modifiche dell'hardware con input utente non offrivano. L'arresto o la disabilitazione di questo servizio comporterà l'instabilità del sistema.
|   **Nome del servizio**    |   DeviceInstall
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="device-management-enrollment-service"></a>Servizio registrazione dispositivi di gestione        
| | |           
|---|---|       
|   **Descrizione del servizio** |   Esegue attività di registrazione dispositivi per la gestione dei dispositivi
|   **Nome del servizio**    |   DmEnrollmentSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="device-setup-manager"></a>Gestione guidata installazione dispositivo         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente il rilevamento, download e installazione del software relative al dispositivo. Se questo servizio è disabilitato, i dispositivi possono essere configurati con software obsoleto e potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   DsmSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="devquery-background-discovery-broker"></a>In Background DevQuery individuazione Broker         
| | |           
|---|---|           
|   **Descrizione del servizio** |   Le app individuare i dispositivi con un'attività backgroud
|   **Nome del servizio**    |   DevQueryBroker
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="dhcp-client"></a>Client DHCP          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Registra e aggiorna indirizzi IP e i record DNS per questo computer. Se questo servizio viene arrestato, il computer non riceverà gli indirizzi IP dinamici e gli aggiornamenti DNS. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   DHCP
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="diagnostic-policy-service"></a>Servizio criteri di diagnostica            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio Criteri di diagnostica consente il rilevamento dei problemi e la risoluzione per i componenti di Windows.  Se questo servizio viene arrestato, la diagnostica non funzioneranno più.
|   **Nome del servizio**    |   DPS
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="diagnostic-service-host"></a>Host del servizio diagnostica     
| | |           
|---|---|       
|   **Descrizione del servizio** |   L'Host del servizio diagnostica viene utilizzato dal servizio Criteri di diagnostica per la diagnostica di host che desidera essere eseguiti in un contesto servizio locale.  Se questo servizio viene arrestato, qualsiasi diagnostica che dipende da esso non funzioneranno più.
|   **Nome del servizio**    |   WdiServiceHost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="diagnostic-system-host"></a>Host di sistema di diagnostica           
| | |           
|---|---|       
|   **Descrizione del servizio** |   La diagnostica di sistema Host viene usata dal servizio Criteri di diagnostica per la diagnostica di host che desidera essere eseguiti in un contesto di sistema locale.  Se questo servizio viene arrestato, qualsiasi diagnostica che dipende da esso non funzioneranno più.
|   **Nome del servizio**    |   WdiSystemHost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="distributed-link-tracking-client"></a>Manutenzione collegamenti distribuiti Client            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce i collegamenti tra i file system NTFS all'interno di un computer o tra computer in una rete.
|   **Nome del servizio**    |   TrkWks
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="distributed-transaction-coordinator"></a>Distributed Transaction Coordinator     
| | |           
|---|---|   
|   **Descrizione del servizio** |   Coordina le transazioni che interessano più gestori di risorse, ad esempio database, le code di messaggi e file System. Se questo servizio viene arrestato, le transazioni vengono avrà esito negativo. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   MSDTC
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />  

##  <a name="dmwappushsvc"></a>dmwappushsvc        
| | |           
|---|---|       
|   **Descrizione del servizio** |   Servizio di Routing di messaggi Push WAP
|   **Nome del servizio**    |   dmwappushservice
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Servizio richiesto nei dispositivi client per Intune, MDM e tecnologie analoghe di gestione e per filtro di scrittura unificato. Non è necessaria per il Server.
|||         
            
<br />      

##  <a name="dns-client"></a>Client DNS      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio Client DNS (dnscache) memorizza nella cache i nomi DNS e registra il nome completo del computer per il computer in uso. Se tale servizio viene arrestato, la risoluzione dei nomi DNS verrà eseguita comunque, Tuttavia, i risultati della query DNS non verranno memorizzati e non verrà registrato il nome del computer. Se il servizio viene disattivato, non sarà più possibile avviare i servizi che dipendono esplicitamente da esso.
|   **Nome del servizio**    |   DnsCache
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="downloaded-maps-manager"></a>Gestione mappe scaricato     
| | |           
|---|---|   
|   **Descrizione del servizio** |   Servizio di Windows per l'accesso a maps scaricato. Questo servizio viene avviato su richiesta dall'applicazione l'accesso a scaricato mappe. Se si disabilita questo servizio impedirà le app di accedere a mappe.
|   **Nome del servizio**    |   MapsBroker
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   La disabilitazione delle app di interruzioni che si basano sul servizio. OK per disattivare, se le app non basarsi su di essa
|||         
            
<br />          

## <a name="embedded-mode"></a>Modalità incorporata            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio in modalità incorporata consente scenari correlati alle applicazioni in Background.  Se si disabilita questo servizio impedirà alle applicazioni di sfondo di attivazione.
|   **Nome del servizio**    |   embeddedmode
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="encrypting-file-system-efs"></a>Encrypting File System (EFS)
| | |                   
|---|---|           
|   **Descrizione del servizio** | Fornisce la tecnologia di crittografia file principale utilizzata per archiviare file crittografati nei volumi NTFS. Se questo servizio viene arrestato o disabilitato, le applicazioni sarà possibile accedere ai file crittografati.            
|   **Nome del servizio**  |  EFS            
|   **Installazione**  |  Sempre installata           
|   **StartType**   |  Manual           
|   **Raccomandazione**  | Alcuna indicazione           
|   **Commenti**   |
|||                 
                            
<br />  

## <a name="enterprise-app-management-service"></a>Servizio di gestione di App aziendali            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Abilita gestione delle applicazioni aziendali.
|   **Nome del servizio**    |   EntAppSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="extensible-authentication-protocol"></a>Extensible Authentication Protocol           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio di protocollo EAP (Extensible Authentication) fornisce l'autenticazione di rete in scenari come wireless e cablate 802.1X, VPN e di protezione accesso rete (NAP).  EAP fornisce anche application programming interface (API) utilizzati dai client di accesso di rete, tra cui i client VPN e wireless durante il processo di autenticazione.  Se si disabilita questo servizio, viene impedito l'accesso a reti che richiedono l'autenticazione EAP questo computer.
|   **Nome del servizio**    |   EapHost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="function-discovery-provider-host"></a>Host provider di individuazione funzioni         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio FDPHOST ospita i provider di individuazione di rete individuazione di funzione (domini di errore). Questi provider di domini di errore forniscono servizi di individuazione di rete per i servizi di individuazione protocollo SSDP (Simple) e i servizi Web - protocollo Discovery (WS-D). L'arresto o la disattivazione del servizio FDPHOST disabiliterà individuazione di rete per questi protocolli quando si usa domini di errore. Quando questo servizio è disponibile, servizi di rete usando i domini di errore e basarsi su questi protocolli di individuazione sarà possibile trovare i dispositivi di rete o le risorse.
|   **Nome del servizio**    |   fdPHost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="function-discovery-resource-publication"></a>Pubblicazione risorse per individuazione       
| | |           
|---|---|       
|   **Descrizione del servizio** |   Pubblica questo computer e le risorse collegate al computer in modo che possano essere individuate in rete.  Se questo servizio viene arrestato, non verranno pubblicate non è più risorse di rete e non essere individuati dagli altri computer nella rete.
|   **Nome del servizio**    |   FDResPub
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="geolocation-service"></a>Servizio di georilevazione           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio consente di monitorare la posizione corrente del sistema e gestisce i recinti virtuali (vale a dire una posizione geografica con gli eventi associati).  Se disabilita questo servizio, le applicazioni in grado di utilizzare o ricevere le notifiche per georilevazione o recinti virtuali.
|   **Nome del servizio**    |   lfsvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   La disabilitazione delle app di interruzioni che si basano sul servizio. OK per disattivare, se le app non basarsi su di essa
|||         
            
<br />          

##  <a name="group-policy-client"></a>Client di Criteri di gruppo     
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio è responsabile dell'applicazione delle impostazioni configurate dagli amministratori per il computer e utenti tramite il componente Criteri di gruppo. Se il servizio è disabilitato, non verranno applicate le impostazioni e le applicazioni e i componenti non potrà essere gestiti tramite criteri di gruppo. Tutti i componenti o le applicazioni che dipendono dal componente Criteri di gruppo potrebbero non essere disponibile se il servizio è disabilitato.
|   **Nome del servizio**    |   gpsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          


## <a name="human-interface-device-service"></a>Servizio Human Interface Device            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Abilita e mantiene l'uso dei pulsanti speciali su tastiere, telecomandi e altri dispositivi multimediali. È consigliabile mantenere questo servizio in esecuzione.
|   **Nome del servizio**    |   hidserv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="hv-host-service"></a>Servizio Host HV     
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce un'interfaccia per l'hypervisor Hyper-V fornire i contatori delle prestazioni per ogni partizione per il sistema operativo host.
|   **Nome del servizio**    |   HvHost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Incrementare la produzione per le macchine virtuali guest. Attualmente non utilizzato, ad eccezione di in modo esplicito popolate macchine virtuali, ma verrà usato in Application Guard
|||         
            
<br />          

## <a name="hyper-v-data-exchange-service"></a>Servizio Scambio di dati Hyper-V         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Rende disponibile un meccanismo per lo scambio di dati tra la macchina virtuale e il sistema operativo in esecuzione nel computer fisico.
|   **Nome del servizio**    |   vmickvpexchange
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />      

## <a name="hyper-v-guest-service-interface"></a>Interfaccia servizio guest Hyper-V          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce un'interfaccia per l'host Hyper-V interagire con servizi specifici in esecuzione nella macchina virtuale.
|   **Nome del servizio**    |   vmicguestinterface
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Servizio Arresto guest Hyper-V           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce un meccanismo per arrestare il sistema operativo della macchina virtuale dalle interfacce di gestione nel computer fisico.
|   **Nome del servizio**    |   vmicshutdown
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />          
    
## <a name="hyper-v-heartbeat-service"></a>Servizio Heartbeat Hyper-V             
| | |           
|---|---|           
|   **Descrizione del servizio** |   Monitora lo stato di questa macchina virtuale segnalando un heartbeat a intervalli regolari. Questo servizio consente di identificare le macchine virtuali in esecuzione che non rispondono.
|   **Nome del servizio**    |   vmicheartbeat
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
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
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Servizio Virtualizzazione Desktop remoto Hyper-V            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce una piattaforma per la comunicazione tra la macchina virtuale e il sistema operativo in esecuzione nel computer fisico.
|   **Nome del servizio**    |   vmicrdv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />          

## <a name="hyper-v-time-synchronization-service"></a>Servizio Sincronizzazione ora Hyper-V         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di sincronizzare l'ora di sistema della macchina virtuale con l'ora di sistema del computer fisico.
|   **Nome del servizio**    |   vmictimesync
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Richiedente Copia Shadow del volume Hyper-V         
| | |           
|---|---|           
|   **Descrizione del servizio** |   Coordina le comunicazioni necessarie per usare il servizio Copia Shadow del Volume per eseguire il backup delle applicazioni e i dati in questa macchina virtuale dal sistema operativo nel computer fisico.
|   **Nome del servizio**    |   vmicvss
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Vedere HvHost
|||         
            
<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>Moduli di impostazione chiavi IPSec IKE e Auth-IP          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio IKEEXT ospita i Internet Key Exchange (IKE) e Authenticated Internet Protocol (AuthIP) moduli di impostazione chiavi. Questi moduli per le chiavi vengono usati per l'autenticazione e lo scambio di chiave in Internet Protocol security (IPsec). L'arresto o la disattivazione del servizio IKEEXT disabiliterà IKE e AuthIP scambio delle chiavi con computer peer. IPsec viene in genere configurato per l'utilizzo; AuthIP o IKE Pertanto, l'arresto o la disattivazione del servizio IKEEXT potrebbe comportare un errore di IPsec e potrebbe compromettere la sicurezza del sistema. Si consiglia di avere in esecuzione il servizio IKEEXT.
|   **Nome del servizio**    |   IKEEXT
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |    
|||         
            
<br />          

## <a name="interactive-services-detection"></a>Rilevamento servizi interattivi           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Attiva la notifica dell'input utente per i servizi interattivi, che consente l'accesso alle finestre di dialogo create dai servizi interattivi quando questi vengono visualizzati. Se questo servizio viene arrestato, le notifiche di nuove finestre di dialogo interattivo servizio smette di funzionare e potrebbe non essere disponibile l'accesso alle finestre di dialogo servizio interattivo. Se il servizio è disabilitato, l'accesso alle finestre di dialogo nuovo servizio interattivo sia le notifiche di smetteranno di funzionare.
|   **Nome del servizio**    |   UI0Detect
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />  

## <a name="internet-connection-sharing-ics"></a>Condivisione connessione Internet (ICS)            
| | |           
|---|---|           
|   **Descrizione del servizio** |   Fornisce il protocollo NAT, indirizzamento, servizi di prevenzione di risoluzione e/o delle intrusioni nome per una rete domestica o di piccole dimensioni.
|   **Nome del servizio**    |   SharedAccess
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Obbligatorio per i client utilizzati come hotspot Wi-Fi e inoltre su entrambe le estremità di proiezione Miracast. Condivisione connessione Internet può essere bloccata con impostazione oggetto Criteri di gruppo, "Impedisci l'utilizzo di Condivisione connessione Internet nella rete del dominio DNS"
|||         
            
<br />          

## <a name="ip-helper"></a>Helper IP            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce connettività tunnel utilizzando le tecnologie di transizione IPv6 (6to4, Teredo, ISATAP e porta Proxy) e IP-HTTPS. Se questo servizio viene arrestato, il computer non sarà necessario che queste tecnologie offrono i vantaggi di connettività avanzata.
|   **Nome del servizio**    |   iphlpsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          


##  <a name="ipsec-policy-agent"></a>Agente criteri IPsec      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Internet Protocol security (IPsec) supporta l'autenticazione peer a livello di rete, l'autenticazione dell'origine dati, l'integrità dei dati, la riservatezza (crittografia) e protezione di riproduzione.  Questo servizio impone i criteri IPsec creati tramite lo snap-in Criteri di protezione IP o lo strumento da riga di comando "netsh ipsec".  Se si arresta il servizio, si potrebbero verificarsi problemi di connettività di rete se i criteri richiedono che le connessioni utilizzano IPsec.  Inoltre, la gestione remota di Windows Firewall non è disponibile quando il servizio viene interrotto.
|   **Nome del servizio**    |   PolicyAgent
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />

##  <a name="kdc-proxy-server-service-kps"></a>Servizio Server Proxy KDC (KPS)      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Servizio Server Proxy KDC viene eseguito sui server perimetrali al proxy Kerberos messaggi di protocollo ai controller di dominio nella rete aziendale.
|   **Nome del servizio**    |   KPSSVC
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione    
|   **Commenti**    |   
|||         
            
<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>KtmRm per Distributed Transaction Coordinator            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Coordina le transazioni tra Distributed Transaction Coordinator (MSDTC) e le transazioni Manager (kernel). Se non è più necessaria, è consigliabile che questo servizio rimane arrestato. Se necessario, MSDTC sia gestione transazioni kernel avvierà il servizio automaticamente. Se questo servizio è disabilitato, qualsiasi transazione MSDTC l'interazione con Gestione risorse di un Kernel avrà esito negativo e non sarà possibile avviare qualsiasi servizi che dipendono esplicitamente da esso.
|   **Nome del servizio**    |   KtmRm
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />

##  <a name="link-layer-topology-discovery-mapper"></a>Mapper individuazione topologia livelli di collegamento        
| | |       
|---|---|       
|   **Descrizione del servizio** |   Crea una mappa di rete, costituito da computer e informazioni sulla topologia (connettività) di dispositivo e i metadati che descrivono ogni PC e dispositivi.  Se il servizio è disabilitato, la mappa di rete non funzionerà correttamente.
|   **Nome del servizio**    |   lltdsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   OK per disabilitare se senza dipendenze nella mappa di rete
|||         
            
<br />

## <a name="local-session-manager"></a>Gestione sessioni locali                    
| | |                   
|---|---|   
|   **Descrizione del servizio** |   Servizio di Windows di core che gestisce le sessioni utente locale. L'arresto o la disabilitazione di questo servizio comporterà l'instabilità del sistema.    
|   **Nome del servizio**    |   LSM |
|   **Installazione**    |   Sempre installata    |
|   **StartType**   |   Automatico   |
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||                 
                    
<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Agente di raccolta Standard Microsoft (R) diagnostica dell'Hub         
| | |           
|---|---|           
|   **Descrizione del servizio** |   Servizio di Windows di core che gestisce le sessioni utente locale. L'arresto o la disabilitazione di questo servizio comporterà l'instabilità del sistema.
|   **Nome del servizio**    |   LSM
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          
            
## <a name="microsoft-account-sign-in-assistant"></a>Assistente per l'accesso Account Microsoft          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Abilita accesso dell'utente tramite i servizi di identità di account Microsoft. Se questo servizio viene arrestato, gli utenti non saranno in grado di accedere al computer tramite il proprio account Microsoft.
|   **Nome del servizio**    |   wlidsvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Microsoft Accounts sono n/d in Windows Server
|||         
            
<br />          

##  <a name="microsoft-app-v-client"></a>Microsoft App-V Client      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce gli utenti di App-V e le applicazioni virtuali
|   **Nome del servizio**    |   AppVClient
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Disabled
|   **Raccomandazione**  |   Già disabilitata
|   **Commenti**    |   
|||         
            
<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Servizio iniziatore iSCSI Microsoft            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce le sessioni iSCSI (Internet SCSI) da questo computer per i dispositivi di destinazione iSCSI remota. Se questo servizio viene arrestato, in questo computer non sarà in grado di accedere alle destinazioni iSCSI. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   MSiSCSI
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   I dati di diagnostica indicano che questo viene utilizzato sul client, nonché server. Non offre alcun vantaggio per la disabilitazione di questa.
|||         
            
<br />          

## <a name="microsoft-passport"></a>Microsoft Passport           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce l'isolamento dei processi per le chiavi di crittografia utilizzato per l'autenticazione al provider di identità associato dell'utente. Se questo servizio è disabilitato, tutte le usa e gestione di queste chiavi non saranno disponibili, che include computer accesso e dell'accesso single sign-in per App e siti Web. Questo servizio viene avviato e si arresta automaticamente. È consigliabile non riconfigurare il servizio.
|   **Nome del servizio**    |   NgcSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Necessario per gli accessi PIN/Hello, che non è supportati nel Server
|||         
            
<br />          

## <a name="microsoft-passport-container"></a>Contenitore di Microsoft Passport         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce le chiavi di identità utente locale usate per l'autenticazione utente per i provider di identità, nonché le smart card virtuali TPM. Se il servizio è disabilitato, le chiavi di identità utente locale e smart card virtuali TPM non saranno accessibili. È consigliabile non riconfigurare il servizio.
|   **Nome del servizio**    |   NgcCtnrSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Provider di copie Shadow Software Microsoft          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di gestire copie shadow dei volumi basati su software impiegate dal servizio Copia Shadow del Volume. Se questo servizio viene arrestato, copie shadow dei volumi basati su software non possono essere gestite. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   swprv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="microsoft-storage-spaces-smp"></a>SMP spazi di archiviazione di Microsoft         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Servizio host per il provider di gestione di spazi di archiviazione di Microsoft. Se questo servizio viene arrestato o disabilitato, non possono essere gestiti gli spazi di archiviazione.
|   **Nome del servizio**    |   smphost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Gestione dell'archiviazione API esito negativo senza questo servizio. Esempio: "Get-WmiObject -class MSFT_Disk -Namespace Root\Microsoft\Windows\Storage".
|||         
            
<br />          

## <a name="nettcp-port-sharing-service"></a>Net.Tcp Port Sharing Service         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di condividere le porte TCP tramite il protocollo NET. TCP.
|   **Nome del servizio**    |   NetTcpPortSharing
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Disabled
|   **Raccomandazione**  |   Già disabilitata
|   **Commenti**    |   
|||         
            
<br />          

## <a name="netlogon"></a>Accesso rete         
| | |           
|---|---|           
|   **Descrizione del servizio** |   Gestisce un canale sicuro tra computer e il controller di dominio per l'autenticazione degli utenti e servizi. Se questo servizio viene arrestato, il computer potrebbe non autenticare utenti e servizi e il controller di dominio non è possibile registrare i record DNS. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   Accesso rete
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="network-connection-broker"></a>Gestore di connessione di rete            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Connessioni di Broker che consentono alle app di Microsoft Store ricevere le notifiche da internet.
|   **Nome del servizio**    |   NcbService
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="network-connections"></a>Connessioni di rete         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce gli oggetti nella cartella di rete e le connessioni remote, in cui è possibile visualizzare sia rete locale e le connessioni remote.
|   **Nome del servizio**    |   Netman
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="network-connectivity-assistant"></a>Assistente connettività di rete      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce la notifica di stato di DirectAccess per i componenti dell'interfaccia utente
|   **Nome del servizio**    |   NcaSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />  

##  <a name="network-list-service"></a>Servizio elenco reti        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Identifica le reti a cui il computer è connesso, raccoglie e archivia le proprietà per queste reti e notifica alle applicazioni quando queste proprietà vengono modificate.
|   **Nome del servizio**    |   netprofm
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="network-location-awareness"></a>Riconoscimento presenza in rete           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Raccoglie e archivia le informazioni di configurazione per la rete e comunica ai programmi quando queste informazioni vengono modificate. Se questo servizio viene arrestato, le informazioni di configurazione potrebbero non essere disponibile. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   NlaSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="network-setup-service"></a>Servizio di configurazione di rete       
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio di configurazione di rete gestisce l'installazione del driver di rete e consente la configurazione delle impostazioni di rete di basso livello.  Se questo servizio viene arrestato, eventuali installazioni di driver che sono in corso possono essere annullate.
|   **Nome del servizio**    |   NetSetupSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="network-store-interface-service"></a>Servizio di interfaccia di rete Store      
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio recapita le notifiche di rete (ad esempio interfaccia aggiunta/eliminazione e così via) per i client in modalità utente. L'arresto del servizio causerà la perdita della connettività di rete. Se il servizio è disabilitato, tutti gli altri servizi che dipendono esplicitamente da questo servizio avrà esito negativo iniziare.
|   **Nome del servizio**    |   nsi
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="offline-files"></a>File offline            
| | |           
|---|---|       
|   **Descrizione del servizio** |   I file Offline servizio esegue le attività di manutenzione nella cache dei file Offline, risponde agli eventi di accesso e disconnessione dell'utente, implementa gli elementi interni dell'API pubblica e invia eventi interessanti da quelli interessati a file Offline le attività e modifiche nello stato della cache.
|   **Nome del servizio**    |   CscService
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Disabled
|   **Raccomandazione**  |   Già disabilitata
|   **Commenti**    |   
|||         
            
<br />          

## <a name="optimize-drives"></a>Ottimizza unità          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente di eseguire in modo più efficiente, ottimizzando al tempo i file nelle unità di archiviazione nel computer.
|   **Nome del servizio**    |   defragsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />

## <a name="performance-counter-dll-host"></a>Host DLL del contatore delle prestazioni         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente agli utenti remoti e i processi a 64 bit eseguire query sui contatori delle prestazioni forniti dalla DLL a 32 bit. Se questo servizio viene arrestato, solo gli utenti locali e i processi a 32 bit sarà in grado di eseguire query sui contatori delle prestazioni forniti dalla DLL a 32 bit.
|   **Nome del servizio**    |   PerfHost
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione    
|   **Commenti**    |   
|||         
            
<br />          

## <a name="performance-logs--alerts"></a>Gli avvisi e registri di prestazioni            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Raccoglie gli avvisi e registri di prestazioni i dati sulle prestazioni dal computer locale o remoto in base ai parametri di pianificazione preconfigurata, quindi scrive i dati in un log o attiva un avviso. Se questo servizio viene arrestato, non verranno raccolti informazioni sulle prestazioni. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   pla
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="phone-service"></a>Servizio telefonico       
| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce lo stato di telefonia sul dispositivo
|   **Nome del servizio**    |   PhoneSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Usato da app moderne VoIP
|||         
            
<br />          

##      <a name="plug-and-play"></a>Servizio Plug and Play       
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente a un computer riconoscere e adattarsi alle modifiche dell'hardware con input utente non offrivano. L'arresto o la disabilitazione di questo servizio comporterà l'instabilità del sistema.
|   **Nome del servizio**    |   PlugPlay
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="portable-device-enumerator-service"></a>Servizio dispositivi portatili di enumeratore           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Applica criteri di gruppo per i dispositivi di archiviazione di massa rimovibili. Consente alle applicazioni, ad esempio Windows Media Player e Image importazione guidata di trasferire e sincronizzare contenuto con i dispositivi di archiviazione di massa rimovibili.
|   **Nome del servizio**    |   WPDBusEnum
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="power"></a>Alimentazione            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce i criteri di risparmio energia e recapito delle notifiche dei criteri risparmio energia.
|   **Nome del servizio**    |   Alimentazione
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="print-spooler"></a>Spooler di stampa            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio effettua lo spooling dei processi di stampa e gestisce l'interazione con la stampante.  Se disabilita questo servizio, non sarà in grado di stampare o vedere le stampanti.
|   **Nome del servizio**    |   Spooler
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   OK per disabilitare se non è un server di stampa o un controller di dominio
|   **Commenti**    |   In un controller di dominio, l'installazione del ruolo del controller di dominio viene aggiunto un thread per il servizio spooler di che è responsabile dell'esecuzione stampa l'operazione di eliminazione: rimuovere gli oggetti di coda di stampa non aggiornati da Active Directory.  Se il servizio spooler non è in esecuzione in almeno un controller di dominio in ogni sito, quindi Active Directory non è in grado di rimuovere le code precedente che non esistono più. https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         
            
<br />          

##  <a name="printer-extensions-and-notifications"></a>Le notifiche e le estensioni della stampante        
| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio consente di aprire le finestre di dialogo di stampa personalizzata e gestisce le notifiche da un server di stampa remoto o una stampante. Se disabilita questo servizio, sarà possibile visualizzare le estensioni della stampante o le notifiche.
|   **Nome del servizio**    |   PrintNotify
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disabilitare se non è un server di stampa
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>Segnalazioni di problemi e supporto per il pannello di controllo di soluzioni     
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio fornisce supporto per la visualizzazione, l'invio e l'eliminazione delle segnalazioni di problemi a livello di sistema per il pannello di controllo segnalazioni di problemi e soluzioni.
|   **Nome del servizio**    |   wercplsupport
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="program-compatibility-assistant-service"></a>Program Compatibility Assistant servizio     
| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio fornisce il supporto per i Program Compatibility Assistant principali (PCA).  Analisi in componenti principali consente di monitorare applicazioni installate ed eseguite dall'utente e rileva i problemi di compatibilità noti. Se questo servizio viene arrestato, analisi in componenti principali non funzionerà correttamente.
|   **Nome del servizio**    |   PcaSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="quality-windows-audio-video-experience"></a>Servizio audio/video Windows di qualità      
| | |           
|---|---|   
|   **Descrizione del servizio** |   Quality Windows Audio Video Experience (qWave) è una piattaforma per Audio-Video (AV) lo streaming di applicazioni in reti domestiche IP di rete. qWave migliora AV lo streaming di prestazioni e affidabilità, garantendo rete quality of service (QoS) per le applicazioni antivirus. Fornisce i meccanismi per il controllo di ammissione, Esegui monitoraggio in tempo e applicazione, commenti sull'applicazione e assegnazione delle priorità del traffico.
|   **Nome del servizio**    |   QWAVE
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Servizio QoS sul lato client
|||         
            
<br />          

##      <a name="radio-management-service"></a>Servizio di gestione radio        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Opzione di gestione e del servizio in modalità aereo
|   **Nome del servizio**    |   RmSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="remote-access-auto-connection-manager"></a>Gestione connessione automatica di accesso remoto            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Crea una connessione a una rete remota ogni volta che un programma fa riferimento a un nome DNS o NetBIOS o l'indirizzo remoto.
|   **Nome del servizio**    |   RasAuto
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="remote-access-connection-manager"></a>Gestione connessione di accesso remoto         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce le connessioni remote e virtuale a rete privata (VPN) da questo computer a Internet o ad altre reti remote. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   RasMan
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="remote-desktop-configuration"></a>Configurazione di Desktop remoto         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Servizio di configurazione di Desktop remoto (RDC) è responsabile per tutti i Servizi Desktop remoto e Desktop remoto correlate alla configurazione e le attività di manutenzione di sessione che richiedono il contesto di sistema. Sono inclusi cartelle temporanee per sessione, i temi desktop remoto e i certificati di desktop remoto.
|   **Nome del servizio**    |   SessionEnv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="remote-desktop-services"></a>Servizi Desktop remoto          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente agli utenti di connettersi in modo interattivo a un computer remoto. Desktop remoto e Server Host sessione Desktop remoto dipendono da questo servizio.  Per impedire l'utilizzo remoto del computer, deselezionare le caselle di controllo nella scheda connessione remota dell'elemento del Pannello di controllo di proprietà del sistema.
|   **Nome del servizio**    |   TermService
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="remote-desktop-services-usermode-port-redirector"></a>Redirector porta UserMode di Servizi Desktop remoto        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente il reindirizzamento delle stampanti/unità/porte per le connessioni RDP
|   **Nome del servizio**    |   UmRdpService
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Supporta i reindirizzamenti sul lato server della connessione.
|||         
            
<br />          

## <a name="remote-procedure-call-rpc"></a>RPC (Remote Procedure Call)          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio RPCSS è Gestione controllo servizi per i server COM e DCOM. Esegue le richieste di attivazioni di oggetti, risoluzioni di utilità di esportazione di oggetto e garbage collection distribuito per i server COM e DCOM. Se questo servizio viene arrestato o disabilitato, programmi che utilizzano COM o DCOM non funzionerà correttamente. Si consiglia di avere l'esecuzione del servizio RPCSS.
|   **Nome del servizio**    |   RpcSs
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>Remote Procedure Call (RPC) localizzatore             
| | |               
|---|---|   
|   **Descrizione del servizio** |   In Windows 2003 e versioni precedenti di Windows, il servizio Localizzatore RPC Remote Procedure Call () gestisce il database del servizio RPC nome. In Windows Vista e versioni successive di Windows, il servizio non fornisce alcuna funzionalità ed è presente per compatibilità delle applicazioni.   |
|   **Nome del servizio**    |   RpcLocator  |
|   **Installazione**    |   Solo con esperienza Desktop    |
|   **StartType**   |   Manual  |
|   **Raccomandazione**  | Alcuna indicazione   |
|   **Commenti**    |       |
|||             
                
<br />              

## <a name="remote-registry"></a>Registro di sistema remoto          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente agli utenti remoti di modificare le impostazioni del Registro di sistema del computer. Se questo servizio viene arrestato, il Registro di sistema può essere modificata solo da utenti in questo computer. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   RemoteRegistry
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="resultant-set-of-policy-provider"></a>Set di Provider di criteri risultante            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce un servizio di rete che elabora le richieste per simulare l'applicazione delle impostazioni di criteri di gruppo per un utente di destinazione o un computer in diverse situazioni e calcola le impostazioni di criteri.
|   **Nome del servizio**    |   RSoPProv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |Alcuna indicazione    
|   **Commenti**    |   
|||         
            
<br />          

## <a name="routing-and-remote-access"></a>Routing e Accesso remoto            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Offre servizi di routing per le aziende in ambienti di rete WAN e di rete locale.
|   **Nome del servizio**    |   RemoteAccess
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Disabled
|   **Raccomandazione**  |   Già disabilitata
|   **Commenti**    |   Già disabilitata
|||         
            
<br />          

## <a name="rpc-endpoint-mapper"></a>Agente mapping endpoint RPC          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Risolve gli identificatori di interfacce RPC a endpoint di trasporto. Se questo servizio viene arrestato o disabilitato, i programmi usando i servizi RPC Remote Procedure Call () non funzionerà correttamente.
|   **Nome del servizio**    |   RpcEptMapper
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="secondary-logon"></a>Accesso secondario     
| | |           
|---|---|       
|   **Descrizione del servizio** |   Abilita l'avvio di processi con credenziali alternative. Se questo servizio viene arrestato, questo tipo di accesso sarà disponibile. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   seclogon
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>Secure Socket Tunneling Protocol Service            
| | |               
|---|---|       
|   **Descrizione del servizio** |   Fornisce il supporto per il Tunneling protocollo SSTP (Secure Socket) per connettersi a computer remoti tramite VPN. Se il servizio è disabilitato, gli utenti non saranno in grado di utilizzare SSTP per accedere a server remoti.    |
|   **Nome del servizio**    |   SstpSvc |
|   **Installazione**    |   Sempre installata    |
|   **StartType**   |   Manual  |
|   **Raccomandazione**  |   Non disabilitare  |
|   **Commenti**    |   La disabilitazione di interruzioni di RRAS   |
|||             
                
<br />              

## <a name="security-accounts-manager"></a>Gestione degli account di sicurezza            
| | |           
|---|---|       
|   **Descrizione del servizio** |   L'avvio di questo servizio segnala ad altri servizi che è pronto per accettare le richieste di gestione di account di sicurezza (SAM).  Se si disabilita questo servizio impedirà ad altri servizi nel sistema da visualizzare una notifica quando il modulo SAM è pronto, che potrebbe causare a sua volta tali servizi non vengono avviati in modo corretto. Questo servizio non deve essere disabilitato.
|   **Nome del servizio**    |   SamSs
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Non disabilitare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="sensor-data-service"></a>Servizio dati del sensore  
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce i dati da un'ampia gamma di sensori
|   **Nome del servizio**    |   SensorDataService
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />  

## <a name="sensor-monitoring-service"></a>Monitoraggio servizio del sensore            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di monitorare diversi sensori allo scopo di esporre i dati e adattare allo stato di sistema e utente.  Se questo servizio viene arrestato o disabilitato, la luminosità dello schermo non si adatta alle condizioni di illuminazione. L'arresto del servizio può influire sulle altre funzionalità del sistema e anche le funzionalità.
|   **Nome del servizio**    |   SensrSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          
        
## <a name="sensor-service"></a>Servizio del sensore           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Un servizio per i sensori che gestisce la funzionalità di diversi sensori. Gestisce l'orientamento di dispositivo semplice (SDO) e la cronologia per i sensori. Carica il sensore SDO che segnala le modifiche apportate orientamento di dispositivo.  Se questo servizio viene arrestato o disabilitato, non verrà caricato il sensore SDO e pertanto non si verificherà rotazione automatica. Raccolta di cronologie dei sensori verrà arrestato.
|   **Nome del servizio**    |   SensorService
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="server"></a>Server           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Supporta file, stampa e pipe con nome di condivisione in rete per questo computer. Se questo servizio viene arrestato, queste funzioni non saranno disponibile. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   LanmanServer
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Necessario per la gestione remota, $ IPC, la condivisione file SMB
|||         
            
<br />          

## <a name="shell-hardware-detection"></a>Rilevamento Hardware shell             
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce notifiche degli eventi di AutoPlay hardware.
|   **Nome del servizio**    |   ShellHWDetection
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="smart-card"></a>Smart card           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce l'accesso alle smart card letto da questo computer. Se questo servizio viene arrestato, questo computer sarà in grado di leggere le smart card. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   SCardSvr
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Disabled
|   **Raccomandazione**  |   Già disabilitata
|   **Commenti**    |   
|||         
            
<br />          

## <a name="smart-card-device-enumeration-service"></a>Servizio di enumerazione dispositivo Smart Card                    
| | |               
|---|---|       
|   **Descrizione del servizio** |   Crea nodi dispositivo del software per tutti i lettori di smart card accessibile a una determinata sessione. Se il servizio è disabilitato, APIs WinRT non sarà in grado di enumerare i lettori di smart card.   |
|   **Nome del servizio**    |   ScDeviceEnum    |
|   **Installazione**    |   Sempre installata    |
|   **StartType**   |   Manual  |
|   **Raccomandazione**  |   OK per disattivare   |
|   **Commenti**    |   Necessari quasi esclusivamente per le app di WinRT    |
|||             
                
<br />              

## <a name="smart-card-removal-policy"></a>Criterio di rimozione della Smart Card        
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente al sistema siano configurati per bloccare il desktop utente al momento della rimozione della smart card.
|   **Nome del servizio**    |   SCPolicySvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="snmp-trap"></a>Trap SNMP            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Riceve i messaggi trap generati dagli agenti di Simple Network Management Protocol (SNMP) locali o remoti e inoltra i messaggi per i programmi di gestione SNMP in esecuzione nel computer. Se questo servizio viene arrestato, programmi basati su SNMP in questo computer non riceverà messaggi trap SNMP. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   SNMPTRAP
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="software-protection"></a>Protezione da software             
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente il download, installazione e l'imposizione delle licenze digitali per Windows e Windows le applicazioni. Se il servizio è disabilitato, il sistema operativo e le applicazioni con licenza possono essere eseguite in una modalità di notifica. Si consiglia di non disabilitare la protezione da Software.
|   **Nome del servizio**    |   sppsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="special-administration-console-helper"></a>Helper di Console di amministrazione speciale        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente agli amministratori di accedere in remoto a un prompt dei comandi usando servizi di gestione emergenze.
|   **Nome del servizio**    |   sacsvr
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="spot-verifier"></a>Campione Verifier            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Verifica i potenziali file system riscontrati.
|   **Nome del servizio**    |   svsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="ssdp-discovery"></a>SSDP Discovery           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente di individuare i dispositivi di rete e i servizi che usano il protocollo di individuazione vengono usati SSDP, ad esempio i dispositivi UPnP. Annuncia anche i dispositivi vengono usati SSDP e servizi in esecuzione nel computer locale. Se questo servizio viene arrestato, non verranno individuati i dispositivi basati su protocollo SSDP. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   SSDPSRV
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="state-repository-service"></a>Repository stato servizio         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce supporto dell'infrastruttura necessaria per il modello di applicazione.
|   **Nome del servizio**    |   StateRepository
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="still-image-acquisition-events"></a>Gli eventi di acquisizione di acquisizione immagini
| | |           
|---|---|   
|   **Descrizione del servizio** |   Avvia le applicazioni associate comunque agli eventi di acquisizione immagine.
|   **Nome del servizio**    |   WiaRpc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />  

## <a name="storage-service"></a>Servizio di archiviazione          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce servizi di attivazione per le impostazioni di archiviazione e l'espansione di archiviazione esterna
|   **Nome del servizio**    |   StorSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="storage-tiers-management"></a>Gestione livelli di archiviazione        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente di ottimizzare la posizione dei dati in livelli di archiviazione su tutti gli spazi di archiviazione a livelli nel sistema.
|   **Nome del servizio**    |   TieringEngineService
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="superfetch"></a>SuperFetch          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce e migliora le prestazioni del sistema nel corso del tempo.
|   **Nome del servizio**    |   SysMain
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="sync-host"></a>Host di sincronizzazione            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio consente di sincronizzare la posta, contatti, calendario e vari altri dati utente. Posta elettronica e ad altre applicazioni dipendenti da questa funzionalità non funzionerà correttamente quando il servizio non è in esecuzione.
|   **Nome del servizio**    |   OneSyncSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Modello di servizio utente
|||         
            
<br />          

## <a name="system-event-notification-service"></a>Servizio di notifica di eventi di sistema            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di monitorare gli eventi di sistema e notifica ai sottoscrittori di COM+ Event System di questi eventi.
|   **Nome del servizio**    |   SENS
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="system-events-broker"></a>Gestore di eventi di sistema             
| | |           
|---|---|       
|   **Descrizione del servizio** |   Coordina l'esecuzione di attività in background per applicazione WinRT. Se questo servizio viene arrestato o disabilitato, attività in background potrebbero non essere attivate.
|   **Nome del servizio**    |   SystemEventsBroker
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Nonostante il fatto che la relativa descrizione implica che è solo per le app di WinRT, è necessaria per utilità di pianificazione, il servizio di infrastruttura di Service broker e altri componenti interni.
|||         
            
<br />          

## <a name="task-scheduler"></a>Utilità di pianificazione           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente all'utente di configurare e pianificare le attività automatizzate in questo computer. Il servizio ospita anche più attività di sistema critici di Windows. Se questo servizio viene arrestato o disabilitato, queste attività non verranno eseguite agli intervalli previsti volte. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   Pianificazione
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="tcpip-netbios-helper"></a>NetBIOS Helper di TCP/IP            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce supporto per NetBIOS su TCP/IP (NetBT) servizio e la risoluzione dei nomi NetBIOS per i client nella rete, pertanto consentendo agli utenti di condividere file, stampa e accedere alla rete. Se questo servizio viene arrestato, queste funzioni potrebbero essere non disponibile. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   lmhosts
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="telephony"></a>Telephony           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce il supporto di API di telefonia (TAPI) per i programmi che consentono di controllare i dispositivi di telefonia nel computer locale e, tramite la LAN, nei server che eseguono anche il servizio.
|   **Nome del servizio**    |   TapiSrv
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   La disabilitazione di interruzioni di RRAS
|||         
            
<br />          

## <a name="themes"></a>Temi           
| | |           
|---|---|
|   **Descrizione del servizio** |   Fornisce gestione temi dell'esperienza utente.
|   **Nome del servizio**    |   Temi
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Non è possibile impostare i temi di accessibilità quando il servizio viene disabilitato
|||         
            
<br />  

## <a name="tile-data-model-server"></a>Dati modello server delle sezioni           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Server delle sezioni per aggiornamenti di riquadri.
|   **Nome del servizio**    |   tiledatamodelsvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Start menu interruzioni se il servizio è disabilitato
|||         
            
<br />          

##  <a name="time-broker"></a>Broker di tempo     
| | |           
|---|---|       
|   **Descrizione del servizio** |   Coordina l'esecuzione di attività in background per applicazione WinRT. Se questo servizio viene arrestato o disabilitato, attività in background potrebbero non essere attivate.
|   **Nome del servizio**    |   TimeBrokerSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Nonostante il fatto che la relativa descrizione implica che è solo per le app di WinRT, è necessaria per utilità di pianificazione, il servizio di infrastruttura di Service broker e altri componenti interni.
|||         
            
<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>Tastiera touchscreen e della grafia pannello servizio         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Abilita la tastiera touchscreen e della grafia pannello funzionalità penna e input penna
|   **Nome del servizio**    |   TabletInputService
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>Aggiornare il servizio agente di orchestrazione per l'aggiornamento di Windows           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce gli aggiornamenti di Windows. Se arrestato, i dispositivi non sarà in grado di scaricare e installare gli aggiornamenti più recenti.
|   **Nome del servizio**    |   UsoSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Descrizione del servizio non è presente in v1607; Aggiornamento di Windows (incluso Windows Server Update Services) dipende da questo servizio.
|||         
            
<br />          

## <a name="upnp-device-host"></a>Dispositivi UPnP Host         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente ai dispositivi UPnP deve essere ospitato in questo computer. Se questo servizio viene arrestato, tutti i dispositivi UPnP ospitati non funzionerà più e non altri dispositivi ospitati possono essere aggiunti. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   upnphost
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="user-access-logging-service"></a>Servizio registrazione accessi utente          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio registra le richieste di accesso client univoci, sotto forma di indirizzi IP e nomi utente, dei prodotti installati e dei ruoli nel server locale. Queste informazioni possono eseguire query, tramite Powershell, gli amministratori di dover quantificare richiesta client del software del server per la gestione delle licenze CAL (Client Access License) non in linea. Se il servizio è disabilitato, le richieste client non verranno registrate e non potrà essere recuperate tramite query di Powershell. L'arresto del servizio non avrà effetto su query dei dati cronologici (vedere la documentazione per la procedura per eliminare i dati cronologici di supporto). L'amministratore di sistema locale è necessario consultare, la propria, condizioni di licenza di Windows Server per determinare il numero di licenze CAL necessari per il software del server avere la licenza in modo appropriato; Utilizzare il servizio registrazione accesso utenti e i dati non altera l'obbligo.
|   **Nome del servizio**    |   UALSVC
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="user-data-access"></a>Accesso ai dati utente        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce l'accesso App ai dati utente strutturato, incluse le informazioni di contatto, calendari, i messaggi e altro contenuto. Se si arresta o si disabilita questo servizio, le app che usano questi dati potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   UserDataSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Modello di servizio utente
|||         
            
<br />          

## <a name="user-data-storage"></a>Archiviazione dei dati utente            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce l'archiviazione dei dati utente strutturato, incluse le informazioni di contatto, calendari, i messaggi e altro contenuto. Se si arresta o si disabilita questo servizio, le app che usano questi dati potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   UnistoreSvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Modello di servizio utente
|||         
            
<br />          

## <a name="user-experience-virtualization-service"></a>Servizio di virtualizzazione esperienza utente           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce supporto per applicazioni e impostazioni del sistema operativo mobile
|   **Nome del servizio**    |   UevAgentService
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Disabled
|   **Raccomandazione**  |   Già disabilitata
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="user-manager"></a>Gestione utenti        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Responsabile utente fornisce i componenti runtime necessari per l'interazione con più utenti.  Se questo servizio viene arrestato, alcune applicazioni potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   UserManager
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="user-profile-service"></a>Servizio profili utente         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio è responsabile del caricamento e scaricamento di profili utente mobili. Se questo servizio viene arrestato o disabilitato, gli utenti non saranno in grado di effettuare l'accesso o disconnettersi, le app potrebbero avere problemi a ottenere i dati degli utenti e i componenti registrati per ricevere le notifiche degli eventi di profilo non riceveranno.
|   **Nome del servizio**    |   ProfSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="virtual-disk"></a>Disco virtuale             
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce servizi di gestione per i dischi, volumi, file System e gli array di archiviazione.
|   **Nome del servizio**    |   VDS
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Alcuna indicazione
|   **Commenti**    |   
|||         
            
<br />          

## <a name="volume-shadow-copy"></a>Copia Shadow del volume           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente di gestire e implementa copie Shadow del Volume usato per il backup e altri scopi. Se questo servizio viene arrestato, le copie shadow non saranno disponibili per il backup e il backup potrebbe non riuscire. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   VSS
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Alcuna indicazione
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="walletservice"></a>WalletService           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Oggetti host utilizzati dai client del portafoglio
|   **Nome del servizio**    |   WalletService
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-audio"></a>Windows Audio            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce l'audio per i programmi basati su Windows.  Se questo servizio viene arrestato, gli effetti e periferiche audio non funzionerà correttamente.  Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo avviare
|   **Nome del servizio**    |   Audiosrv
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-audio-endpoint-builder"></a>Generatore di Endpoint Audio di Windows           
| | |           
|---|---|
|   **Descrizione del servizio** |   Gestisce i dispositivi audio per il servizio Windows Audio.  Se questo servizio viene arrestato, gli effetti e periferiche audio non funzionerà correttamente.  Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo avviare
|   **Nome del servizio**    |   AudioEndpointBuilder
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-biometric-service"></a>Servizio di biometria Windows            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio di biometria Windows offre alle applicazioni client la possibilità di acquisire, confrontare, modificare e archiviare dati biometrici senza accedere direttamente all'hardware biometrici o solo gli esempi. Il servizio è ospitato in un processo SVCHOST privilegiato.
|   **Nome del servizio**    |   WbioSrvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="windows-camera-frame-server"></a>Server di Windows della fotocamera Frame         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente a più client per accedere a frame video dai dispositivi webcam.
|   **Nome del servizio**    |   FrameServer
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-connection-manager"></a>Gestione connessione di Windows           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Rende automatica connettere/disconnettere decisioni basate sulle opzioni di connettività di rete attualmente disponibili per i PC e consente la gestione della connettività di rete in base alle impostazioni di criteri di gruppo.
|   **Nome del servizio**    |   Wcmsvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-defender-network-inspection-service"></a>Servizio Windows Defender Network Inspection System          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di proteggersi da tentativi di intrusione destinate a vulnerabilità note e appena individuate nei protocolli di rete
|   **Nome del servizio**    |   WdNisSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione    
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-defender-service"></a>Servizio Windows Defender         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente agli utenti di proteggere da malware e altro software potenzialmente indesiderato
|   **Nome del servizio**    |   WinDefend
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Foundation Driver Windows - User-mode Driver Framework           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Crea e gestisce i processi di driver in modalità utente. Questo servizio non può essere arrestato.
|   **Nome del servizio**    |   wudfsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-encryption-provider-host-service"></a>Servizio Host Provider di crittografia di Windows     
| | |           
|---|---|   
|   **Descrizione del servizio** |   Le funzionalità relative a crittografia Broker servizio Host Provider di crittografia Windows da provider di crittografia di terze parti per i processi necessari per valutare e applicare i criteri EAS. L'arresto si compromette i controlli di conformità EAS che sono stati stabiliti dall'account di posta elettronica connesso
|   **Nome del servizio**    |   WEPHOSTSVC
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-error-reporting-service"></a>Servizio Segnalazione errori Windows          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente errori da segnalare quando i programmi smettono di funzionare o non risponde e soluzioni esistenti da recapitare. Consente inoltre i log da generare per la diagnostica e ripristinare i servizi. Se questo servizio viene arrestato, segnalazione errori potrebbero non funzionare correttamente e i risultati dei servizi di diagnostica e ripristini potrebbero non essere visualizzati.
|   **Nome del servizio**    |   WerSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Raccoglie e invia i dati di arresto anomalo/blocco usati da MS e fornitori di software indipendenti/produttori di terze parti. I dati vengono utilizzati per diagnosticare bug causato arresto anomalo del sistema, che può includere i bug di sicurezza. Anche necessari per la segnalazione errori
|||         
            
<br />          

## <a name="windows-event-collector"></a>Raccolta eventi Windows          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio gestisce le sottoscrizioni persistente per gli eventi provenienti da origini remote che supportano il protocollo WS-Management. Sono inclusi i registri eventi, hardware e le origini eventi inoltrati in Windows Vista. Il servizio archivia inoltrati gli eventi in un registro eventi locale. Se questo servizio viene arrestato o disabilitato non è possibile creare sottoscrizioni di eventi e gli eventi inoltrati non possono essere accettati.
|   **Nome del servizio**    |   Wecsvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Raccoglie gli eventi ETW (inclusi gli eventi di sicurezza) per una migliore gestibilità, la diagnostica.  Un numero elevato di funzionalità e strumenti di terze parti si basa su di esso, inclusi gli strumenti di controllo di sicurezza
|||         
            
<br />          

## <a name="windows-event-log"></a>Registro eventi di Windows            
| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio gestisce gli eventi e registri eventi. Supporta la registrazione di eventi, l'esecuzione di query eventi, la sottoscrizione di eventi, registri eventi di archiviazione e la gestione dei metadati di evento. È possibile visualizzare gli eventi in formato XML e testo normale. L'arresto del servizio potrebbe compromettere la sicurezza e affidabilità del sistema.
|   **Nome del servizio**    |   EventLog
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-firewall"></a>Windows Firewall         
| | |           
|---|---|   
|   **Descrizione del servizio** |   Windows Firewall consente di proteggere il computer, impedendo agli utenti non autorizzati di ottenere accesso al computer tramite Internet o una rete.
|   **Nome del servizio**    |   MpsSvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="windows-font-cache-service"></a>Servizio Cache tipi di carattere di Windows      
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente di ottimizzare le prestazioni delle applicazioni la memorizzazione nella cache i dati di uso comune del tipo di carattere. Applicazioni verranno avviato il servizio se non è già in esecuzione. Può essere disabilitato, anche se verrà peggiorare le prestazioni dell'applicazione.
|   **Nome del servizio**    |   FontCache
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-image-acquisition-wia"></a>Windows Image Acquisition (WIA)          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce servizi di acquisizione di immagini per scanner e fotocamere digitali
|   **Nome del servizio**    |   stisvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="windows-insider-service"></a>Servizio di Windows Insider     
| | |           
|---|---|   
|   **Descrizione del servizio** |   wisvc
|   **Nome del servizio**    |   wisvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Server non supporta versioni di anteprima, pertanto è no-op nel Server. Funzionalità può anche essere disabilitata tramite criteri di gruppo.
|||         
            
<br />          

##  <a name="windows-installer"></a>Windows Installer       
| | |           
|---|---|
|   **Descrizione del servizio** |   Aggiunge, modifica e rimuove le applicazioni fornite come un pacchetto Windows Installer (*. msi, *. msp). Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   msiserver
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-license-manager-service"></a>Servizio di gestione licenze di Windows          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce un'infrastruttura di supporto per i Microsoft Store.  Questo servizio viene avviato su richiesta e se disabilitata quindi il contenuto acquisito tramite il Microsoft Store non funzionerà correttamente.
|   **Nome del servizio**    |   LicenseManager
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-management-instrumentation"></a>Strumentazione gestione Windows (WMI)       
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce un modello comune di interfaccia e oggetto per accedere alle informazioni di gestione sul sistema operativo, dispositivi, applicazioni e servizi. Se questo servizio viene arrestato, la maggior parte dei software basato su Windows non funzionerà correttamente. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   Winmgmt
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="windows-mobile-hotspot-service"></a>Servizio Hotspot Mobile di Windows          
| | |           
|---|---|       
|   **Descrizione del servizio** |   Offre la possibilità di condividere una connessione di rete dati con un altro dispositivo.
|   **Nome del servizio**    |   icssvc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-modules-installer"></a>Programma di installazione di Windows i moduli        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente a installazione, modifica e rimozione di aggiornamenti di Windows e componenti facoltativi. Se il servizio è disabilitato, installare o disinstallare della mancata riuscita degli aggiornamenti per computer Windows.
|   **Nome del servizio**    |   TrustedInstaller
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-push-notifications-system-service"></a>Servizio di sistema di notifiche Push Windows            
| | |           
|---|---|
|   **Descrizione del servizio** |   Questo servizio viene eseguito nella sessione 0 e ospita il provider della connessione e piattaforma di notifica che gestisce la connessione tra il dispositivo e il server WNS.
|   **Nome del servizio**    |   WpnService
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Necessario per i riquadri animati e altre funzionalità
|||         
            
<br />      

## <a name="windows-push-notifications-user-service"></a>Servizio di Windows Push delle notifiche utente          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio ospita piattaforma di notifica di Windows che fornisce il supporto per locale e le notifiche push. Le notifiche supportate sono tessera, di tipo avviso popup e raw.
|   **Nome del servizio**    |   WpnUserService
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   OK per disattivare
|   **Commenti**    |   Modello di servizio utente
|||         
            
<br />          
    
## <a name="windows-remote-management-ws-management"></a>Gestione remota Windows (WS-Management)            
| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio Gestione remota di Windows (WinRM) implementa il protocollo WS-Management per la gestione remota. WS-Management è un protocollo di servizi Web standard usato per la gestione remota del software e dell'hardware. Il servizio WinRM ascolta le richieste WS-Management sulla rete e le elabora. Il servizio WinRM deve essere configurato con uno strumento da riga di comando winrm.cmd oppure mediante Criteri di gruppo per poter eseguire l'ascolto sulla rete. Il servizio WinRM fornisce l'accesso ai dati WMI e abilita la raccolta di eventi. Per la raccolta di eventi e la sottoscrizione agli eventi il servizio deve essere in esecuzione. I messaggi WinRM usano HTTP e HTTPS come trasporto. Il servizio WinRM non dipende da IIS ma è preconfigurato per condividere una porta con IIS nello stesso computer.  Il servizio WinRM si riserva il prefisso URL /wsman. Per evitare conflitti con IIS, gli amministratori devono verificare che nessun sito Web ospitato su IIS usi il prefisso URL /wsman.
|   **Nome del servizio**    |   WinRM
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Necessario per la gestione remota
|||         
            
<br />          

##  <a name="windows-search"></a>Windows Search      
| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce l'indicizzazione del contenuto, la memorizzazione nella cache proprietà e i risultati della ricerca per i file, posta elettronica e altro contenuto.
|   **Nome del servizio**    |   WSearch
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Disabled
|   **Raccomandazione**  |   Già disabilitata
|   **Commenti**    |   
|||         
            
<br />          

##  <a name="windows-time"></a>Ora di Windows        
| | |           
|---|---|   
|   **Descrizione del servizio** |   Mantiene la sincronizzazione di data e ora in tutti i client e server nella rete. Se questo servizio viene arrestato, la sincronizzazione di data e ora sarà disponibile. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   W32Time
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione
|   **Commenti**    |   
|||         
            
<br />          

## <a name="windows-update"></a>Windows Update           
| | |           
|---|---|       
|   **Descrizione del servizio** |   Abilita il rilevamento, download e installazione degli aggiornamenti per Windows e altri programmi. Se il servizio è disabilitato, gli utenti del computer non saranno in grado di utilizzare Windows Update o le funzionalità di aggiornamento automatico e i programmi non sarà in grado di usare l'API di Windows Update Agent (WUA).
|   **Nome del servizio**    |   wuauserv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>Servizio di rilevamento automatico Proxy Web di WinHTTP         
| | |           
|---|---|   
|   **Descrizione del servizio** |   WinHTTP implementa stack client HTTP e fornisce agli sviluppatori con un componente di automazione COM e l'API Win32 per l'invio delle richieste HTTP e la ricezione delle risposte. Inoltre, WinHTTP fornisce supporto per l'individuazione automatica di una configurazione del proxy tramite l'implementazione del protocollo Web Proxy Auto-Discovery (WPAD).
|   **Nome del servizio**    |   WinHttpAutoProxySvc
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Non disabilitare
|   **Commenti**    |   Tutto ciò che usa lo stack di rete può avere una dipendenza funzionale in questo servizio. Molte organizzazioni si basano su questa opzione per configurare il routing del proxy HTTP delle reti interne.  In caso contrario, internamente che hanno origine le connessioni HTTP a Internet tutti non riuscirà.
|||         
            
<br />          

## <a name="wired-autoconfig"></a>Configurazione automatica reti cablate         
| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio configurazione automatica reti cablate (DOT3SVC) è responsabile dell'esecuzione IEEE 802.1x autenticazione sulle interfacce Ethernet. Se la distribuzione corrente di rete cablata impone l'autenticazione 802.1X, il servizio DOT3SVC deve essere configurato per l'esecuzione per la connettività di livello 2 e/o per fornire l'accesso alle risorse di rete. Non sono interessate dal servizio DOT3SVC reti cablate che non applicano l'autenticazione 802.1x.
|   **Nome del servizio**    |   dot3svc
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione   
|   **Commenti**    |   
|||         
            
<br />          

## <a name="wmi-performance-adapter"></a>Scheda WMI Performance          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce informazioni della libreria delle prestazioni dai provider di Strumentazione gestione Windows (WMI) per i client nella rete. Questo servizio viene eseguito solo quando viene attivato l'Helper di dati delle prestazioni.
|   **Nome del servizio**    |   wmiApSrv
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Manual
|   **Raccomandazione**  | Alcuna indicazione       
|   **Commenti**    |   
|||         
            
<br />          

## <a name="workstation"></a>workstation          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Crea e gestisce le connessioni di rete client ai server remoti tramite il protocollo SMB. Se questo servizio viene arrestato, queste connessioni sarà disponibile. Se il servizio è disabilitato, tutti i servizi che dipendono esplicitamente da esso avrà esito negativo iniziare.
|   **Nome del servizio**    |   LanmanWorkstation
|   **Installazione**    |   Sempre installata
|   **StartType**   |   Automatico
|   **Raccomandazione**  | Alcuna indicazione       
|   **Commenti**    |   
|||         
            
<br />

## <a name="xbox-live-auth-manager"></a>Xbox Live Auth Manager           
| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce servizi di autenticazione e autorizzazione per l'interazione con Xbox Live. Se questo servizio viene arrestato, alcune applicazioni potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   XblAuthManager
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Deve essere disabilitato
|   **Commenti**    |   
|||         
            
<br />          

## <a name="xbox-live-game-save"></a>Salva gioco con Xbox Live          
| | |           
|---|---|   
|   **Descrizione del servizio** |   Sincronizzazioni questo servizio Salva i dati di Xbox Live Salva giochi abilitate.  Se questo servizio viene arrestato, gioco salvare i dati non caricare o scaricare da Xbox Live.
|   **Nome del servizio**    |   XblGameSave
|   **Installazione**    |   Solo con esperienza Desktop
|   **StartType**   |   Manual
|   **Raccomandazione**  |   Deve essere disabilitato
|   **Commenti**    |   Sincronizzazioni questo servizio Salva i dati di Xbox Live Salva giochi abilitate.  Se questo servizio viene arrestato, gioco salvare i dati non caricare o scaricare da Xbox Live.
|||         
                
<br /> 
<br /> 

