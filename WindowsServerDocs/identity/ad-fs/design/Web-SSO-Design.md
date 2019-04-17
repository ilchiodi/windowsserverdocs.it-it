---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Progettazione di Web SSO
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1b8344594c9fc477ed8424c716ec8d7f7fd91ef3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="web-sso-design"></a>Progettazione di Web SSO

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Progettazione di accesso in apartment \(SSO\) in Active Directory Federation Services \(AD FS\) del Web, gli utenti devono autenticarsi una sola volta per accedere a più applicazioni protette FS\ Active Directory o servizi. In questa struttura, tutti gli utenti sono esterni ed esiste alcun trust federativo perché non sono presenti organizzazioni partner. In genere, si distribuisce questa soluzione quando si desidera fornire singoli consumer o clienti l'accesso a uno o più applicazioni o servizi di Active Directory protetta con ADFS su Internet, come illustrato nella figura seguente.  
  
![progettazione di Web sso](media/adfs2_WebSSODesign.gif)  
  
Con il Web SSO, un'organizzazione che ospita un'applicazione protetta FS\ Active Directory o servizio in una rete perimetrale può mantenere un archivio separato di account dei clienti nella rete perimetrale, che rende più semplice isolare gli account cliente dagli account dei dipendenti.  
  
È possibile gestire gli account locali per i clienti nella rete perimetrale tramite servizi di dominio Active Directory \(AD DS\), SQL Server o un archivio attributi personalizzato.  
  
Questa soluzione coincide con l'obiettivo di distribuzione in [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Per un elenco di attività dettagliate che è possibile utilizzare per pianificare e distribuire il progetto Web SSO, vedere [elenco di controllo: implementazione di una progettazione di Web SSO](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
