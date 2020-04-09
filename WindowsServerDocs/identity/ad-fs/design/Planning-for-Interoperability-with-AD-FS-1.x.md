---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Pianificazione per l'interoperabilità con AD FS 1.x
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0bbf64a7bf110e3d73084dd047c84b2b83be8d9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858614"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Pianificazione per l'interoperabilità con AD FS 1.x

Active Directory Federation Services \(AD FS\) server federativi che eseguono Windows Server&reg; 2012 possono interagire con un ad FS \(1,0 installato con Windows Server 2003 R2\) servizio federativo e un ad FS 1,1 \(installato con Windows Server 2008 o Windows Server 2008 R2\) servizio federativo. Sono supportate le seguenti combinazioni di interoperabilità:  

-   Qualsiasi AD FS 1. *x* servizio federativo può inviare un'attestazione che può essere utilizzata da un Servizio federativo di ad FS in Windows Server 2012. Per ulteriori informazioni, vedere [elenco di controllo: configurazione di ad FS per l'utilizzo di attestazioni da ad FS 1. x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  

-   Qualsiasi Servizio federativo di AD FS in Windows Server 2012 può inviare un AD FS 1. *x*\-attestazione compatibile che può essere utilizzata da un ad FS 1. *x* servizio federativo. Per altre informazioni, vedere [Checklist: Configuring AD FS to Send Claims to an AD FS 1.x Federation Service](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  

-   Qualsiasi Servizio federativo di AD FS in Windows Server 2012 può inviare un AD FS 1. *x*\-attestazione compatibile che può essere utilizzata da uno o più server Web che eseguono il ad FS 1. attestazioni *x*\-agente Web in grado di riconoscere. Per altre informazioni, vedere [Checklist: Configuring AD FS to Send Claims to an AD FS 1.x Claims-Aware Web Agent](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  

> [!NOTE]  
> AD FS non supporta o interagisce con il AD FS 1. agente Web basato su *token Windows NT* .  

AD FS 1. *x*\-attestazione compatibile è un'attestazione che può essere inviata da un ad FS servizio federativo in Windows Server 2012 e riconosciuta da un ad FS 1. *x* servizio federativo. Quindi, un AD FS 1. *x* servizio federativo possibile utilizzare le attestazioni inviate da un ad FS servizio federativo, è necessario inviare un identificatore nome \(ID\) tipo di attestazione.  

## <a name="understanding-the-name-id-claim-type"></a>Informazioni sul tipo di attestazione ID nome  
Il tipo di attestazione ID nome equivale al tipo di attestazione di identità usato da AD FS 1.*x* . Questa attestazione deve essere usata ogni volta che è necessaria l'interoperabilità con AD FS 1.*x*. Il tipo di attestazione ID nome Abilita un AD FS 1. *x* Servizio federativo o ad FS 1. *x* attestazioni\-agente Web in grado di utilizzare le attestazioni inviate da ad FS in Windows Server 2012, purché tali attestazioni vengano inviate in uno dei formati ID nome nella tabella seguente.  


|      Formato ID nome       |               URI corrispondente                |
|---------------------------|------------------------------------------------|
| Indirizzo di posta elettronica AD FS 1.*x* | http://schemas.xmlsoap.org/claims/EmailAddress |
|   UPN di posta elettronica AD FS 1.*x*   |     http://schemas.xmlsoap.org/claims/UPN      |
|        Nome comune        |  http://schemas.xmlsoap.org/claims/CommonName  |
|           Gruppo           |    http://schemas.xmlsoap.org/claims/Group     |

Deve essere inviata solo una attestazione ID nome nel formato appropriato. Una volta soddisfatto questo criterio, è possibile inviare anche molte altre attestazioni, a condizione che siano che siano conformi alle restrizioni descritte nella tabella.  

> [!NOTE]  
> AD FS 1. *x* servizio federativo può interpretare solo i tipi di attestazione in ingresso che iniziano con il Uniform Resource Identifier \(URI\) di http://schemas.xmlsoap.org/claims/.  

## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
