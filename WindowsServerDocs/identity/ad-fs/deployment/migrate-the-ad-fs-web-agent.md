---
title: Eseguire la migrazione l'agente web AD FS
description: Fornisce informazioni sull'agente web ADFS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7cde62cb23c69a425522e40ed65ee2d40ef28268
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445630"
---
# <a name="migrate-the-ad-fs-web-agent"></a>Eseguire la migrazione l'agente web AD FS

Eseguire la migrazione di AD FS 1.1 agente basato su token Windows o Active Directory agente in grado di riconoscere attestazioni ADFS 1.1 installato con Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012, eseguire un aggiornamento sul posto del sistema operativo del computer che ospita l'agente in Windows Server 2012. Per altre informazioni, vedere [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Non è richiesta alcuna configurazione aggiuntiva.  
  
> [!IMPORTANT]
>  L'agente basato su token Windows AD FS 1.1 migrato funziona solo con un servizio federativo AD FS 1.1 installato con Windows Server 2008 R2 o Windows Server 2008. Per altre informazioni, vedere [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
> 
>  L'agente in grado di riconoscere attestazioni AD FS 1.1 migrato funzionerà con i componenti seguenti:  
> 
> - Servizio federativo AD FS 1.1 installato in Windows Server 2008 R2 o Windows Server 2008  
>   -   Servizio federativo AD FS 2.0 installato in Windows Server 2008 R2 o Windows Server 2008  
>   -   Servizio federativo AD FS installato con Windows Server 2012  
> 
>   Per altre informazioni, vedere [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione di AD Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del Proxy Server 2.0 Federation di AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di AD Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del Proxy Server 2.0 Federation di AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)