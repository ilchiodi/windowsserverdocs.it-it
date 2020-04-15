---
title: Linee guida sulla sicurezza per i servizi di sistema in Windows Server 2016
description: Linee guida sulla sicurezza per la disabilitazione dei servizi in Windows Server 2016 con Esperienza desktop
ms.prod: windows-server
ms.technology: techgroup-security
ms.topic: article
ms.date: 11/26/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: f7bb6f73fb2d898c3e5170dda96fef5aea611a88
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855054"
---
# <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>Linee guida sulla sicurezza per la disabilitazione dei servizi in Windows Server 2016 con Esperienza desktop

Si applica a: Windows Server 2016

Il sistema operativo Windows include molti servizi di sistema che offrono importanti funzionalità. Servizi diversi presentano criteri di avvio predefiniti diversi: alcuni vengono avviati per impostazione predefinita (modalità automatica), altri quando necessario (modalità manuale), altri ancora sono disabilitati per impostazione predefinita e deve essere abilitati in modo esplicito prima di poter essere eseguiti. Queste impostazioni predefinite sono state scelte con attenzione per ogni servizio al fine di bilanciare prestazioni, funzionalità e sicurezza per i clienti tipici.

Alcuni clienti aziendali, tuttavia, potrebbero preferire un approccio più incentrato sulla sicurezza per PC e server Windows, in modo da ridurre al minimo la superficie di attacco, e pertanto potrebbero voler disabilitare completamente tutti i servizi che non sono necessari nei loro ambienti. Per questi clienti, Microsoft&reg; fornisce indicazioni che illustrano quali servizi possono essere disattivati in modo sicuro per questo scopo.

Le linee guida riguardano solo Windows Server 2016 con Esperienza desktop (solo se non usato in sostituzione del desktop per gli utenti finali). A partire da Windows Server 2019, le indicazioni riportate in queste linee guida sono configurate per impostazione predefinita. Ogni servizio del sistema è classificato nel modo seguente:

-   **Opportuno disabilitare:** un'azienda attenta alla sicurezza preferirà probabilmente disabilitare il servizio e rinunciare alla relativa funzionalità (vedi di seguito altri dettagli).
- **OK disabilitare:** il servizio offre funzionalità utili ad alcune aziende, ma non a tutte, e le aziende attente alla sicurezza che non fanno uso del servizio possono disabilitarlo senza alcun rischio.
- **Non disabilitare:** la disabilitazione del servizio ha impatto su funzionalità essenziali o impedisce il corretto funzionamento di funzionalità o ruoli specifici. Il servizio non dovrebbe quindi essere disabilitato.
-  **(Nessuna indicazione):** l'impatto della disabilitazione del servizio non è stato completamente valutato. Pertanto, la configurazione predefinita del servizio non dovrebbe essere modificata.


I clienti possono configurare i propri PC e server Windows in modo da disabilitare determinati servizi usando i modelli di sicurezza nei propri criteri di gruppo oppure tramite l'automazione di PowerShell. In alcuni casi, le linee guida includono specifiche impostazioni di Criteri di gruppo che disabilitano direttamente la funzionalità del servizio, in alternativa alla disabilitazione del servizio stesso.

Microsoft consiglia ai clienti di disabilitare i servizi seguenti e le rispettive attività pianificate in Windows Server 2016 con Esperienza desktop:

Servizi: 
1. Gestione autenticazione Xbox Live
2. Giochi salvati su Xbox Live

Attività pianificate: 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

Puoi anche accedere alle informazioni su tutti i servizi descritti in dettaglio in questo articolo visualizzando il foglio di calcolo di Microsoft Excel allegato: [Linee guida sulla sicurezza per la disabilitazione dei servizi di sistema in Windows Server 2016 con Esperienza desktop](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx)



### <a name="disabling-services-not-installed-by-default"></a>Disabilitazione dei servizi non installati per impostazione predefinita

Microsoft sconsiglia l'applicazione di criteri per disabilitare i servizi che non vengono installati per impostazione predefinita.
-  Se la funzionalità viene installata, il servizio è in genere necessario. L'installazione del servizio o della funzionalità richiede diritti amministrativi. Impedire l'installazione delle funzionalità, ma non l'avvio del servizio.
-  Il blocco del servizio di Microsoft Windows non impedisce a un amministratore (o in alcuni casi a un utente senza privilegi di amministratore) di installare un servizio equivalente di terze parti, anche con un rischio per la sicurezza più elevato.
-  Uno standard di riferimento o un benchmark che disabilita un servizio di Windows non predefinito (ad esempio, W3SVC) darà ad alcuni revisori l'impressione errata che la tecnologia (ad esempio, IIS) sia intrinsecamente non sicura e non debba mai essere usata.
-  Se la funzionalità (con il servizio) non viene mai installata, aggiunge semplicemente un carico superfluo allo standard di riferimento e al lavoro di verifica.


Per tutti i servizi di sistema elencati in questo documento, le due tabelle seguenti offrono una spiegazione delle colonne e dei consigli di Microsoft per l'abilitazione e la disabilitazione dei servizi di sistema in Windows Server 2016 con Esperienza desktop: 



### <a name="explanation-of-columns"></a>Spiegazione delle colonne

| | |
|---|---|
|**Descrizione del servizio**|   Descrizione del servizio, da sc.exe qdescription.|
|**Nome** |Nome principale (interno) del servizio|
|**Installazione** | *Sempre installato*: il servizio è installato in Windows Server 2016 Core e Windows Server 2016 con Esperienza desktop. *Solo con Esperienza desktop*: il servizio è installato in Windows Server 2016 con Esperienza desktop, ma ***non*** è installato in Server Core. |
|**Tipo di avvio**  |Tipo di avvio del servizio in Windows Server 2016|
|**Consiglio** |Consiglio o raccomandazione di Microsoft sulla disabilitazione del servizio in Windows Server 2016 in una distribuzione aziendale tipica e gestita correttamente, in cui il server non viene usato in sostituzione del desktop per gli utenti finali.|
|**Commenti** |Spiegazione aggiuntiva|



### <a name="explanation-of-microsoft-recommendations"></a>Spiegazioni dei consigli di Microsoft

| | |
|---|---|
|**Non disabilitare** |Il servizio non dovrebbe essere disabilitato.|
|**OK disabilitare**| Il servizio può essere disabilitato se la funzionalità supportata non viene usata.|
|**Già disabilitato**|  Il servizio è disabilitato per impostazione predefinita. Non è necessario applicarlo con criteri.|
|**Opportuno disabilitare** |Il servizio non dovrebbe mai essere abilitato in un sistema gestito correttamente a livello aziendale.|



Le tabelle seguenti presentano le linee guida sulla sicurezza per la disabilitazione dei servizi di sistema in Windows Server 2016 con Esperienza desktop:



##  <a name="activex-installer-axinstsv"></a>ActiveX Installer (AxInstSV)

| | |
|---|---|
|   **Descrizione del servizio** |   Fornisce la convalida del controllo dell'account utente per l'installazione dei controlli ActiveX da Internet e consente la gestione dell'installazione dei controlli ActiveX in base alle impostazioni di Criteri di gruppo. Il servizio viene avviato su richiesta e, se disabilitato, il comportamento dell'installazione dei controlli ActiveX dipenderà dalle impostazioni del browser predefinito.    |
|   **Nome del servizio**    |   AxInstSV    |
|   **Installazione**    |   Solo con Esperienza desktop    |
|   **Tipo di avvio**   |   Manuale  |
|   **Consiglio**  |   OK disabilitare   |
|   **Commenti**    |   OK disabilitare se la funzionalità non è necessaria. |




## <a name="alljoyn-router-service"></a>Servizio router AllJoyn   

| | |
|---|---|
|   **Descrizione del servizio** |   Instrada i messaggi AllJoyn per i client AllJoyn locali. Se il servizio viene arrestato, l'esecuzione dei client AllJoyn senza router associati non sarà possibile. |
|   **Nome del servizio**    |   AJRouter    |
|   **Installazione**    |   Solo con Esperienza desktop    |
|   **Tipo di avvio**   |   Manuale  |
|   **Consiglio**  | Nessuna indicazione       |
|   **Commenti**    |       |
| | |



## <a name="app-readiness"></a>Preparazione app

| | |
|---|---|
**Descrizione del servizio** |   Prepara le app per consentirne l'uso la prima volta che un utente accede al PC e quando vengono aggiunte nuove app.
**Nome del servizio**    |   AppReadiness
**Installazione**    |   Solo con Esperienza desktop
**Tipo di avvio**   |   Manuale
**Consiglio**  |   Non disabilitare
**Commenti**    |   
| | |



##  <a name="application-identity"></a>Identità applicazione

| | |       
|---|---|   
**Descrizione del servizio** |   Determina e verifica l'identità di un'applicazione. La disabilitazione di questo servizio impedisce l'applicazione di AppLocker.
**Nome del servizio**    |   AppIDSvc
**Installazione**    |   Sempre installato
**Tipo di avvio**   |   Manuale
**Consiglio**  |Nessuna indicazione    
**Commenti**    |   
|||     



##  <a name="application-information"></a>Informazioni applicazioni 

| | |       
|---|---|   
|   **Descrizione del servizio** |   Facilita l'esecuzione di applicazioni interattive con privilegi amministrativi aggiuntivi.  Se il servizio viene arrestato, gli utenti non saranno in grado di avviare le applicazioni con i privilegi amministrativi aggiuntivi eventualmente necessari per l'esecuzione delle attività utente desiderate.
|   **Nome del servizio**    |   Appinfo
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   Supporta l'elevazione dei privilegi di Controllo dell'account utente stesso desktop
|||     



##  <a name="application-layer-gateway-service"></a>Servizio Gateway di livello applicazione       

| | |           
|---|---|           
|   **Descrizione del servizio** |   Fornisce il supporto per i plug-in dei protocolli di terze parti per Condivisione connessione Internet.
|   **Nome del servizio**    |   ALG
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |Nessuna indicazione    
|   **Commenti**    |   
|||     



##  <a name="application-management"></a>Gestione applicazioni      

| | |           
|---|---|       
|   **Descrizione del servizio** |   Elabora le richieste di installazione, rimozione ed enumerazione del software distribuito tramite Criteri di gruppo. Se il servizio è disabilitato, gli utenti non saranno in grado di installare, rimuovere o enumerare il software distribuito tramite Criteri di gruppo. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   AppMgmt
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="appx-deployment-service-appxsvc"></a>Servizio di distribuzione AppX (AppXSVC)       

| | |           
|---|---|
|   **Descrizione del servizio** |   Offre supporto infrastrutturale per la distribuzione di applicazioni dello Store. Il servizio viene avviato su richiesta e, se disabilitato, le applicazioni dello Store non verranno distribuite nel sistema e potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   AppXSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="auto-time-zone-updater"></a>Strumento di aggiornamento automatico fuso orario           

| | |           
|---|---|           
|   **Descrizione del servizio** |   Imposta automaticamente il fuso orario del sistema.
|   **Nome del servizio**    |   tzautoupdate
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Disabilitata
|   **Consiglio**  |   Già disabilitato
|   **Commenti**    |   
|||         



## <a name="background-intelligent-transfer-service"></a>Servizio trasferimento intelligente in background          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Trasferisce file in background usando la larghezza di banda inattiva della rete. Se il servizio è disabilitato, le applicazioni dipendenti da BITS, come Windows Update o MSN Explorer, non saranno in grado di effettuare il download automatico di programmi e altre informazioni.
|   **Nome del servizio**    |   BITS
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         




## <a name="background-tasks-infrastructure-service"></a>Servizio infrastruttura attività in background      

| | |           
|---|---|   
|   **Descrizione del servizio** |   Servizio dell'infrastruttura Windows che controlla quali attività in background possono essere eseguite nel sistema.
|   **Nome del servizio**    |   BrokerInfrastructure
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="base-filtering-engine"></a>Base Filtering Engine            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Base Filtering Engine (BFE) è un servizio per la gestione dei criteri firewall e IPSec (Internet Protocol Security) e l'implementazione del filtro delle modalità utente. L'arresto o la disabilitazione del servizio BFE riduce in modo significativo la sicurezza del sistema. Può causare un comportamento imprevedibile delle applicazioni di gestione IPSec e firewall.
|   **Nome del servizio**    |   BFE
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="bluetooth-support-service"></a>Servizio di supporto Bluetooth            

| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio Bluetooth supporta l'individuazione e l'associazione di dispositivi Bluetooth remoti.  L'arresto o la disabilitazione di questo servizio può causare il malfunzionamento dei dispositivi Bluetooth già installati e impedire l'individuazione o l'associazione di nuovi dispositivi.
|   **Nome del servizio**    |   bthserv
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   OK disabilitare se non è usato. Altro meccanismo di disabilitazione: https://technet.microsoft.com/library/dd252791.aspx
|||         




## <a name="cdpusersvc"></a>CDPUserSvc           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio utente è usato per gli scenari relativi alla piattaforma dispositivi connessi.
|   **Nome del servizio**    |   CDPUserSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   Modello di servizio utente
|||         




##  <a name="certificate-propagation"></a>Propagazione certificati     

| | |           
|---|---|
|   **Descrizione del servizio** |   Copia i certificati utente e i certificati radice dalle smart card all'archivio certificati dell'utente corrente, rileva quando una smart card è inserita in un lettore di smart card e, se necessario, installa il minidriver Plug and Play della smart card.
|   **Nome del servizio**    |   CertPropSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="client-license-service-clipsvc"></a>Servizio licenze client (ClipSVC)        

| | |           
|---|---|   
|   **Descrizione del servizio** |   Offre supporto infrastrutturale per Microsoft Store. Il servizio viene avviato su richiesta e, se disabilitato, le applicazioni acquistate tramite Microsoft Store presenteranno problemi di funzionamento.
|   **Nome del servizio**    |   ClipSVC
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="cng-key-isolation"></a>Isolamento chiavi CNG

| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio Isolamento chiavi CNG è ospitato nel processo della LSA. Consente l'isolamento dei processi relativi alle chiavi per le chiavi private e le operazioni di crittografia associate, come richiesto da Criteri comuni. Il servizio archivia e usa chiavi a lunga durata nell'ambito di un processo sicuro, in conformità con i requisiti di Criteri comuni.
|   **Nome del servizio**    |   KeyIso
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="com-event-system"></a>COM+ Event System       

| | |           
|---|---|       
|   **Descrizione del servizio** |   Supporta il servizio di notifica eventi di sistema (SENS), che implementa la distribuzione automatica degli eventi nei componenti COM che eseguono la sottoscrizione. Se il servizio viene arrestato, il servizio SENS verrà chiuso e non sarà più in grado di inviare notifiche di connessione e disconnessione. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   EventSystem
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="com-system-application"></a>Applicazione di sistema COM+     

| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce la configurazione e registrazione di componenti basati su COM+. Se il servizio viene arrestato, la maggior parte dei componenti basati su COM+ non funzionerà correttamente. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   COMSysApp
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="computer-browser"></a>Browser di computer        

| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce un elenco aggiornato dei computer in rete e lo fornisce ai computer designati come browser. Se il servizio viene arrestato, l'elenco non verrà aggiornato né gestito. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   Browser
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Disabilitata
|   **Consiglio**  |   Già disabilitato
|   **Commenti**    |   
|||         



## <a name="connected-devices-platform-service"></a>Servizio piattaforma dispositivi connessi       

| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio viene usato per scenari con dispositivi connessi e Universal Glass.
|   **Nome del servizio**    |   CDPSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="connected-user-experiences-and-telemetry"></a>Esperienze utente connesse e telemetria     

| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio Esperienze utente connesse e telemetria abilita funzionalità che supportano esperienze utente connesse e nell'applicazione. Questo servizio gestisce inoltre la raccolta e la trasmissione basate su eventi delle informazioni diagnostiche e sull'utilizzo (usate per migliorare l'esperienza e la qualità della piattaforma Windows) quando le opzioni di privacy per la diagnostica e l'utilizzo sono abilitate in Feedback e diagnostica.
|   **Nome del servizio**    |   DiagTrack
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="contact-data"></a>Dati contatti        

| | |           
|---|---|       
|   **Descrizione del servizio** |   Indicizza i dati dei contatti per ricerche più rapide. Se il servizio viene arrestato o disabilitato, i contatti potrebbero non essere inclusi nei risultati di ricerca.
|   **Nome del servizio**    |   PimIndexMaintenanceSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   Modello di servizio utente
|||         



## <a name="coremessaging"></a>CoreMessaging            

| | |           
|---|---|           
|   **Descrizione del servizio** |   Gestisce le comunicazioni tra i componenti del sistema.
|   **Nome del servizio**    |   CoreMessagingRegistrar
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="credential-manager"></a>Gestione credenziali           

| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente l'archiviazione e il recupero in modo sicuro delle credenziali degli utenti, delle applicazioni e dei pacchetti dei servizi di sicurezza.
|   **Nome del servizio**    |   VaultSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="cryptographic-services"></a>Servizi di crittografia           

| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce tre servizi di gestione: il servizio Database catalogo, per verificare le firme dei file di Windows e installare nuovi programmi, il servizio Archivio radice protetto, per aggiungere e rimuovere dal computer i certificati dell'Autorità di certificazione radice attendibile, e il Servizio aggiornamento automatico certificati radice, per il recupero dei certificati radice da Windows Update e l'abilitazione di scenari come SSL. Se il servizio viene arrestato, i servizi di gestione non funzioneranno correttamente. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   CryptSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="data-sharing-service"></a>Servizio di condivisione dati         

| | |           
|---|---|       
|   **Descrizione del servizio** |   Offre funzionalità di gestione dei dati tra le applicazioni.
|   **Nome del servizio**    |   DsSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          

| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio DCP (raccolta dei dati e pubblicazione) consente alle app proprietarie di caricare dati nel cloud.
|   **Nome del servizio**    |   DcpSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="dcom-server-process-launcher"></a>Utilità di avvio processi server DCOM         

| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio DCOMLAUNCH avvia server COM e DCOM in risposta a richieste di attivazione degli oggetti. Se il servizio viene arrestato o disabilitato, i programmi che usano COM o DCOM non funzioneranno correttamente. È fortemente consigliabile mantenere in esecuzione il servizio DCOMLAUNCH.
|   **Nome del servizio**    |   DcomLaunch
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |Nessuna indicazione    
|   **Commenti**    |   
|||         



##  <a name="device-association-service"></a>Servizio associazione dispositivi      

| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente l'associazione tra il sistema e i dispositivi cablati o wireless.
|   **Nome del servizio**    |   DeviceAssociationService
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="device-install-service"></a>Servizio installazione dispositivi

| | |
|---|---|
|   **Descrizione del servizio** |   Consente a un computer di riconoscere le modifiche hardware e adattarsi ad esse con un intervento minimo o nullo da parte dell'utente. Se il servizio viene arrestato o disabilitato, il sistema diventerà instabile.
|   **Nome del servizio**    |   DeviceInstall
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione
|   **Commenti**    |
|||



##  <a name="device-management-enrollment-service"></a>Servizio di registrazione gestione dispositivi        

| | |           
|---|---|       
|   **Descrizione del servizio** |   Esegue le attività di registrazione dei dispositivi per la gestione dei dispositivi.
|   **Nome del servizio**    |   DmEnrollmentSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="device-setup-manager"></a>Gestione configurazione dispositivi         

| | |           
|---|---|       
|   **Descrizione del servizio** |   Abilita il rilevamento, il download e l'installazione di software correlato ai dispositivi. Se il servizio viene disabilitato, i dispositivi potrebbero essere configurati con software obsoleto e non funzionare correttamente.
|   **Nome del servizio**    |   DsmSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="devquery-background-discovery-broker"></a>Gestore individuazione in background DevQuery         

| | |           
|---|---|           
|   **Descrizione del servizio** |   Consente alle app di individuare i dispositivi con un'attività in background.
|   **Nome del servizio**    |   DevQueryBroker
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="dhcp-client"></a>Client DHCP          

| | |           
|---|---|       
|   **Descrizione del servizio** |   Registra e aggiorna gli indirizzi IP e i record DNS per il computer. Se il servizio viene arrestato, il computer non riceverà né indirizzi IP dinamici né aggiornamenti DNS. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   Dhcp
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="diagnostic-policy-service"></a>Servizio Criteri di diagnostica            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Il Servizio Criteri di diagnostica consente di rilevare e risolvere i problemi relativi ai componenti di Windows.  Se il servizio viene arrestato, le funzionalità di diagnostica verranno disattivate.
|   **Nome del servizio**    |   DPS
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="diagnostic-service-host"></a>Host servizio di diagnostica     

| | |           
|---|---|       
|   **Descrizione del servizio** |   Host servizio di diagnostica viene usato dal Servizio Criteri di diagnostica per l'hosting delle funzionalità di diagnostica che devono essere eseguite nel contesto di un servizio locale.  Se il servizio viene arrestato, tutte le funzionalità di diagnostica che dipendono da tale servizio verranno disattivate.
|   **Nome del servizio**    |   WdiServiceHost
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="diagnostic-system-host"></a>Host sistema di diagnostica           

| | |           
|---|---|       
|   **Descrizione del servizio** |   Host sistema di diagnostica viene usato dal Servizio Criteri di diagnostica per l'hosting delle funzionalità di diagnostica che devono essere eseguite in un contesto del sistema locale.  Se il servizio viene arrestato, tutte le funzionalità di diagnostica che dipendono da tale servizio verranno disattivate.
|   **Nome del servizio**    |   WdiSystemHost
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="distributed-link-tracking-client"></a>Manutenzione collegamenti distribuiti client            

| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce i collegamenti tra file NTFS all'interno di un computer o tra i computer di una rete.
|   **Nome del servizio**    |   TrkWks
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="distributed-transaction-coordinator"></a>Distributed Transaction Coordinator     

| | |           
|---|---|   
|   **Descrizione del servizio** |   Coordina le transazioni estese a più gestori di risorse, quali database, code di messaggi e file system. Se il servizio viene arrestato, le transazioni avranno esito negativo. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   MSDTC
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="dmwappushsvc"></a>dmwappushsvc        

| | |           
|---|---|       
|   **Descrizione del servizio** |   Servizio routing messaggi push WAP
|   **Nome del servizio**    |   dmwappushservice
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   Il servizio è obbligatorio sui dispositivi client per Intune, MDM e simili tecnologie di gestione, e per Filtro scrittura unificato. Non è necessario per il server.
|||         



##  <a name="dns-client"></a>Client DNS      

| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio Client DNS (dnscache) memorizza nella cache i nomi DNS e registra il nome completo del computer per il computer in uso. Se tale servizio viene arrestato, la risoluzione dei nomi DNS verrà eseguita comunque, ma i risultati delle query relative ai nomi DNS non verranno memorizzati nella cache e il nome del computer non verrà registrato. Se il servizio viene disattivato, non sarà più possibile avviare i servizi che dipendono esplicitamente da esso.
|   **Nome del servizio**    |   Dnscache
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="downloaded-maps-manager"></a>Gestione mappe scaricate     

| | |           
|---|---|   
|   **Descrizione del servizio** |   Servizio di Windows per consentire alle applicazioni di accedere alle mappe scaricate. Il servizio viene avviato su richiesta dall'applicazione che accede alle mappe scaricate. La disabilitazione di questo servizio impedirà alle app di accedere alle mappe.
|   **Nome del servizio**    |   MapsBroker
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   La disabilitazione causa l'interruzione delle app che si basano sul servizio. OK disabilitare se le app non si basano sul servizio.
|||         



## <a name="embedded-mode"></a>Modalità incorporata            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio Modalità incorporata consente scenari correlati alle applicazioni in background.  La disabilitazione di questo servizio impedirà l'attivazione di applicazioni in background.
|   **Nome del servizio**    |   embeddedmode
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="encrypting-file-system-efs"></a>Encrypting File System (EFS)

| | |                   
|---|---|           
|   **Descrizione del servizio** | Implementa la tecnologia di crittografia file principale usata per archiviare i file crittografati nei volumi con file system NTFS. Se il servizio viene arrestato o disabilitato, le applicazioni non saranno in grado di accedere ai file crittografati.            
|   **Nome del servizio**  |  EFS            
|   **Installazione**  |  Sempre installato           
|   **Tipo di avvio**   |  Manuale           
|   **Consiglio**  | Nessuna indicazione           
|   **Commenti**   |
|||                 



## <a name="enterprise-app-management-service"></a>Servizio di gestione app aziendali            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente la gestione delle applicazioni aziendali.
|   **Nome del servizio**    |   EntAppSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="extensible-authentication-protocol"></a>Extensible Authentication Protocol           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio EAP (Extensible Authentication Protocol) offre l'autenticazione di rete 802.1x per reti cablate e wireless, VPN e Protezione accesso alla rete.  Offre inoltre interfacce API (Application Programming Interfaces) usate dai client di accesso alla rete, inclusi client wireless e VPN, durante il processo di autenticazione.  Se il servizio viene disabilitato, il computer non avrà accesso alle reti che richiedono l'autenticazione EAP.
|   **Nome del servizio**    |   EapHost
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="function-discovery-provider-host"></a>Host provider di individuazione funzioni         

| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio FDPHOST ospita i provider di individuazione delle reti di individuazione funzioni. Questi provider forniscono servizi di individuazione delle reti per i protocolli SSDP (Simple Services Discovery Protocol) e WS-D (Web Services - Discovery). L'arresto o la disabilitazione del servizio FDPHOST determina la disabilitazione dell'individuazione delle reti per tali protocolli quando viene usata l'individuazione funzioni. Se questo servizio non è disponibile, i servizi di rete che usano l'individuazione funzioni e si basano su tali protocolli di individuazione non saranno in grado di trovare dispositivi o risorse di rete.
|   **Nome del servizio**    |   fdPHost
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="function-discovery-resource-publication"></a>Pubblicazione risorse per individuazione      

| | |           
|---|---|       
|   **Descrizione del servizio** |   Pubblica questo computer e le risorse a esso collegate in modo che possano essere individuate in rete.  Se il servizio viene arrestato, le risorse di rete non saranno più pubblicate e non potranno essere individuate da altri computer della rete.
|   **Nome del servizio**    |   FDResPub
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="geolocation-service"></a>Servizio di georilevazione          

| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio esegue il monitoraggio della posizione corrente del sistema e gestisce i recinti virtuali (posizione geografica con eventi associati).  Se il servizio viene disabilitato, le applicazioni non potranno usare né ricevere notifiche relative alla georilevazione o ai recinti virtuali.
|   **Nome del servizio**    |   lfsvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   La disabilitazione causa l'interruzione delle app che si basano sul servizio. OK disabilitare se le app non si basano sul servizio.
|||         



##  <a name="group-policy-client"></a>Client di Criteri di gruppo     

| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio è responsabile dell'applicazione delle impostazioni configurate dagli amministratori per il computer e gli utenti tramite il componente Criteri di gruppo. Se il servizio viene disabilitato, le impostazioni non verranno applicate e non sarà possibile gestire applicazioni e componenti tramite Criteri di gruppo. Se il servizio viene disabilitato, tutti i componenti o le applicazioni che dipendono dal componente Criteri di gruppo potrebbero non funzionare.
|   **Nome del servizio**    |   gpsvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         




## <a name="human-interface-device-service"></a>Servizio Human Interface Device           

| | |           
|---|---|       
|   **Descrizione del servizio** |   Abilita e gestisce l'uso dei pulsanti speciali su tastiere, telecomandi e altri dispositivi multimediali. È consigliabile mantenere in esecuzione questo servizio.
|   **Nome del servizio**    |   hidserv
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="hv-host-service"></a>Servizio host hypervisor     

| | |           
|---|---|   
|   **Descrizione del servizio** |   Offre un'interfaccia per l'hypervisor Hyper-V per fornire contatori delle prestazioni per ogni partizione al sistema operativo host.
|   **Nome del servizio**    |   HvHost
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Servizio di miglioramento delle prestazioni per le macchine virtuali guest. Attualmente è usato solo per le macchine virtuali popolate in modo esplicito, ma verrà usato in Application Guard.
|||         



## <a name="hyper-v-data-exchange-service"></a>Servizio Scambio di dati Hyper-V        

| | |           
|---|---|   
|   **Descrizione del servizio** |   Rende disponibile un meccanismo per lo scambio di dati tra la macchina virtuale e il sistema operativo in esecuzione nel computer fisico.
|   **Nome del servizio**    |   vmickvpexchange
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Vedi HvHost
|||         



## <a name="hyper-v-guest-service-interface"></a>Interfaccia servizio guest Hyper-V          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce un'interfaccia per l'interazione dell'host Hyper-V con servizi specifici in esecuzione nella macchina virtuale.
|   **Nome del servizio**    |   vmicguestinterface
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Vedi HvHost
|||         



## <a name="hyper-v-guest-shutdown-service"></a>Servizio Arresto guest Hyper-V           

| | |           
|---|---|       
|   **Descrizione del servizio** |   Rende disponibile un meccanismo per l'arresto del sistema operativo della macchina virtuale dalle interfacce di gestione del computer fisico.
|   **Nome del servizio**    |   vmicshutdown
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Vedi HvHost
|||         



## <a name="hyper-v-heartbeat-service"></a>Servizio Heartbeat Hyper-V
| | |
|---|---|
|   **Descrizione del servizio** |   Esegue il monitoraggio dello stato della macchina virtuale segnalando un heartbeat a intervalli regolari. Questo servizio consente di identificare le macchine virtuali in esecuzione che si sono bloccate.
|   **Nome del servizio**    |   vmicheartbeat
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Vedi HvHost
|||



## <a name="hyper-v-powershell-direct-service"></a>Servizio Hyper-V PowerShell Direct            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Rende disponibile un meccanismo per gestire la macchina virtuale con PowerShell tramite una sessione di macchina virtuale senza una rete virtuale.
|   **Nome del servizio**    |   vmicvmsession
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Vedi HvHost
|||         



## <a name="hyper-v-remote-desktop-virtualization-service"></a>Servizio Virtualizzazione Desktop remoto Hyper-V            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Offre una piattaforma per le comunicazioni tra la macchina virtuale e il sistema operativo in esecuzione nel computer fisico.
|   **Nome del servizio**    |   vmicrdv
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Vedi HvHost
|||         



## <a name="hyper-v-time-synchronization-service"></a>Servizio Sincronizzazione ora Hyper-V         

| | |           
|---|---|       
|   **Descrizione del servizio** |   Sincronizza l'ora di sistema della macchina virtuale con quella del computer fisico.
|   **Nome del servizio**    |   vmictimesync
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Vedi HvHost
|||         



## <a name="hyper-v-volume-shadow-copy-requestor"></a>Richiedente Copia Shadow del volume Hyper-V         

| | |           
|---|---|           
|   **Descrizione del servizio** |   Coordina le comunicazioni necessarie per usare il Servizio Copia Shadow del volume per il backup delle applicazioni e dei dati della macchina virtuale dal sistema operativo del computer fisico.
|   **Nome del servizio**    |   vmicvss
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Vedi HvHost
|||         



## <a name="ike-and-authip-ipsec-keying-modules"></a>Moduli di impostazione chiavi IPSec IKE e Auth-IP          

| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio IKEEXT ospita i moduli di impostazione chiavi IKE (Internet Key Exchange) e Auth-IP (Authenticated Internet Protocol). Questi moduli vengono usati per l'autenticazione e lo scambio di chiavi in IPSec (Internet Protocol Security). Se il servizio IKEEXT viene arrestato o disabilitato, lo scambio di chiavi IKE e AuthIP con i computer peer non potrà avere luogo. IPSec è in genere configurato per l'uso di IKE o AuthIP, pertanto l'arresto o la disabilitazione del servizio IKEEXT potrebbe generare un errore IPSec e compromettere la sicurezza del sistema. È quindi consigliabile lasciare in esecuzione il servizio IKEEXT.
|   **Nome del servizio**    |   IKEEXT
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |    
|||         



## <a name="interactive-services-detection"></a>Rilevamento servizi interattivi           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Abilita la notifica dell'input utente per i servizi interattivi, che consente di accedere alle finestre di dialogo create dai servizi interattivi quando vengono visualizzate. Se il servizio viene arrestato, non saranno più disponibili le notifiche relative alle nuove finestre di dialogo dei servizi interattivi, che potrebbero pertanto non essere accessibili. Se il servizio viene disabilitato, non saranno più disponibili né le notifiche, né l'accesso alle nuove finestre di dialogo dei servizi interattivi.
|   **Nome del servizio**    |   UI0Detect
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="internet-connection-sharing-ics"></a>Condivisione connessione Internet (ICS)            

| | |           
|---|---|           
|   **Descrizione del servizio** |   Fornisce servizi di conversione indirizzi di rete, indirizzamento e risoluzione nomi e/o servizi di prevenzione intrusione per una rete domestica o una piccola rete aziendale.
|   **Nome del servizio**    |   SharedAccess
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   È obbligatorio per i client usati come hotspot Wi-Fi e anche su entrambe le estremità della proiezione Miracast. Il servizio ICS può essere bloccato con l'impostazione per oggetti Criteri di gruppo "Proibisci l'uso della Condivisione connessione Internet nella rete del dominio DNS".
|||         



## <a name="ip-helper"></a>Helper IP            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Offre connettività in modalità tunnel usando le tecnologie di transizione IPv6 (6to4, ISATAP, proxy delle porte e Teredo) e IP-HTTPS. Se il servizio viene arrestato, il computer non potrà usufruire dei vantaggi della connettività avanzata garantiti da queste tecnologie.
|   **Nome del servizio**    |   iphlpsvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         




##  <a name="ipsec-policy-agent"></a>Agente criteri IPsec      

| | |           
|---|---|       
|   **Descrizione del servizio** |   IPsec (Internet Protocol security) supporta l'autenticazione peer a livello di rete, l'autenticazione dell'origine dati, l'integrità dei dati, la riservatezza dei dati (crittografia) e la protezione della riproduzione.  Questo servizio applica i criteri IPsec creati mediante lo snap-in Criteri di sicurezza IP o l'utilità da riga di comando "netsh ipsec".  Se il servizio viene arrestato, è possibile che si verifichino problemi di connettività di rete se i criteri richiedono l'uso di IPsec per le connessioni  e inoltre la gestione remota di Windows Firewall non sarà possibile.
|   **Nome del servizio**    |   PolicyAgent
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="kdc-proxy-server-service-kps"></a>Servizio Server proxy KDC (KPS)      

| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio Server proxy KDC viene eseguito su server perimetrali per fornire un proxy per i messaggi del protocollo Kerberos ai controller di dominio della rete aziendale.
|   **Nome del servizio**    |   KPSSVC
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione    
|   **Commenti**    |   
|||         



## <a name="ktmrm-for-distributed-transaction-coordinator"></a>KtmRm per Distributed Transaction Coordinator            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Coordina le transazioni tra Distributed Transaction Coordinator (MSDTC) e Gestione transazioni kernel. Se non è necessario, è consigliabile mantenere arrestato questo servizio. Se è necessario, il servizio verrà avviato automaticamente da MSDTC e Gestione transazioni kernel. Se il servizio viene disabilitato, eventuali transazioni MSDTC che interagiscono con Gestione transazioni kernel avranno esito negativo e i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   KtmRm
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="link-layer-topology-discovery-mapper"></a>Mapper individuazione topologia livelli di collegamento        

| | |       
|---|---|       
|   **Descrizione del servizio** |   Crea una mappa di rete composta da informazioni sulla topologia (connettività) dei PC e dei dispositivi e dai metadati che descrivono i PC e i dispositivi stessi.  Se il servizio viene disabilitato, la mappa di rete non funzionerà correttamente.
|   **Nome del servizio**    |   lltdsvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   OK disabilitare se non sono presenti dipendenze dalla mappa di rete.
|||         



## <a name="local-session-manager"></a>Gestione sessioni locali                    

| | |                   
|---|---|   
|   **Descrizione del servizio** |   Servizio di base di Windows che gestisce le sessioni utente locali. Se il servizio viene arrestato o disabilitato, il sistema diventerà instabile.    
|   **Nome del servizio**    |   LSM |
|   **Installazione**    |   Sempre installato    |
|   **Tipo di avvio**   |   Automatico   |
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||                 



## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Servizio Agente di raccolta standard hub diagnostica Microsoft (R)         

| | |           
|---|---|           
|   **Descrizione del servizio** |   Servizio di base di Windows che gestisce le sessioni utente locali. Se il servizio viene arrestato o disabilitato, il sistema diventerà instabile.
|   **Nome del servizio**    |   LSM
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="microsoft-account-sign-in-assistant"></a>Assistente per l'accesso all'account Microsoft
| | |
|---|---|
|   **Descrizione del servizio** |   Consente l'accesso utente attraverso i servizi di identità per account Microsoft. Se il servizio viene arrestato, gli utenti non saranno in grado di accedere al computer tramite il proprio account Microsoft.
|   **Nome del servizio**    |   wlidsvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   Gli account Microsoft non sono disponibili su Windows Server.
|||



##  <a name="microsoft-app-v-client"></a>Client Microsoft App-V      

| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce le applicazioni virtuali e gli utenti App-V.
|   **Nome del servizio**    |   AppVClient
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Disabilitata
|   **Consiglio**  |   Già disabilitato
|   **Commenti**    |   
|||         



## <a name="microsoft-iscsi-initiator-service"></a>Servizio iniziatore iSCSI Microsoft            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce le sessioni SCSI (iSCSI) Internet tra il computer e i dispositivi di destinazione iSCSI remoti. Se il servizio viene arrestato, il computer non sarà in grado di accedere alle destinazioni iSCSI. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   MSiSCSI
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   I dati di diagnostica indicano che il servizio viene usato sul client e sul server. La disabilitazione del servizio non offre alcun vantaggio.
|||         



## <a name="microsoft-passport"></a>Microsoft Passport           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce l'isolamento dei processi per le chiavi crittografiche usate per l'autenticazione con i provider di identità associati di un utente. Se il servizio è disabilitato, tutti gli utilizzi e la gestione di queste chiavi non saranno disponibili, inclusi l'accesso al computer e le funzionalità Single Sign-On per app e siti Web. Questo servizio viene avviato e arrestato automaticamente. È consigliabile non riconfigurare questo servizio.
|   **Nome del servizio**    |   NgcSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   È necessario per gli accessi PIN/Hello, che non sono supportati sul server.
|||         



## <a name="microsoft-passport-container"></a>Contenitore Microsoft Passport         

| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce le chiavi di identità dell'utente locale usate per autenticare l'utente con i provider di identità, nonché le smart card virtuali TPM. Se il servizio viene disabilitato, le chiavi di identità dell'utente locale e le smart card virtuali TPM non saranno accessibili. È consigliabile non riconfigurare questo servizio.
|   **Nome del servizio**    |   NgcCtnrSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="microsoft-software-shadow-copy-provider"></a>Provider di copie shadow software Microsoft          

| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce le copie shadow del volume basate sul software eseguite dal servizio Copia Shadow del volume. Se il servizio viene arrestato, le copie shadow del volume basate sul software non potranno essere gestite. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   swprv
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="microsoft-storage-spaces-smp"></a>SMP spazi di archiviazione Microsoft         

| | |           
|---|---|       
|   **Descrizione del servizio** |   Servizio host per il provider di gestione per spazi di archiviazione Microsoft. Se il servizio viene arrestato o disabilitato, gli spazi di archiviazione non potranno essere gestiti.
|   **Nome del servizio**    |   smphost
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Senza questo servizio, le API di gestione dell'archiviazione non potranno essere eseguite. Esempio: "Get-WmiObject -class MSFT_Disk -Namespace Root\Microsoft\Windows\Storage".
|||         



## <a name="nettcp-port-sharing-service"></a>Servizio di condivisione porta Net.Tcp         

| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di condividere le porte TCP tramite il protocollo net.tcp.
|   **Nome del servizio**    |   NetTcpPortSharing
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Disabilitata
|   **Consiglio**  |   Già disabilitato
|   **Commenti**    |   
|||         



## <a name="netlogon"></a>Accesso rete         

| | |           
|---|---|           
|   **Descrizione del servizio** |   Mantiene un canale sicuro tra il computer e il controller di dominio per l'autenticazione di utenti e di servizi. Se il servizio viene arrestato, è possibile che il computer non effettui l'autenticazione di utenti e servizi e che il controller di dominio non registri i record DNS. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   Accesso rete
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="network-connection-broker"></a>Gestore connessione rete            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Connessioni del gestore che consentono alle app di Microsoft Store di ricevere notifiche da Internet.
|   **Nome del servizio**    |   NcbService
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



##  <a name="network-connections"></a>Connessioni di rete         

| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce gli oggetti nella cartella Connessioni di rete e telefoniche in cui è possibile visualizzare connessioni di rete locale (LAN) e connessioni remote.
|   **Nome del servizio**    |   Netman
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="network-connectivity-assistant"></a>Assistente connettività di rete      

| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce la notifica dello stato DirectAccess per i componenti dell'interfaccia utente.
|   **Nome del servizio**    |   NcaSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="network-list-service"></a>Servizio Elenco reti        

| | |           
|---|---|   
|   **Descrizione del servizio** |   Identifica le reti a cui è connesso il computer, raccoglie e archivia le proprietà di tali reti e invia notifiche alle applicazioni quando tali proprietà vengono modificate.
|   **Nome del servizio**    |   netprofm
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="network-location-awareness"></a>Riconoscimento presenza in rete           

| | |           
|---|---|       
|   **Descrizione del servizio** |   Raccoglie e archivia le informazioni di configurazione per la rete e notifica ai programmi l'eventuale modifica di tali informazioni. Se il servizio viene arrestato, le informazioni di configurazione potrebbero non essere disponibili. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   NlaSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="network-setup-service"></a>Servizio di installazione della rete       

| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio di installazione della rete gestisce l'installazione dei driver di rete e consente la configurazione di impostazioni di rete di basso livello.  Se il servizio viene arrestato, eventuali installazioni di driver in corso potrebbero essere annullate.
|   **Nome del servizio**    |   NetSetupSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="network-store-interface-service"></a>Servizio Interfaccia archivio di rete      

| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio recapita notifiche di rete, ad esempio aggiunte o eliminazioni di elementi dell'interfaccia, ai client in modalità utente. L'arresto del servizio comporta la perdita della connettività di rete. Se il servizio viene disabilitato, gli altri servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   nsi
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="offline-files"></a>File offline            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio File offline esegue attività di manutenzione della cache File offline, risponde agli eventi di accesso e fine sessione degli utenti, implementa gli elementi interni dell'API pubblica e distribuisce gli eventi alle applicazioni e agli utenti interessati alle attività di File offline e ai cambiamenti di stato della cache.
|   **Nome del servizio**    |   CscService
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Disabilitata
|   **Consiglio**  |   Già disabilitato
|   **Commenti**    |   
|||         



## <a name="optimize-drives"></a>Ottimizza unità          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente al computer di funzionare in modo più efficiente ottimizzando i file nelle unità di archiviazione.
|   **Nome del servizio**    |   defragsvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="performance-counter-dll-host"></a>Host DLL contatore prestazioni         

| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente agli utenti remoti e ai processi a 64 bit di eseguire query sui contatori di prestazioni forniti dalle DLL a 32 bit. Se il servizio viene arrestato, le query sui contatori di prestazioni forniti dalle DLL a 32 bit potranno essere eseguite solo dagli utenti locali e dai processi a 32 bit.
|   **Nome del servizio**    |   PerfHost
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione    
|   **Commenti**    |   
|||         



## <a name="performance-logs--alerts"></a>Avvisi e registri di prestazioni            

| | |           
|---|---|   
|   **Descrizione del servizio** |   Raccoglie dati relativi alle prestazioni dal computer locale o da computer remoti sulla base di parametri di pianificazione preconfigurati, quindi scrive i dati in un registro o attiva un avviso. Se il servizio viene arrestato, le informazioni sulle prestazioni non verranno raccolte. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   pla
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="phone-service"></a>Servizio Telefono       

| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce lo stato della telefonia nel dispositivo.
|   **Nome del servizio**    |   PhoneSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   Usato dalle app VoIP moderne.
|||         



##      <a name="plug-and-play"></a>Plug and Play       

| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente a un computer di riconoscere le modifiche hardware e adattarsi ad esse con un intervento minimo o nullo da parte dell'utente. Se il servizio viene arrestato o disabilitato, il sistema diventerà instabile.
|   **Nome del servizio**    |   PlugPlay
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="portable-device-enumerator-service"></a>Servizio enumeratore dispositivi mobili           

| | |           
|---|---|       
|   **Descrizione del servizio** |   Applica Criteri di gruppo per i dispositivi di archiviazione di massa rimovibili. Consente ad applicazioni quali Windows Media Player e Importazione guidata immagini di trasferire e sincronizzare il contenuto tramite dispositivi di archiviazione di massa rimovibili.
|   **Nome del servizio**    |   WPDBusEnum
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="power"></a>Potenza            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce criteri per il risparmio energia e il recapito di notifiche relative a tali criteri.
|   **Nome del servizio**    |   Potenza
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="print-spooler"></a>Spooler di stampa            

| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio effettua lo spooling dei processi di stampa e gestisce l'interazione con la stampante.  Se il servizio viene disabilitato, non sarà più possibile stampare o visualizzare le stampanti.
|   **Nome del servizio**    |   Spooler
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   OK disabilitare se non si tratta di un server di stampa o di un controller di dominio.
|   **Commenti**    |   In un controller di dominio, l'installazione del ruolo del controller di dominio aggiunge un thread per il servizio spooler che è responsabile dell'eliminazione degli oggetti coda di stampa obsoleti da Active Directory.  Se il servizio spooler non è in esecuzione su almeno un controller di dominio in ogni sito, Active Directory non è in grado di rimuovere le code precedenti che non esistono più. [https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/](https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/ )
|||         



##  <a name="printer-extensions-and-notifications"></a>Estensioni e notifiche della stampante        

| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio apre le finestre di dialogo personalizzate della stampante e gestisce le notifiche da una stampante o un server di stampa remoto. Se il servizio viene disabilitato, non sarà possibile visualizzare le estensioni o le notifiche della stampante.
|   **Nome del servizio**    |   PrintNotify
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare se non si tratta di un server di stampa.
|   **Commenti**    |   
|||         



##  <a name="problem-reports-and-solutions-control-panel-support"></a>Segnalazioni di problemi e soluzioni nel Pannello di controllo     

| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio fornisce supporto per la visualizzazione, l'invio e l'eliminazione di rapporti sui problemi a livello di sistema per Segnalazioni di problemi e soluzioni nel Pannello di controllo.
|   **Nome del servizio**    |   wercplsupport
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="program-compatibility-assistant-service"></a>Servizio Risoluzione problemi compatibilità programmi     

| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio fornisce supporto per Risoluzione problemi compatibilità programmi,  che effettua il monitoraggio dei programmi installati ed eseguiti dall'utente e rileva i problemi di compatibilità noti. Se il servizio viene arrestato, Risoluzione problemi compatibilità programmi non funzionerà correttamente.
|   **Nome del servizio**    |   PcaSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



##  <a name="quality-windows-audio-video-experience"></a>Servizio audio/video Windows di qualità      

| | |           
|---|---|   
|   **Descrizione del servizio** |   Servizio audio/video Windows di qualità (qWave) è una piattaforma di rete per applicazioni che usano il flusso di dati audio/video (AV) in reti domestiche IP. qWave migliora le prestazioni e l'affidabilità durante l'utilizzo del flusso AV, garantendo la qualità del servizio (QoS, Quality of Service) sulla rete per le applicazioni AV. Offre infatti meccanismi per il controllo dell'ammissione, il monitoraggio e l'imposizione in fase di esecuzione, il feedback dell'applicazione e l'impostazione della priorità del traffico.
|   **Nome del servizio**    |   QWAVE
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   Servizio QoS sul lato client
|||         



##      <a name="radio-management-service"></a>Servizio di gestione radio        

| | |           
|---|---|   
|   **Descrizione del servizio** |   Servizio di gestione radio e modalità aereo
|   **Nome del servizio**    |   RmSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="remote-access-auto-connection-manager"></a>Auto Connection Manager di Accesso remoto            

| | |           
|---|---|   
|   **Descrizione del servizio** |   Crea una connessione a una rete remota ogni volta che un programma fa riferimento a un DNS o a un nome o indirizzo NetBIOS remoto.
|   **Nome del servizio**    |   RasAuto
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="remote-access-connection-manager"></a>Connection Manager di Accesso remoto         

| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce le connessioni remote e di rete privata virtuale (VPN) dal computer a Internet o ad altre reti remote. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   RasMan
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="remote-desktop-configuration"></a>Configurazione di Desktop remoto         

| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio Configurazione Desktop remoto è responsabile di tutte le attività di manutenzione della configurazione e delle sessioni correlate a Servizi Desktop remoto e Desktop remoto che richiedono il contesto SYSTEM. Sono inclusi i temi di Desktop remoto, i certificati di Desktop remoto e le cartelle temporanee per sessione.
|   **Nome del servizio**    |   SessionEnv
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   
|||         



## <a name="remote-desktop-services"></a>Servizi Desktop remoto          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente agli utenti di connettersi in modo interattivo a un computer remoto. Desktop remoto e il server Host sessione Desktop remoto dipendono da questo servizio.  Per impedire l'utilizzo remoto del computer, è necessario deselezionare le caselle di controllo nella scheda Connessione remota della finestra delle proprietà Sistema nel Pannello di controllo.
|   **Nome del servizio**    |   TermService
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   
|||         



##  <a name="remote-desktop-services-usermode-port-redirector"></a>Redirector porta UserMode di Servizi Desktop remoto        

| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente il reindirizzamento di stampanti, unità o porte per le connessioni Desktop remoto.
|   **Nome del servizio**    |   UmRdpService
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Supporta i reindirizzamenti sul lato server della connessione.
|||         



## <a name="remote-procedure-call-rpc"></a>RPC (Remote Procedure Call)          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Il servizio RPCSS corrisponde a Gestione controllo servizi per i server COM e DCOM. Questo servizio esegue richieste di attivazione degli oggetti, funzionalità di risoluzione per l'object exporter e funzioni distribuite di garbage collection per i server COM e DCOM. Se il servizio viene arrestato o disabilitato, i programmi che usano COM o DCOM non funzioneranno correttamente. È fortemente consigliabile mantenere in esecuzione il servizio RPCSS.
|   **Nome del servizio**    |   RpcSs
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="remote-procedure-call-rpc-locator"></a>RPC Locator             

| | |               
|---|---|   
|   **Descrizione del servizio** |   In Windows 2003 e nelle versioni precedenti di Windows, il servizio RPC (Remote Procedure Call) Locator gestisce il database del servizio dei nomi RPC. In Windows Vista e nelle versioni successive di Windows, questo servizio non offre alcuna funzionalità ed è disponibile per compatibilità con le applicazioni.   |
|   **Nome del servizio**    |   RpcLocator  |
|   **Installazione**    |   Solo con Esperienza desktop    |
|   **Tipo di avvio**   |   Manuale  |
|   **Consiglio**  | Nessuna indicazione   |
|   **Commenti**    |       |
|||             



## <a name="remote-registry"></a>Registro di sistema remoto          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente agli utenti remoti di modificare le impostazioni del Registro di sistema del computer in uso. Se il servizio viene arrestato, il Registro di sistema potrà essere modificato soltanto dagli utenti del computer. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   RemoteRegistry
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   
|||         



##  <a name="resultant-set-of-policy-provider"></a>Provider Gruppo di criteri risultante            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce un servizio di rete che elabora richieste per simulare l'applicazione di impostazioni dei Criteri di gruppo per un utente o computer di destinazione in varie situazioni, oltre a calcolare le impostazioni del gruppo di criteri risultante.
|   **Nome del servizio**    |   RSoPProv
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |Nessuna indicazione    
|   **Commenti**    |   
|||         



## <a name="routing-and-remote-access"></a>Routing e Accesso remoto            

| | |           
|---|---|   
|   **Descrizione del servizio** |   Offre servizi di routing ad aziende in ambienti LAN e WAN.
|   **Nome del servizio**    |   RemoteAccess
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Disabilitata
|   **Consiglio**  |   Già disabilitato
|   **Commenti**    |   Già disabilitato
|||         



## <a name="rpc-endpoint-mapper"></a>Agente mapping endpoint RPC          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Risolve gli identificatori di interfaccia RPC in endpoint del trasporto. Se il servizio viene arrestato o disabilitato, i programmi che usano servizi RPC (Remote Procedure Call) non funzioneranno in modo corretto.
|   **Nome del servizio**    |   RpcEptMapper
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="secondary-logon"></a>Accesso secondario     

| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di avviare i processi con credenziali alternative. Se il servizio viene arrestato, questo tipo di accesso non sarà disponibile. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   seclogon
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="secure-socket-tunneling-protocol-service"></a>Servizio SSTP (Secure Socket Tunneling Protocol)            

| | |               
|---|---|       
|   **Descrizione del servizio** |   Fornisce supporto per il protocollo SSTP (Secure Socket Tunneling Protocol) per la connessione a computer remoti tramite VPN. Se il servizio viene disabilitato, gli utenti non potranno usare SSTP per l'accesso a server remoti.    |
|   **Nome del servizio**    |   SstpSvc |
|   **Installazione**    |   Sempre installato    |
|   **Tipo di avvio**   |   Manuale  |
|   **Consiglio**  |   Non disabilitare  |
|   **Commenti**    |   La disabilitazione causa l'interruzione di RRAS.   |
|||             



## <a name="security-accounts-manager"></a>Sistema di gestione degli account di sicurezza (SAM)            

| | |           
|---|---|       
|   **Descrizione del servizio** |   L'avvio di questo servizio segnala ad altri servizi che il sistema di gestione degli account di sicurezza (SAM) è pronto ad accettare richieste.  Se il servizio viene disabilitato, non sarà possibile notificare tale stato del sistema di gestione degli account di sicurezza agli altri servizi del sistema, che potrebbero pertanto non essere avviati correttamente. Il servizio non dovrebbe essere disabilitato.
|   **Nome del servizio**    |   SamSs
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Non disabilitare
|   **Commenti**    |   
|||         



## <a name="sensor-data-service"></a>Servizio dati sensore  

| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce i dati provenienti da un'ampia gamma di sensori.
|   **Nome del servizio**    |   SensorDataService
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="sensor-monitoring-service"></a>Servizio monitoraggio sensori            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Esegue il monitoraggio di diversi sensori per esporre i dati e adattarli allo stato del sistema e dell'utente.  Se il servizio viene arrestato o disabilitato, la luminosità dello schermo non verrà regolata in base alle condizioni di illuminazione. L'arresto del servizio può inoltre influire su altre funzionalità e caratteristiche del sistema.
|   **Nome del servizio**    |   SensrSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         

## <a name="sensor-service"></a>Servizio sensori

| | |
|---|---|
|   **Descrizione del servizio** |   Servizio per i sensori che gestisce la funzionalità di diversi sensori. Gestisce l'orientamento dispositivo semplice (SDO, Simple Device Orientation) e la cronologia per i sensori. Carica il sensore SDO che segnala le modifiche di orientamento del dispositivo.  Se il servizio viene arrestato o disabilitato, il sensore SDO non verrà caricato e quindi non verrà eseguita la rotazione automatica. Verrà arrestata anche la raccolta cronologia dai sensori.
|   **Nome del servizio**    |   SensorService
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |
|||

## <a name="server"></a>Server           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Supporta la condivisione in rete di file, stampa e named pipe per il computer in uso. Se il servizio viene arrestato, queste funzioni non saranno disponibili. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   LanmanServer
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   È necessario per la gestione remota, IPC$ e la condivisione di file SMB.
|||         



## <a name="shell-hardware-detection"></a>Rilevamento hardware shell             

| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce notifiche di eventi hardware AutoPlay.
|   **Nome del servizio**    |   ShellHWDetection
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="smart-card"></a>Smart card           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce l'accesso alle smart card lette dal computer. Se il servizio viene arrestato, il computer non sarà in grado di leggere le smart card. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   SCardSvr
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Disabilitata
|   **Consiglio**  |   Già disabilitato
|   **Commenti**    |   
|||         



## <a name="smart-card-device-enumeration-service"></a>Servizio di enumerazione dispositivo smart card                    

| | |               
|---|---|       
|   **Descrizione del servizio** |   Crea nodi del dispositivo software per tutti i lettori di smart card accessibili in una determinata sessione. Se il servizio viene disabilitato, le API di WinRT non saranno in grado di enumerare i lettori di smart card.   |
|   **Nome del servizio**    |   ScDeviceEnum    |
|   **Installazione**    |   Sempre installato    |
|   **Tipo di avvio**   |   Manuale  |
|   **Consiglio**  |   OK disabilitare   |
|   **Commenti**    |   È necessario quasi esclusivamente per le app WinRT.    |
|||             



## <a name="smart-card-removal-policy"></a>Criterio rimozione smart card        

| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di configurare il sistema in modo che il desktop dell'utente venga bloccato alla rimozione della smart card.
|   **Nome del servizio**    |   SCPolicySvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="snmp-trap"></a>Trap SNMP            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Riceve messaggi trap generati da agenti Simple Network Management Protocol (SNMP) locali o remoti e inoltra i messaggi a programmi di gestione SNMP in esecuzione su questo computer. Se il servizio viene arrestato, le applicazioni basate su SNMP nel computer non riceveranno messaggi trap SNMP. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   SNMPTRAP
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="software-protection"></a>Protezione software             

| | |           
|---|---|       
|   **Descrizione del servizio** |   Abilita il download, l'installazione e l'applicazione di licenze digitali per Windows e le applicazioni Windows. Se il servizio viene disabilitato, è possibile che il sistema operativo e le applicazioni con licenza vengano eseguiti in modalità di notifica. È consigliabile non disabilitare il servizio di protezione software.
|   **Nome del servizio**    |   sppsvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="special-administration-console-helper"></a>Helper console di amministrazione speciale        

| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente agli amministratori di accedere in remoto a un prompt dei comandi usando Servizi di gestione emergenze.
|   **Nome del servizio**    |   sacsvr
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="spot-verifier"></a>Verifica errori NTFS            

| | |           
|---|---|   
|   **Descrizione del servizio** |   Verifica il potenziale danneggiamento del file system.
|   **Nome del servizio**    |   svsvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="ssdp-discovery"></a>Individuazione SSDP           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Individua i servizi e i dispositivi di rete che usano il protocollo di individuazione SSDP, ad esempio i dispositivi UPnP. Annuncia inoltre i dispositivi e i servizi SSDP in esecuzione nel computer locale. Se questo servizio viene arrestato, i dispositivi basati su SSDP non verranno individuati. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   SSDPSRV
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="state-repository-service"></a>Servizio repository stati         

| | |           
|---|---|   
|   **Descrizione del servizio** |   Offre il supporto infrastrutturale necessario per il modello applicativo.
|   **Nome del servizio**    |   StateRepository
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="still-image-acquisition-events"></a>Eventi acquisizione Still Image

| | |           
|---|---|   
|   **Descrizione del servizio** |   Avvia le applicazioni associate agli eventi di acquisizione fermo immagine.
|   **Nome del servizio**    |   WiaRpc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="storage-service"></a>Servizio di archiviazione          

| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente l'abilitazione dei servizi per le impostazioni di archiviazione e l'espansione dell'archiviazione esterna.
|   **Nome del servizio**    |   StorSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="storage-tiers-management"></a>Gestione livelli di archiviazione        

| | |           
|---|---|   
|   **Descrizione del servizio** |   Ottimizza il posizionamento dei dati nei livelli degli spazi di archiviazione a livelli nel sistema.
|   **Nome del servizio**    |   TieringEngineService
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="superfetch"></a>Ottimizzazione avvio          

| | |           
|---|---|       
|   **Descrizione del servizio** |   Mantiene e migliora nel tempo le prestazioni del sistema.
|   **Nome del servizio**    |   SysMain
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="sync-host"></a>Sincronizza host            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio sincronizza la posta elettronica, i contatti, il calendario e diversi altri dati utente. La posta elettronica e altre applicazioni dipendenti da questa funzionalità non funzioneranno correttamente se il servizio non è in esecuzione.
|   **Nome del servizio**    |   OneSyncSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   Modello di servizio utente
|||         



## <a name="system-event-notification-service"></a>Servizio di notifica eventi di sistema            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Esegue il monitoraggio degli eventi di sistema e li notifica ai sottoscrittori di COM+ Event System.
|   **Nome del servizio**    |   SENS
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="system-events-broker"></a>Gestore eventi di sistema             

| | |           
|---|---|       
|   **Descrizione del servizio** |   Coordina l'esecuzione di processi in background per l'applicazione WinRT. Se il servizio viene arrestato o disabilitato, i processi in background potrebbero non essere attivati.
|   **Nome del servizio**    |   SystemEventsBroker
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Anche se la descrizione sottintende che sia destinato solo alle app WinRT, il servizio è necessario per l'utilità di pianificazione, il servizio dell'infrastruttura di gestione e altri componenti interni.
|||         



## <a name="task-scheduler"></a>Utilità di pianificazione           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente a un utente di configurare e pianificare attività automatiche nel computer in uso. Il servizio ospita inoltre più attività critiche per il sistema Windows. Se il servizio viene arrestato o disabilitato, le attività non verranno eseguite in base agli orari pianificati. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   Pianificazione
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="tcpip-netbios-helper"></a>Helper NetBIOS di TCP/IP            

| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce supporto al servizio NetBIOS su TCP/IP (NetBT) e alla risoluzione di nomi NetBIOS per i client della rete, consentendo la condivisione di file, la stampa e l'accesso alla rete da parte degli utenti. Se il servizio viene arrestato, queste funzioni potrebbero non essere disponibili. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   lmhosts
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="telephony"></a>Telephony           

| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce il supporto per TAPI (Telephony API) ai programmi che controllano i dispositivi di telefonia sul computer locale e, attraverso la LAN, agli altri server che eseguono il servizio.
|   **Nome del servizio**    |   TapiSrv
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   La disabilitazione causa l'interruzione di RRAS.
|||         



## <a name="themes"></a>Temi           

| | |           
|---|---|
|   **Descrizione del servizio** |   Consente la gestione dei temi tramite l'esperienza utente.
|   **Nome del servizio**    |   Temi
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Non è possibile impostare i temi di accessibilità quando il servizio è disabilitato.
|||         



## <a name="tile-data-model-server"></a>Server modello dati sezioni           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Server delle sezioni per gli aggiornamenti delle sezioni.
|   **Nome del servizio**    |   tiledatamodelsvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   La disabilitazione del servizio causa l'interruzione del menu Start.
|||         



##  <a name="time-broker"></a>Gestore tempo     

| | |           
|---|---|       
|   **Descrizione del servizio** |   Coordina l'esecuzione di processi in background per l'applicazione WinRT. Se il servizio viene arrestato o disabilitato, i processi in background potrebbero non essere attivati.
|   **Nome del servizio**    |   TimeBrokerSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Anche se la descrizione sottintende che sia destinato solo alle app WinRT, il servizio è necessario per l'utilità di pianificazione, il servizio dell'infrastruttura di gestione e altri componenti interni.
|||         



## <a name="touch-keyboard-and-handwriting-panel-service"></a>Servizio tastiera virtuale e pannello per la grafia         

| | |           
|---|---|   
|   **Descrizione del servizio** |   Abilita le funzionalità di input penna della tastiera virtuale e del pannello per la grafia.
|   **Nome del servizio**    |   TabletInputService
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="update-orchestrator-service-for-windows-update"></a>Servizio agente di orchestrazione aggiornamenti per Windows Update           

| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce gli aggiornamenti di Windows. Se viene arrestato, i dispositivi non potranno scaricare e installare gli aggiornamenti più recenti.
|   **Nome del servizio**    |   UsoSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   La descrizione del servizio non è presente nella versione 1607. Windows Update (incluso WSUS) dipende da questo servizio.
|||         



## <a name="upnp-device-host"></a>Host di dispositivi UPnP         

| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente l'hosting dei dispositivi UPnP nel computer. Se il servizio viene arrestato, gli eventuali dispositivi UPnP ospitati smetteranno di funzionare e non sarà possibile aggiungere altri dispositivi ospitati. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   upnphost
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="user-access-logging-service"></a>Servizio registrazione accessi utente          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio registra le richieste di accesso client univoche dei prodotti e ruoli installati nel server locale, sotto forma di indirizzi IP e nomi utente. Tali informazioni possono essere recuperate, tramite query PowerShell, dagli amministratori che hanno l'esigenza di quantificare le richieste client di software server per la gestione delle licenze CAL (Client Access License) offline. Se il servizio viene disabilitato, le richieste client non verranno registrate e non potranno essere recuperate tramite query PowerShell. L'arresto del servizio non influisce sul recupero tramite query dei dati cronologici (per la procedura di eliminazione dei dati cronologici, vedi la documentazione di supporto). L'amministratore del sistema locale deve verificare le condizioni della licenza di Windows Server per determinare il numero di licenze CAL (Client Access License) necessario affinché il software server risulti regolarmente coperto da licenza. L'uso del servizio UAL non esonera da tale obbligo.
|   **Nome del servizio**    |   UALSVC
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="user-data-access"></a>Accesso dati utente        

| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente alle app di accedere ai dati utente strutturati, incluse le informazioni su contatti, calendari, messaggi e altro contenuto. Se il servizio viene arrestato o disabilitato, le app che usano questi dati potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   UserDataSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   Modello di servizio utente
|||         



## <a name="user-data-storage"></a>Archiviazione dati utente            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce l'archiviazione dei dati utente strutturati, incluse le informazioni su contatti, calendari, messaggi e altro contenuto. Se il servizio viene arrestato o disabilitato, le app che usano questi dati potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   UnistoreSvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   Modello di servizio utente
|||         



## <a name="user-experience-virtualization-service"></a>Servizio User Experience Virtualization           

| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce il supporto per il roaming delle impostazioni delle applicazioni e del sistema operativo.
|   **Nome del servizio**    |   UevAgentService
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Disabilitata
|   **Consiglio**  |   Già disabilitato
|   **Commenti**    |   
|||         



##  <a name="user-manager"></a>Gestione utenti        

| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestione utenti fornisce i componenti di runtime necessari per l'interazione tra più utenti.  Se il servizio viene arrestato, alcune applicazioni potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   UserManager
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="user-profile-service"></a>Servizio profili utente         

| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio è responsabile di caricare e scaricare i profili utente. Se il servizio viene arrestato o disabilitato, gli utenti non potranno più accedere o disconnettersi dal sistema, le app potrebbero avere problemi a ottenere i dati utente e i componenti registrati per la ricezione delle notifiche degli eventi relativi ai profili non le riceveranno.
|   **Nome del servizio**    |   ProfSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="virtual-disk"></a>Disco virtuale             

| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce servizi di gestione per dischi, volumi, file system e array di archiviazione.
|   **Nome del servizio**    |   vds
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Nessuna indicazione
|   **Commenti**    |   
|||         



## <a name="volume-shadow-copy"></a>Copia shadow del volume           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce e implementa le copie shadow del volume usate a scopo di backup e altro. Se il servizio viene arrestato, le copie shadow non saranno disponibili per il backup e l'esecuzione del backup potrebbe non riuscire. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   VSS
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Nessuna indicazione
|   **Commenti**    |   
|||         



##  <a name="walletservice"></a>WalletService           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Ospita gli oggetti usati dai client del portafoglio.
|   **Nome del servizio**    |   WalletService
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="windows-audio"></a>Audio di Windows            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Gestisce l'audio per le applicazioni basate su Windows.  Se il servizio viene arrestato, gli effetti e i dispositivi audio non funzioneranno correttamente.  Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   Audiosrv
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="windows-audio-endpoint-builder"></a>Generatore endpoint audio Windows           

| | |           
|---|---|
|   **Descrizione del servizio** |   Gestisce i dispositivi audio per il servizio Audio Windows.  Se il servizio viene arrestato, gli effetti e i dispositivi audio non funzioneranno correttamente.  Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   AudioEndpointBuilder
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="windows-biometric-service"></a>Servizio di biometria Windows            

| | |           
|---|---|   
|   **Descrizione del servizio** |   Il Servizio di biometria Windows consente alle applicazioni client di acquisire, confrontare, modificare e archiviare dati biometrici senza accedere direttamente all'hardware o a campioni biometrici. Il servizio è ospitato in un processo SVCHOST privilegiato.
|   **Nome del servizio**    |   WbioSrvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="windows-camera-frame-server"></a>Server di fotogrammi fotocamera Windows         

| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente a più client di accedere ai fotogrammi video dalle fotocamere.
|   **Nome del servizio**    |   FrameServer
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="windows-connection-manager"></a>Gestione connessioni Windows           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Stabilisce decisioni di connessione/disconnessione automatica in base alle opzioni di connettività di rete attualmente disponibili per il PC e consente la gestione della connettività di rete in base alle impostazioni di Criteri di gruppo.
|   **Nome del servizio**    |   Wcmsvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="windows-defender-network-inspection-service"></a>Servizio Controllo rete di Windows Defender          

| | |           
|---|---|       
|   **Descrizione del servizio** |   Contribuisce a proteggere da tentativi di intrusione nei confronti di vulnerabilità note e nuove dei protocolli di rete.
|   **Nome del servizio**    |   WdNisSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione    
|   **Commenti**    |   
|||         



## <a name="windows-defender-service"></a>Servizio Windows Defender         

| | |           
|---|---|       
|   **Descrizione del servizio** |   Contribuisce a proteggere gli utenti da malware e altro software potenzialmente indesiderato.
|   **Nome del servizio**    |   WinDefend
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows Driver Foundation - Framework driver modalità utente           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Crea e gestisce i processi dei driver in modalità utente. Questo servizio non può essere arrestato.
|   **Nome del servizio**    |   wudfsvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="windows-encryption-provider-host-service"></a>Servizio host del provider di crittografia di Windows     

| | |           
|---|---|   
|   **Descrizione del servizio** |   Funzionalità correlate dei gestori del servizio host del provider di crittografia di Windows fornite da provider di crittografia di terze parti per processi che devono valutare e applicare criteri EAS. Se il servizio viene arrestato, le verifiche di conformità EAS stabilite dagli account di posta elettronica connessi saranno compromessi.
|   **Nome del servizio**    |   WEPHOSTSVC
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="windows-error-reporting-service"></a>Servizio Segnalazione errori Windows          

| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente di segnalare gli errori quando i programmi smettono di funzionare o si bloccano e di ricevere soluzioni. Consente inoltre di generare registri per i servizi di diagnostica e ripristino. Se il servizio viene arrestato, è possibile che la segnalazione errori non funzioni correttamente e che i risultati dei servizi di diagnostica e ripristino non vengano visualizzati.
|   **Nome del servizio**    |   WerSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Raccoglie e invia i dati di arresto anomalo/blocco usati da Microsoft e da fornitori di software o hardware indipendenti di terze parti. I dati vengono usati per diagnosticare i bug che causano l'arresto anomalo del sistema, che possono includere i bug di sicurezza. È necessario anche per la segnalazione di errori a un server interno.
|||         



## <a name="windows-event-collector"></a>Raccolta eventi Windows          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio consente la gestione delle sottoscrizioni permanenti agli eventi da origini remote che supportano il protocollo WS-Management, tra cui registri eventi di Windows Vista, nonché origini eventi hardware e IPMI. Il servizio archivia gli eventi inoltrati in un registro eventi locale. Se il servizio viene arrestato o disabilitato, non sarà possibile creare sottoscrizioni agli eventi e gli eventi inoltrati non verranno accettati.
|   **Nome del servizio**    |   Wecsvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Raccoglie gli eventi ETW (inclusi gli eventi di sicurezza) per la gestibilità e la diagnostica.  Numerosi strumenti di terze parti e funzionalità si basano su questo servizio, inclusi gli strumenti di controllo della sicurezza.
|||         



## <a name="windows-event-log"></a>Registro eventi di Windows            

| | |           
|---|---|       
|   **Descrizione del servizio** |   Questo servizio gestisce gli eventi e i registri eventi e supporta la registrazione, la ricerca e la sottoscrizione di eventi, nonché l'archiviazione di registri eventi e la gestione di metadati di eventi. Il servizio è in grado di visualizzare eventi in formato XML e testo normale. L'arresto del servizio può compromettere la sicurezza e l'affidabilità del sistema.
|   **Nome del servizio**    |   EventLog
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="windows-firewall"></a>Windows Firewall         

| | |           
|---|---|   
|   **Descrizione del servizio** |   Windows Firewall facilita la protezione del computer dagli accessi utente non autorizzati da Internet o da una rete.
|   **Nome del servizio**    |   MpsSvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="windows-font-cache-service"></a>Servizio cache tipi di carattere Windows      

| | |           
|---|---|   
|   **Descrizione del servizio** |   Ottimizza le prestazioni delle applicazioni memorizzando nella cache i dati dei tipi di carattere usati comunemente. Se non è già in esecuzione, il servizio verrà avviato dalle applicazioni. Il servizio può essere disabilitato, ma le prestazioni delle applicazioni risulteranno ridotte.
|   **Nome del servizio**    |   FontCache
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="windows-image-acquisition-wia"></a>Acquisizione di immagini di Windows (WIA)          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Offre servizi di acquisizione immagini per scanner e fotocamere digitali.
|   **Nome del servizio**    |   stisvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



##  <a name="windows-insider-service"></a>Servizio Windows Insider     

| | |           
|---|---|   
|   **Descrizione del servizio** |   wisvc
|   **Nome del servizio**    |   wisvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   Il server non supporta la distribuzione di versioni di anteprima, pertanto si verifica una fase senza operazioni sul server. La funzionalità può essere disabilitata anche tramite Criteri di gruppo.
|||         



##  <a name="windows-installer"></a>Windows Installer       

| | |           
|---|---|
|   **Descrizione del servizio** |   Aggiunge, modifica e rimuove applicazioni fornite come pacchetto di Windows Installer (*.msi, *.msp). Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   msiserver
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="windows-license-manager-service"></a>Servizio Gestione licenze Windows          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Offre supporto infrastrutturale per Microsoft Store.  Il servizio viene avviato su richiesta e, se disabilitato, il contenuto acquisito tramite Windows Store non funzionerà correttamente.
|   **Nome del servizio**    |   LicenseManager
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="windows-management-instrumentation"></a>Strumentazione gestione Windows (WMI)       

| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce un modello di interfacce e di oggetti comune per accedere alle informazioni di gestione sul sistema operativo, i dispositivi, le applicazioni e i servizi. Se il servizio viene arrestato, la maggior parte del software basato su Windows non funzionerà correttamente. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   Winmgmt
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



##  <a name="windows-mobile-hotspot-service"></a>Servizio hotspot di Windows Mobile          

| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente la condivisione della connessione alla rete dati con un altro dispositivo.
|   **Nome del servizio**    |   icssvc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   
|||         



## <a name="windows-modules-installer"></a>Programma di installazione dei moduli di Windows        

| | |           
|---|---|   
|   **Descrizione del servizio** |   Consente l'installazione, la modifica e la rimozione degli aggiornamenti e dei componenti facoltativi di Windows. Se questo servizio viene disabilitato, è possibile che l'installazione o la disinstallazione degli aggiornamenti di Windows non venga eseguita per questo computer.
|   **Nome del servizio**    |   TrustedInstaller
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="windows-push-notifications-system-service"></a>Servizio di sistema notifiche Push Windows            

| | |           
|---|---|
|   **Descrizione del servizio** |   Questo servizio viene eseguito nella sessione 0 e ospita la piattaforma di notifica e il provider di connessione che gestisce la connessione tra il dispositivo e il server WNS.
|   **Nome del servizio**    |   WpnService
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   È necessario per i riquadri animati e per altre funzionalità.
|||         



## <a name="windows-push-notifications-user-service"></a>Servizio utente notifica Push Windows          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio ospita la piattaforma di notifica Windows che fornisce il supporto per le notifiche locali e push. Le notifiche supportate sono riquadri, avvisi popup e notifiche non elaborate.
|   **Nome del servizio**    |   WpnUserService
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   OK disabilitare
|   **Commenti**    |   Modello di servizio utente
|||         



## <a name="windows-remote-management-ws-management"></a>Gestione remota Windows (WS-Management)
| | |
|---|---|
|   **Descrizione del servizio** |   Il servizio Gestione remota di Windows (WinRM) implementa il protocollo WS-Management per la gestione remota. WS-Management è un protocollo di servizi Web standard usato per la gestione remota del software e dell'hardware. Il servizio WinRM ascolta le richieste WS-Management sulla rete e le elabora. Il servizio WinRM deve essere configurato con uno strumento da riga di comando winrm.cmd oppure mediante Criteri di gruppo per poter eseguire l'ascolto sulla rete. Il servizio WinRM fornisce l'accesso ai dati WMI e abilita la raccolta di eventi. Per la raccolta di eventi e la sottoscrizione agli eventi il servizio deve essere in esecuzione. I messaggi WinRM usano HTTP e HTTPS come trasporto. Il servizio WinRM non dipende da IIS ma è preconfigurato per condividere una porta con IIS nello stesso computer.  Il servizio WinRM si riserva il prefisso URL /wsman. Per evitare conflitti con IIS, gli amministratori devono verificare che nessun sito Web ospitato su IIS usi il prefisso URL /wsman.
|   **Nome del servizio**    |   WinRM
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   È necessario per la gestione remota.
|||



##  <a name="windows-search"></a>Windows Search      

| | |           
|---|---|       
|   **Descrizione del servizio** |   Fornisce indicizzazione di contenuto, memorizzazione di proprietà nella cache e risultati di ricerca per file, posta elettronica e altro contenuto.
|   **Nome del servizio**    |   WSearch
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Disabilitata
|   **Consiglio**  |   Già disabilitato
|   **Commenti**    |   
|||         



##  <a name="windows-time"></a>Ora di Windows        

| | |           
|---|---|   
|   **Descrizione del servizio** |   Gestisce la sincronizzazione di data e ora su tutti i client e i server della rete. Se il servizio viene arrestato, la sincronizzazione di data e ora non sarà disponibile. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   W32Time
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione
|   **Commenti**    |   
|||         



## <a name="windows-update"></a>Windows Update           

| | |           
|---|---|       
|   **Descrizione del servizio** |   Consente il rilevamento, il download e l'installazione di aggiornamenti per Windows e altri programmi. Se il servizio viene disabilitato, gli utenti del computer non saranno in grado di usare Windows Update o la relativa funzionalità di aggiornamento automatico e i programmi non potranno usare l'API Agente di Windows Update (Windows Update Agent, WUA).
|   **Nome del servizio**    |   wuauserv
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="winhttp-web-proxy-auto-discovery-service"></a>Servizio rilevamento automatico proxy WinHTTP         

| | |           
|---|---|   
|   **Descrizione del servizio** |   WinHTTP implementa lo stack HTTP client e offre agli sviluppatori un'API Win32 e un componente di automazione COM per l'invio delle richieste HTTP e la ricezione delle risposte. WinHTTP offre inoltre supporto per l'individuazione automatica di una configurazione proxy tramite una specifica implementazione del protocollo WPAD (Web Proxy Auto-Discovery).
|   **Nome del servizio**    |   WinHttpAutoProxySvc
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Non disabilitare
|   **Commenti**    |   Qualsiasi implementazione che usa lo stack di rete può avere una dipendenza funzionale da questo servizio. Molte organizzazioni si basano su questo servizio per configurare il routing del proxy HTTP delle reti interne.  Senza questo servizio, le connessioni HTTP dalla rete interna a Internet avranno esito negativo.
|||         



## <a name="wired-autoconfig"></a>Configurazione automatica reti cablate         

| | |           
|---|---|       
|   **Descrizione del servizio** |   Il servizio Configurazione automatica reti cablate (DOT3SVC) è responsabile dell'esecuzione dell'autenticazione IEEE 802.1X su interfacce Ethernet. Se la rete cablata in uso impone l'autenticazione 802.1X, è necessario configurare il servizio DOT3SVC in modo che venga eseguito per attivare la connettività di livello 2 e/o per fornire l'accesso alle risorse di rete. Il servizio DOT3SVC non interessa in alcun modo le reti cablate che non impongono l'autenticazione 802.1X.
|   **Nome del servizio**    |   dot3svc
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione   
|   **Commenti**    |   
|||         



## <a name="wmi-performance-adapter"></a>Scheda delle prestazioni WMI          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce informazioni relative alla libreria delle prestazioni dai provider di Strumentazione gestione Windows (WMI) ai client nella rete. Il servizio funziona solo quando l'helper Dati di prestazione è attivato.
|   **Nome del servizio**    |   wmiApSrv
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  | Nessuna indicazione       
|   **Commenti**    |   
|||         



## <a name="workstation"></a>Workstation          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Crea e gestisce le connessioni di rete tra client e server remoti usando il protocollo SMB. Se il servizio viene arrestato, le connessioni non saranno disponibili. Se il servizio viene disabilitato, i servizi che dipendono esplicitamente da esso non potranno essere avviati.
|   **Nome del servizio**    |   LanmanWorkstation
|   **Installazione**    |   Sempre installato
|   **Tipo di avvio**   |   Automatico
|   **Consiglio**  | Nessuna indicazione       
|   **Commenti**    |   
|||         



## <a name="xbox-live-auth-manager"></a>Gestione autenticazione Xbox Live           

| | |           
|---|---|   
|   **Descrizione del servizio** |   Fornisce servizi di autenticazione e autorizzazione che consentono di interagire con Xbox Live. Se il servizio viene arrestato, alcune applicazioni potrebbero non funzionare correttamente.
|   **Nome del servizio**    |   XblAuthManager
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Opportuno disabilitare
|   **Commenti**    |   
|||         



## <a name="xbox-live-game-save"></a>Giochi salvati su Xbox Live          

| | |           
|---|---|   
|   **Descrizione del servizio** |   Questo servizio consente di sincronizzare i dati salvati su Xbox Live per i giochi abilitati.  Se il servizio viene arrestato, non sarà possibile caricare né scaricare i dati salvati su Xbox Live.
|   **Nome del servizio**    |   XblGameSave
|   **Installazione**    |   Solo con Esperienza desktop
|   **Tipo di avvio**   |   Manuale
|   **Consiglio**  |   Opportuno disabilitare
|   **Commenti**    |   Questo servizio consente di sincronizzare i dati salvati su Xbox Live per i giochi abilitati.  Se il servizio viene arrestato, non sarà possibile caricare né scaricare i dati salvati su Xbox Live.
|||         




