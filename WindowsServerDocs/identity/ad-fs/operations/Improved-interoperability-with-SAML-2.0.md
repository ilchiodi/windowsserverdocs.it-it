---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: "Interoperabilità migliorata con SAML 2.0"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4f55eaacec8ee0eb41e1980f1aa15c6256f8b979
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="improved-interoperability-with-saml-20"></a>Interoperabilità migliorata con SAML 2.0

>Si applica a: Windows Server 2016

  
ADFS in Windows Server 2016 contiene supporto del protocollo SAML aggiuntivo, incluso il supporto per l'importazione di relazioni di trust in base ai metadati che contiene più entità.  Ciò consente di configurare ADFS per partecipare confederations come Federation InCommon e altre implementazioni conformi al eGov 2.0 standard.   
  
La nuova funzionalità si basa su gruppi di relying party o provider di attestazioni. Ogni gruppo è un elemento EntitiesDescriptor (< md:EntitiesDescriptor >), come specificato nei eGov 2.0 profilo, contenente uno o più elementi EntityDescriptor.  I gruppi sono comuni regole di autorizzazione e tutte le altre proprietà possono essere modificate come oggetti singoli trust.  
  
Una volta i gruppi di trust vengono importati in ADFS, ADFS Aggiorna automaticamente le relazioni di trust come un gruppo in base al documento di metadati.  
  
L'abilitazione di questi scenari è semplice come l'uso di tale oggetti aggiungere e rimuovere AdfsClaimsProviderTrustsGroup e AdfsRelyingPartyTrustsGroup i nuovi cmdlet di PowerShell. Questa operazione può essere eseguita tramite un URL dei metadati o un file, come illustrato negli esempi seguenti.  
  
Inoltre, AD FS 2016 fornisce supporto per il parametro dell'ambito come descritto nella sezione 3.4.1.2 specifica di SAML dei componenti di base. Questo elemento consente relying parti per specificare uno o più provider di identità per l'autenticazione richiesta.  
  
## <a name="examples"></a>Esempi  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>Riferimenti  
  
Il profilo eGov 2.0 è reperibile [qui.](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
La specifica dei componenti di base SAML è reperibile [qui.](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   


