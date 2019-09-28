---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Pianificazione per l'interoperabilità con AD FS 1.x
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e9f72bd83c90a804749329521a72e3232589c735
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407968"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Pianificazione per l'interoperabilità con AD FS 1.x

Active Directory Federation Services \(AD FS @ no__t-1 i server federativi che eseguono Windows Server® 2012 possono interagire con un AD FS 1,0 \(installed con Windows Server 2003 R2 @ no__t-3 Servizio federativo e un AD FS 1,1 \(installed con Windows Server 2008 o Windows Server 2008 R2 @ no__t-5 Servizio federativo. Sono supportate le seguenti combinazioni di interoperabilità:  

-   Qualsiasi AD FS 1. *x* servizio federativo può inviare un'attestazione che può essere utilizzata da un Servizio federativo di ad FS in Windows Server 2012. Per ulteriori informazioni, vedere [Elenco di controllo: Configurazione di AD FS per l'utilizzo di attestazioni da AD FS 1. x @ no__t-0.  

-   Qualsiasi Servizio federativo di AD FS in Windows Server 2012 può inviare un AD FS 1. *x*@no__t: attestazione 1compatible che può essere utilizzata da un ad FS 1. *x* servizio federativo. Per ulteriori informazioni, vedere [Elenco di controllo: Configurazione AD FS per inviare attestazioni a una AD FS 1. x Servizio federativo @ no__t-0.  

-   Qualsiasi Servizio federativo di AD FS in Windows Server 2012 può inviare un AD FS 1. *x*@no__t: attestazione 1compatible che può essere utilizzata da uno o più server Web che eseguono il ad FS 1. *x* Claims @ no__t-3aware Web Agent. Per ulteriori informazioni, vedere [Elenco di controllo: Configurazione AD FS per inviare attestazioni a un agente Web in grado di riconoscere attestazioni AD FS 1. x @ no__t-0.  

> [!NOTE]  
> AD FS non supporta o interagisce con il AD FS 1. agente Web basato su *token Windows NT* .  

AD FS 1. *x*@no__t 1compatible è un'attestazione che può essere inviata da un ad FS servizio federativo in Windows Server 2012 e riconosciuta da un ad FS 1. *x* servizio federativo. Quindi, un AD FS 1. *x* servizio federativo possibile utilizzare le attestazioni inviate da un ad FS servizio federativo, è necessario inviare un identificatore nome \(ID @ no__t-2 tipo di attestazione.  

## <a name="understanding-the-name-id-claim-type"></a>Informazioni sul tipo di attestazione ID nome  
Il tipo di attestazione ID nome equivale al tipo di attestazione di identità usato da AD FS 1.*x* . Questa attestazione deve essere usata ogni volta che è necessaria l'interoperabilità con AD FS 1.*x*. Il tipo di attestazione ID nome Abilita un AD FS 1. *x* Servizio federativo o ad FS 1. *x* Claims @ no__t-2aware Web Agent to User Claims that ad FS in Windows Server 2012 sends, purché queste attestazioni vengano inviate in uno dei formati ID nome nella tabella seguente.  


|      Formato ID nome       |               URI corrispondente                |
|---------------------------|------------------------------------------------|
| Indirizzo di posta elettronica AD FS 1.*x* | http://schemas.xmlsoap.org/claims/EmailAddress |
|   UPN di posta elettronica AD FS 1.*x*   |     http://schemas.xmlsoap.org/claims/UPN      |
|        Nome comune        |  http://schemas.xmlsoap.org/claims/CommonName  |
|           Group           |    http://schemas.xmlsoap.org/claims/Group     |

Deve essere inviata solo una attestazione ID nome nel formato appropriato. Una volta soddisfatto questo criterio, è possibile inviare anche molte altre attestazioni, a condizione che siano che siano conformi alle restrizioni descritte nella tabella.  

> [!NOTE]  
> AD FS 1. *x* servizio federativo può interpretare solo i tipi di attestazione in ingresso che iniziano con il Uniform Resource Identifier \(URI @ no__t-2 di http://schemas.xmlsoap.org/claims/.  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
