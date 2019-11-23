---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 'Elenco di controllo: implementazione di un progetto Web SSO'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8488e0c9195930374aacd959e72d0eff34142ca7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408462"
---
# <a name="checklist-implementing-a-web-sso-design"></a>Elenco di controllo: Implementazione di un progetto Web SSO

Questo elenco di controllo padre include collegamenti di riferimento incrociato\-ai concetti importanti sul Web single\-Sign\-in \(SSO\) design for Active Directory Federation Services \(ad FS\). Contiene inoltre collegamenti a elenchi di controllo subordinati che consentiranno di completare le attività necessarie per implementare il progetto.  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Se un collegamento di riferimento porta a un argomento concettuale o a un elenco di controllo subordinato, tornare in questo argomento dopo aver consultato l'argomento concettuale o aver completato le attività dell'elenco di controllo subordinato, in modo da procedere con le attività rimanenti.  
  
![Web SSO](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: implementazione di un progetto Web SSO**  
  
||Attività|Riferimento|  
|-|--------|-------------|  
|![Web sso](media/icon_checkboxo.gif)|Esaminare i concetti importanti sul progetto di Web SSO e determinare gli obiettivi di distribuzione di ADFS è possibile utilizzare per personalizzare il progetto per soddisfare le esigenze dell'organizzazione. **Nota:**|](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[progettazione SSO](https://technet.microsoft.com/library/dd807033.aspx) web SSO ![<br /><br />![SSO Web che](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifica gli obiettivi di distribuzione ad FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![Web sso](media/icon_checkboxo.gif)|Esaminare i componenti hardware, software, certificati, Domain Name System \(DNS\), archivio di attributi e sui requisiti client per la distribuzione di ADFS nell'organizzazione.|![Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Appendice A: revisione dei requisiti di ad FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![Web sso](media/icon_checkboxo.gif)|In base al piano di progettazione, installare uno o più server federativi nella rete aziendale o nella rete perimetrale. **Nota:** la progettazione di Web SSO richiede solo un server federativo funzioni correttamente. Un server federativo singolo svolge il ruolo di provider di attestazioni sia relying party ruolo.|![Web SSO](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)|  
|![Web sso](media/icon_checkboxo.gif)|\(facoltativo\) determinare se l'organizzazione necessita o meno di un proxy server federativo nella rete perimetrale.|![Web SSO](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![Web sso](media/icon_checkboxo.gif)|In base al piano di progetto di Web SSO e a come si intende usarlo, aggiungere l'archivio di attributi, i trust della relying party, le attestazioni e le regole di attestazione appropriate al Servizio federativo.|![Web SSO](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[elenco di controllo: configurazione dell'organizzazione partner account](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![Web sso](media/icon_checkboxo.gif)|Se si è un amministratore dell'organizzazione partner risorse, le attestazioni\-abilitare l'applicazione Web browser, l'applicazione del servizio Web o l'applicazione Microsoft® Office SharePoint® Server usando WIF e WIF SDK. **Nota:**|![Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)| 
