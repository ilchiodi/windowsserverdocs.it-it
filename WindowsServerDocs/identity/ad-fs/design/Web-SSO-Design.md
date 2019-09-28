---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Soluzione di accesso Single Sign-On Web
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d7f52cd36588a1e5de4536a760c38c147dd1e003
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407832"
---
# <a name="web-sso-design"></a>Soluzione di accesso Single Sign-On Web

Nella progettazione Web single @ no__t-0Sign @ no__t-1Sulla Barra \(SSO @ no__t-3 Active Directory Federation Services \(AD FS @ no__t-5, gli utenti devono eseguire l'autenticazione una sola volta per accedere a più applicazioni o servizi AD FS @ no__t-6secured. In questa soluzione tutti gli utenti sono esterni e non esiste alcun trust federativo perché non sono presenti organizzazioni partner. In genere, si distribuisce questo tipo di progettazione quando si desidera fornire singoli consumer o clienti l'accesso a una o più applicazioni o servizi di Active Directory protetta con ADFS su Internet, come illustrato nella figura seguente.  
  
![progettazione di Web sso](media/adfs2_WebSSODesign.gif)  
  
Con il progetto Web SSO, un'organizzazione che in genere ospita un'applicazione AD FS @ no__t-0secured o un servizio in una rete perimetrale può mantenere un archivio separato di account dei clienti nella rete perimetrale, rendendo più semplice isolare gli account dei clienti da account del dipendente.  
  
È possibile gestire gli account locali per i clienti nella rete perimetrale usando Active Directory Domain Services \(AD DS @ no__t-1, SQL Server o un archivio attributi personalizzato.  
  
Questa soluzione coincide con l'obiettivo di distribuzione in [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Per un elenco delle attività dettagliate che è possibile usare per pianificare e distribuire il progetto Web SSO, vedere [Checklist: Implementazione di un progetto Web SSO @ no__t-0.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
