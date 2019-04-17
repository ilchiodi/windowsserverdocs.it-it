---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: "Pianificazione per l'interoperabilità con AD FS 1. x"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f287261ce6cb56e40385ef4de922045153819a23
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Pianificazione per l'interoperabilità con AD FS 1. x

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Federation Services \(AD FS\) federativi che esegue Windows Server® 2012 possono interagire con entrambi un'istanza di ADFS 1.0 \ (installato con Windows Server 2003 R2\) del servizio federativo e un'istanza di ADFS 1.1 \ (installato con Windows Server 2008 o Windows Server 2008 R2\) del servizio federativo. Una delle seguenti combinazioni di interoperabilità sono supportata:  
  
-   Qualsiasi AD FS 1. *x* servizio federativo può inviare un'attestazione che può essere utilizzata da un servizio federativo ADFS in Windows Server 2012. Per ulteriori informazioni, vedere [elenco di controllo: configurazione di ADFS per utilizzare attestazioni ADFS 1. x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
-   Qualsiasi servizio federativo AD FS in Windows Server 2012 può inviare un'istanza di ADFS 1. *x*\-compatible attestazione che può essere utilizzata da un'istanza di ADFS 1. *x* servizio federativo. Per ulteriori informazioni, vedere [Checklist: Configuring AD FS to Send Claims to an AD FS 1. x servizio federativo](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Qualsiasi servizio federativo AD FS in Windows Server 2012 può inviare un'istanza di ADFS 1. *x*\-compatible attestazione che può essere utilizzata da uno o più server Web con AD FS 1. *x* agente Web compatibile con claims\. Per ulteriori informazioni, vedere [Checklist: Configuring AD FS to Send Claims to an AD FS 1. x Claims-Aware Web agente](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
> [!NOTE]  
> ADFS non supporta o interagire con AD FS 1. *x* agente Web basato su token Windows NT.  
  
Un'istanza di ADFS 1. *x*\-compatible attestazione è un'attestazione che può essere inviata da un servizio federativo ADFS in Windows Server 2012 e compresa da un'istanza di ADFS 1. *x* servizio federativo. In modo che un'istanza di ADFS 1. *x* servizio federativo può utilizzare le attestazioni inviate da un servizio federativo AD FS, è necessario inviare un tipo di attestazione ID nome \(ID\).  
  
## <a name="understanding-the-name-id-claim-type"></a>Informazioni sul tipo di attestazione ID nome  
Il tipo di attestazione ID nome è l'equivalente di attestazioni di identità digitare da AD FS 1. *x* uses. Questa attestazione deve essere usata ogni volta che necessaria l'interoperabilità con AD FS 1. *x*. L'attestazione ID nome digitare consente di un'istanza di ADFS 1. *x* servizio federativo o AD FS 1. *x* agente Web compatibile con claims\ per utilizzare attestazioni inviate da AD FS in Windows Server 2012, a condizione che queste attestazioni siano inviate in uno dei formati di ID nome indicati nella tabella seguente.  
  
|Formato ID nome|URI corrispondente|  
|------------------|---------------------|  
|AD FS 1. *x* indirizzo di posta elettronica|http://schemas.xmlsoap.org/claims/EmailAddress|  
|AD FS 1. *x* UPN di posta elettronica|http://schemas.xmlsoap.org/claims/UPN|  
|Nome comune|http://schemas.xmlsoap.org/claims/CommonName|  
|Gruppo|http://schemas.xmlsoap.org/claims/Group|  
  
Deve essere inviata solo una attestazione ID nome nel formato appropriato. Quando questo criterio viene soddisfatta, molte altre attestazioni possono essere inviati, presupponendo che siano conformi alle restrizioni descritte nella tabella.  
  
> [!NOTE]  
> Un'istanza di ADFS 1. *x* servizio federativo può interpretare i tipi di attestazione solo in ingresso che iniziano con la \(URI\) Uniform Resource Identifier di http://schemas.xmlsoap.org/claims/.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
