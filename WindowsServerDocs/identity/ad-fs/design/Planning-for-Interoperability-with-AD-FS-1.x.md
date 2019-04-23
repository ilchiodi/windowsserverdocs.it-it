---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Pianificazione per l'interoperabilità con AD FS 1.x
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f287261ce6cb56e40385ef4de922045153819a23
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877562"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Pianificazione per l'interoperabilità con AD FS 1.x

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Federation Services \(ADFS\) che esegue Windows Server® 2012 server federativi possono interagire con entrambi un'istanza di ADFS 1.0 \(installata con Windows Server 2003 R2\) servizio federativo e un'istanza di ADFS 1.1 \(installato con Windows Server 2008 o Windows Server 2008 R2\) servizio federativo. Sono supportate le seguenti combinazioni di interoperabilità:  
  
-   Qualsiasi istanza di ADFS 1. *x* servizio federativo può inviare un'attestazione che può essere utilizzata da un servizio federativo di AD FS in Windows Server 2012. Per ulteriori informazioni, vedere [Elenco di controllo: Configurazione di ADFS per utilizzare attestazioni ADFS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
-   Qualsiasi servizio federativo ADFS in Windows Server 2012 può inviare un'istanza di ADFS 1. *x*\-attestazione compatibili che può essere utilizzati da un'istanza di ADFS 1. *x* servizio federativo. Per ulteriori informazioni, vedere [Elenco di controllo: Configurazione di AD FS per l'invio di attestazioni a un servizio federativo di AD FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Qualsiasi servizio federativo ADFS in Windows Server 2012 può inviare un'istanza di ADFS 1. *x*\-attestazione compatibili che può essere utilizzati da uno o più server Web che esegue AD FS 1. *x* attestazioni\-agente Web compatibile con. Per ulteriori informazioni, vedere [Elenco di controllo: Configurazione di AD FS per l'invio di attestazioni a un agente di AD FS 1.x Claims-Aware Web](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
> [!NOTE]  
> ADFS non supporta né offre interoperabilità con AD FS 1. *x* agente Web basato su token Windows NT.  
  
Un'istanza di ADFS 1. *x*\-attestazione compatibile è un'attestazione che può essere inviata da un servizio federativo di AD FS in Windows Server 2012 e compresa da un'istanza di ADFS 1. *x* servizio federativo. In modo che un'istanza di ADFS 1. *x* servizio federativo può utilizzare le attestazioni inviate da un servizio federativo AD FS, un identificatore di nome \(ID\) attestazione di tipo deve essere inviato.  
  
## <a name="understanding-the-nameid-claim-type"></a>Informazioni sul tipo di attestazione ID nome  
Il tipo di attestazione ID nome equivale al tipo di attestazione di identità usato da AD FS 1.*x*. Questa attestazione deve essere usata ogni volta che è necessaria l'interoperabilità con AD FS 1.*x*. L'attestazione ID nome tipo Abilita sia un'istanza di ADFS 1. *x* servizio federativo o AD FS 1. *x* attestazioni\-agente Web compatibile con per utilizzare attestazioni inviate da ADFS in Windows Server 2012, purché queste attestazioni siano inviate in uno dei formati di ID nome nella tabella seguente.  
  
|Formato ID nome|URI corrispondente|  
|------------------|---------------------|  
|Indirizzo di posta elettronica AD FS 1.*x*|http://schemas.xmlsoap.org/claims/EmailAddress|  
|UPN di posta elettronica AD FS 1.*x*|http://schemas.xmlsoap.org/claims/UPN|  
|Nome comune|http://schemas.xmlsoap.org/claims/CommonName|  
|Raggruppa|http://schemas.xmlsoap.org/claims/Group|  
  
Deve essere inviata solo una attestazione ID nome nel formato appropriato. Una volta soddisfatto questo criterio, è possibile inviare anche molte altre attestazioni, a condizione che siano che siano conformi alle restrizioni descritte nella tabella.  
  
> [!NOTE]  
> Un'istanza di ADFS 1. *x* servizio federativo può interpretare i tipi di attestazione solo in ingresso che iniziano con l'Uniform Resource Identifier \(URI\) di http://schemas.xmlsoap.org/claims/.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
