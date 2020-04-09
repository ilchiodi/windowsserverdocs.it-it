---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: Interoperabilità migliorata con SAML 2.0
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3abc3f09e5ae572800e5580d14a76ada6d62e320
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816364"
---
# <a name="improved-interoperability-with-saml-20"></a>Interoperabilità migliorata con SAML 2.0



  
AD FS in Windows Server 2016 contiene supporto del protocollo SAML aggiuntivo, incluso il supporto per l'importazione di trust basati su metadati che contengono più entità.  Ciò consente di configurare ADFS per partecipare confederations come Federation InCommon e altre implementazioni conformi al eGov 2.0 standard.   
  
La nuova funzionalità è basata su gruppi di relying party o trust del provider di attestazioni. Ogni gruppo è un elemento EntitiesDescriptor (< MD: EntitiesDescriptor >) come specificato nel profilo eGov 2,0, contenente uno o più elementi EntityDescriptor.  I gruppi dispongono di regole di autorizzazione comuni e tutte le altre proprietà possono essere modificate come singoli oggetti trust.  
  
Una volta importati i gruppi di attendibilità in AD FS, AD FS aggiorna automaticamente i trust come un gruppo basato sul documento di metadati.  
  
L'abilitazione di questi scenari è semplice come l'uso dei nuovi cmdlet di PowerShell che aggiungono e rimuovono gli oggetti AdfsClaimsProviderTrustsGroup e AdfsRelyingPartyTrustsGroup. Questa operazione può essere eseguita usando un URL di metadati o un file, come illustrato negli esempi seguenti.  
  
Inoltre, AD FS 2016 dispone del supporto per il parametro di ambito come descritto nella specifica SAML core, sezione 3.4.1.2. Questo elemento consente alle relying party di specificare uno o più provider di identità per una richiesta di autenticazione.  
  
## <a name="examples"></a>Esempi  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>Riferimenti  
  
Il profilo eGov 2,0 è disponibile [qui.](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
La specifica SAML core è disponibile [qui.](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   


