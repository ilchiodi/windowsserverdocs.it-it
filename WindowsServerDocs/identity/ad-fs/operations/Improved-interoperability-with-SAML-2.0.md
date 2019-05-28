---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: Interoperabilità migliorata con SAML 2.0
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4148614ba35ce29f567edb08b94e115d3f9152e9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189098"
---
# <a name="improved-interoperability-with-saml-20"></a>Interoperabilità migliorata con SAML 2.0



  
ADFS in Windows Server 2016 contiene supporto del protocollo SAML aggiuntivo, incluso il supporto per l'importazione di relazioni di trust in base ai metadati che contiene più entità.  Ciò consente di configurare ADFS per partecipare confederations come Federation InCommon e altre implementazioni conformi al eGov 2.0 standard.   
  
La nuova funzionalità si basa su gruppi di relying party o trust di provider di attestazioni. Ogni gruppo è un elemento di EntitiesDescriptor (< md:EntitiesDescriptor >) come specificato nei eGov 2.0 profilo, che contiene uno o più elementi EntityDescriptor.  I gruppi hanno regole di autorizzazione più comuni e tutte le altre proprietà può essere modificato, ad esempio gli oggetti attendibilità singoli.  
  
Dopo aver importati i gruppi di trust in AD FS, ADFS Aggiorna automaticamente le relazioni di trust come un gruppo basato sul documento di metadati.  
  
L'abilitazione di questi scenari è semplice come l'utilizzo di nuovi cmdlet di PowerShell che aggiungere e rimuovere AdfsClaimsProviderTrustsGroup e AdfsRelyingPartyTrustsGroup oggetti. Questa operazione può essere eseguita usando un URL di metadati o un file, come illustrato negli esempi seguenti.  
  
Inoltre, AD FS 2016 offre supporto per il parametro di ambito, come descritto nella specifica SAML, sezione 3.4.1.2. Questo elemento consente relying parti per specificare uno o più provider di identità per l'autenticazione richiesta.  
  
## <a name="examples"></a>Esempi  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>Riferimenti  
  
Il profilo eGov 2.0 sono reperibili [qui.](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
La specifica SAML Core è reperibile [qui.](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   


