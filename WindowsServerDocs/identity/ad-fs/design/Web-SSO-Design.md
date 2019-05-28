---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Soluzione di accesso Single Sign-On Web
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 29d50a4d1855e609b6ac9ee627256201074a5033
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190719"
---
# <a name="web-sso-design"></a>Soluzione di accesso Single Sign-On Web

In Web Single\-Sign\-sul \(SSO\) progettazione in Active Directory Federation Services \(ADFS\), gli utenti devono autenticarsi una sola volta per accedere a più ADFS\- applicazioni protette o servizi. In questa soluzione tutti gli utenti sono esterni e non esiste alcun trust federativo perché non sono presenti organizzazioni partner. In genere, si distribuisce questo tipo di progettazione quando si desidera fornire singoli consumer o clienti l'accesso a una o più applicazioni o servizi di Active Directory protetta con ADFS su Internet, come illustrato nella figura seguente.  
  
![progettazione di Web sso](media/adfs2_WebSSODesign.gif)  
  
Con la progettazione di Web SSO Federativo, un'organizzazione che ospita un'istanza di AD FS\-protetti dell'applicazione o servizio in una rete perimetrale può mantenere un archivio separato di account dei clienti nella rete perimetrale, rendendo più semplice isolare cliente account dagli account dei dipendenti.  
  
È possibile gestire gli account locali per i clienti nella rete perimetrale usando servizi di dominio Active Directory \(Active Directory Domain Services\), SQL Server o un archivio attributi personalizzato.  
  
Questa soluzione coincide con l'obiettivo di distribuzione in [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Per un elenco di attività dettagliate che è possibile usare per pianificare e distribuire il progetto Web SSO, vedere [elenco di controllo: Implementazione di un progetto Web SSO](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
