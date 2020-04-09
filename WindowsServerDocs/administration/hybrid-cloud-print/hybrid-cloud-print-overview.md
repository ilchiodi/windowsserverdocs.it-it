---
title: Panoramica di stampa cloud ibrido Windows Server
description: Hybrid Cloud Print consente ai professionisti IT di supportare i requisiti di stampa per i dispositivi BYOD o aggiunti a un dominio.
ms.prod: windows-server
ms.technology: server-general
author: trudyha
ms.author: trudyha
ms.date: 10/16/2017
ms.openlocfilehash: f448e8709f9e73165ba1a477c59567fcff4a2008
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852004"
---
# <a name="windows-server-hybrid-cloud-print-overview"></a>Panoramica di stampa cloud ibrido Windows Server

**Si applica a**
-   Windows Server 2016

## <a name="what-is-hybrid-cloud-print"></a>Che cos'è la stampa cloud ibrida?
**Hybrid Cloud Print** è una nuova funzionalità di Windows Server 2016 disponibile tramite **funzionalità su richiesta**. Consente ai professionisti IT di supportare i requisiti di stampa per gli utenti che portano i propri dispositivi o usano dispositivi aggiunti al Azure Active Directory. Sono inclusi i dispositivi mobili, ad esempio Windows Phone, laptop o tablet che eseguono Windows 10 o Windows Mobile. Fornisce supporto per la stampa da qualsiasi luogo in cui le persone hanno accesso a Internet.

Per gli amministratori IT, la funzionalità di **stampa cloud ibrida** fornisce l'accesso utente sicuro alle stampanti locali usando l'autenticazione a più fattori di Azure per convalidare l'accesso utente. La funzionalità Single Sign-on (SSO) semplifica l'esperienza utente. Il **cloud ibrido** è basato sul ruolo del **server di stampa** di Windows, offrendo ai professionisti IT un'esperienza simile alla gestione delle stampanti e della sicurezza dell'accesso utente.

Il **cloud ibrido** consente agli utenti dell'organizzazione di stampare dai dispositivi usati per completare il lavoro, anche quando si trovano fuori dalla propria scrivania o dall'area di lavoro.

La **stampa cloud ibrida** è supportata in Windows 10 Creators Update e Windows 10 S.
 
## <a name="feature-summary"></a>Riepilogo delle funzionalità
Il servizio di **stampa cloud ibrido** è costituito da due componenti principali sul lato server, ovvero il servizio di **individuazione** e il servizio di **stampa Windows** .
- Endpoint del servizio di **individuazione** in esecuzione su un servizio IIS che supporta Mopria Alliance Industry Standard per l'individuazione delle stampanti nel cloud.
- Endpoint del servizio di **stampa Windows** in esecuzione in un servizio IIS che supporta il protocollo IPP (Internet Printing Protocol) standard del settore per garantire il supporto del sistema operativo client più ampio.

## <a name="deployment"></a>Distribuzione
**Hybrid Cloud Print** supporta un paio di opzioni di distribuzione diverse a seconda della posizione in cui l'organizzazione richiede l'autenticazione utente. Di seguito è illustrata una distribuzione simile alla seguente:

![Diagramma che mostra una rappresentazione grafica della soluzione di stampa cloud ibrida](../media/hybrid-cloud-print/wshcp-deployment-options.png)

*Diagramma della soluzione di stampa cloud ibrida*

Il diagramma mostra:
- **Stampa cloud ibrida** con Azure Active Directory come provider di identità utente. 
- Gli endpoint dei servizi di stampa e di **individuazione** di **Windows** sono registrati con Azure Active Directory per consentire al dispositivo client di recuperare il token di autenticazione utente necessario da usare per questi servizi. 
- Un servizio MDM, ad esempio **Microsoft Intune**, effettua il provisioning del dispositivo client con i criteri necessari per connettersi Azure Active Directory a servizio di **stampa Windows** e al servizio di **individuazione** .

Questa tabella contiene altre informazioni sugli elementi del diagramma.  

| Elemento | Descrizione |
| ------- | ----------- |
| Azure Active Directory  | Fornisce e controlla la funzionalità di autorizzazione e identità utente |
| Active Directory        | Fornisce e controlla la funzionalità di autorizzazione e identità utente |
| Azure AD Connect  | Sincronizza le credenziali utente tra Azure AD e AD locale. |
| Servizio MDM (Intune) | Fornisce funzionalità di provisioning dei criteri dei dispositivi per garantire che il dispositivo BYOD sia conforme ai criteri aziendali. |
| Proxy Azure AD | Fornisce una connessione di lunga durata stabilita da un firewall ad Azure per consentire il flusso di traffico dell'applicazione configurato specifico da Internet alla rete aziendale. |
| App Web di Azure | Il nucleo della soluzione di stampa cloud ibrida. Fornisce gli endpoint Web richiesti per individuare le stampanti e inviare il contenuto di stampa per i dispositivi non aggiunti a un dominio. |
| Dispositivo BYOD/stampante/spooler del server di stampa Windows | Sono così come sono. Non sono state apportate modifiche alla funzionalità nella distribuzione. |

Esistono due modi per installare la **stampa del cloud ibrido**:
- \* * Funzionalità su richiesta. per ulteriori informazioni sull'aggiunta e la rimozione di file di ruoli e funzionalità, vedere [configurare funzionalità su richiesta in Windows Server](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) . 
- \* * Impostazioni di Windows Server 2016, che gli amministratori possono passare a **impostazioni** -> **app** -> **gestire funzionalità opzionali** -> **aggiungere una funzionalità** e cercare il pacchetto funzionalità su richiesta 
- Comandi di PowerShell: in una finestra di amministratore di PowerShell, eseguire i comandi seguenti:

```PowerShell
    Install-Module -Name PublishCloudPrinter
    Import-Module PublishCloudPrinter
    ```
