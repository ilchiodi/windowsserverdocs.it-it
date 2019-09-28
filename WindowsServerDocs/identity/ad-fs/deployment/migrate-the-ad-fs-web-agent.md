---
title: Eseguire la migrazione dell'agente Web AD FS
description: Fornisce informazioni sull'agente Web AD FS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ddba9668b54e325dae6dfc0cf67d50d3ae5d90be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408191"
---
# <a name="migrate-the-ad-fs-web-agent"></a>Eseguire la migrazione dell'agente Web AD FS

Per eseguire la migrazione dell'agente basato su token Windows AD FS 1,1 o dell'agente in grado di riconoscere attestazioni di AD FS 1,1 installato con Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012, eseguire un aggiornamento sul posto del sistema operativo del computer che ospita uno degli agenti a Windows Server 2012. Per altre informazioni, vedere [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Non è richiesta alcuna configurazione aggiuntiva.  
  
> [!IMPORTANT]
>  L'agente basato su token Windows AD FS 1.1 migrato funziona solo con un servizio federativo AD FS 1.1 installato con Windows Server 2008 R2 o Windows Server 2008. Per altre informazioni, vedere [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
> 
>  L'agente in grado di riconoscere attestazioni AD FS 1.1 migrato funzionerà con i componenti seguenti:  
> 
> - Servizio federativo AD FS 1.1 installato in Windows Server 2008 R2 o Windows Server 2008  
>   -   Servizio federativo AD FS 2.0 installato in Windows Server 2008 R2 o Windows Server 2008  
>   -   AD FS servizio federativo installato con Windows Server 2012  
> 
>   Per altre informazioni, vedere [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione del server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del proxy server federativo di AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione del server federativo AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del proxy server federativo AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)