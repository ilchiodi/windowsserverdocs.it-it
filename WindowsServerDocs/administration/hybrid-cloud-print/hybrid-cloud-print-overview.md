---
title: Cenni preliminari sulla stampa di Cloud ibrido di Windows Server
description: Per la stampa Cloud ibrido consente ai professionisti IT di supportare i requisiti di stampa per BYOD o dominio dispositivi aggiunti all'identità.
ms.prod: w10
ms.mktglfcycl: manage
ms.sitesec: library
ms.pagetype: store
ms.technology: server-general
author: TrudyHa
ms.author: TrudyHa
ms.date: 10/16/2017
ms.openlocfilehash: faa9fde857a9a4ee3f7c03f682b3dbced0340417
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878832"
---
# <a name="windows-server-hybrid-cloud-print-overview"></a>Cenni preliminari sulla stampa di Cloud ibrido di Windows Server

**Si applica a**
-   Windows Server 2016

## <a name="what-is-hybrid-cloud-print"></a>Che cos'è stampa Cloud ibrido?
**Stampa di Cloud ibrido** è disponibile tramite una nuova funzionalità per Windows Server 2016 **funzionalità su richiesta**. Consente ai professionisti IT di supportare i requisiti di stampa per chi desidera portano i propri dispositivi o usano i dispositivi aggiunti ad Azure Active Directory. Sono inclusi i dispositivi mobili, ad esempio Windows phone, computer portatili o Tablet che eseguono Windows 10 o Windows Mobile. Fornisce il supporto della stampa ovunque gli utenti hanno accesso a Internet.

Agli amministratori IT **Hybrid Cloud Print** fornisce l'accesso sicuro alle stampanti locali usando l'autenticazione a più fattori di Azure per convalidare l'accesso utente. Funzionalità (SSO) Single sign-on semplifica l'esperienza dell'utente. **Stampa di Cloud ibrido** si basa su Windows **Server di stampa** ruolo, offrendo un'esperienza simile alla gestione delle stampanti e sicurezza dall'accesso utente ai professionisti IT.

**Per la stampa Cloud ibrido** consente agli utenti dell'organizzazione per stampare dai dispositivi usati per completare il proprio lavoro, anche quando si è lontani dalla loro scrivania o all'area di lavoro.

**Stampa di Cloud ibrido** è supportata in Windows 10 Creators Update e Windows 10 S.
 
## <a name="feature-summary"></a>Riepilogo delle funzionalità
**Stampa di Cloud ibrido** è costituito da due componenti lato server principali: **Individuazione** servizio, e **Windows Print** servizio.
- **Individuazione** endpoint del servizio in esecuzione in un servizio IIS di supporto standard di settore Mopria Alliance per l'individuazione di stampanti nel cloud.
- **Stampa di Windows** supportano standard protocollo IPP (Internet Printing) per verificare che il sistema operativo client più ampia endpoint del servizio in esecuzione in un servizio IIS che supportano del settore.

## <a name="deployment"></a>Distribuzione
**Stampa di Cloud ibrido** supporta un paio di opzioni di distribuzione diversi a seconda di dove l'organizzazione richiede l'autenticazione dell'utente. Ecco come potrebbe risultare una distribuzione:

![Diagramma che mostra una rappresentazione grafica della soluzione ibrida Cloud Print](../media/hybrid-cloud-print/wshcp-deployment-options.png)

*Diagramma della soluzione ibrida stampa Cloud*

Il diagramma mostra:
- **Stampa di Cloud ibrido** usando Azure Active Directory come provider di identità utente. 
- **Windows Print** servizio e **individuazione** gli endpoint di servizio sono registrati con Azure Active Directory per abilitare il dispositivo client recuperare il token di autenticazione necessari utente da usare per questi servizi. 
- Manutenzione di un MDM, ad esempio **Microsoft Intune**, effettuare il provisioning del dispositivo client con i criteri necessari per la connessione di Azure Active Directory per **Windows Print** servizio e **individuazione**service.

Questa tabella contiene altre informazioni sugli elementi nel diagramma.  

| Elemento | Descrizione |
| ------- | ----------- |
| Azure Active Directory  | Fornisce controlli e funzionalità di identità e autorizzazione utente |
| Active Directory        | Fornisce controlli e funzionalità di identità e autorizzazione utente |
| Azure AD Connect  | Consente di sincronizzare le credenziali dell'utente tra Azure AD e Active Directory locale. |
| Servizio MDM (Intune) | Fornisce funzionalità di provisioning dei criteri dispositivo per assicurarsi che il dispositivo client (dispositivi BYOD) sia conforme ai criteri aziendali. |
| Azure AD Proxy | Fornisce una connessione di lunga durata che viene stabilita da dietro il firewall per Azure per consentire il flusso del traffico specifico dell'applicazione configurata da Internet alla rete aziendale. |
| App Web di Azure | Il nucleo della soluzione ibrida Cloud Print. Fornisce gli endpoint web richiesto per l'invio di contenuto di stampa per i dispositivi aggiunti non di dominio e individuazione delle stampanti. |
| Dispositivo BYOD / Server Spooler di stampa di Windows / stampante | Si tratta come-è. Nessuna modifica nella funzionalità nella distribuzione. |

Esistono due modi per installare **Hybrid Cloud Print**:
- **Funzionalità su richiesta** -visualizzare [configurare funzionalità su richiesta in Windows Server](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) per altre informazioni sull'aggiunta e rimozione di file di ruoli e funzionalità. 
- **Impostazioni di Windows Server 2016** -gli amministratori possono passare a **delle impostazioni** -> **app** -> **Gestisci funzionalità facoltative**  ->  **Aggiungere una funzionalità** e cercare le funzionalità nei pacchetti richiesta 
- Comandi di PowerShell - finestra di amministratore In di PowerShell, Esegui questi comandi:

```PowerShell
    Install-Module -Name PublishCloudPrinter
    Import-Module PublishCloudPrinter
    ```
