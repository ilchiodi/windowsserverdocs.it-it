---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Soluzione di accesso Single Sign-On Web
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 47c946ac617cc64c224c1bc3153fcaf55c2d069c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858514"
---
# <a name="web-sso-design"></a>Soluzione di accesso Single Sign-On Web

Nel\-Web Single Sign\-on \(SSO\) design in Active Directory Federation Services \(ad FS\), gli utenti devono eseguire l'autenticazione una sola volta per accedere a più AD FS\-applicazioni o servizi protetti. In questa soluzione tutti gli utenti sono esterni e non esiste alcun trust federativo perché non sono presenti organizzazioni partner. In genere, si distribuisce questo tipo di progettazione quando si desidera fornire singoli consumer o clienti l'accesso a una o più applicazioni o servizi di Active Directory protetta con ADFS su Internet, come illustrato nella figura seguente.  
  
![progettazione di Web sso](media/adfs2_WebSSODesign.gif)  
  
Con il progetto Web SSO, un'organizzazione che in genere ospita un AD FS\-applicazione o servizio protetto in una rete perimetrale può mantenere un archivio separato di account dei clienti nella rete perimetrale, rendendo più semplice isolare gli account dei clienti dagli account dei dipendenti.  
  
È possibile gestire gli account locali per i clienti nella rete perimetrale utilizzando Active Directory Domain Services \(AD DS\), SQL Server o un archivio attributi personalizzato.  
  
Questa soluzione coincide con l'obiettivo di distribuzione in [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Per un elenco di attività dettagliate che è possibile utilizzare per pianificare e distribuire il progetto Web SSO, vedere [elenco di controllo: implementazione di un progetto Web SSO](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
