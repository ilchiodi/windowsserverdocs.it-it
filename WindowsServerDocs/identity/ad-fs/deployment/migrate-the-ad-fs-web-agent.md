---
title: Eseguire la migrazione l'agente web ADFS
description: Fornisce informazioni sull'agente web ADFS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 945a5f4cf0e6c491479b095671ff5e77416c6fa3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-the-ad-fs-web-agent"></a>Eseguire la migrazione l'agente web ADFS

Per eseguire la migrazione AD FS 1.1 agente basato su token Windows o l'annuncio agente in grado di riconoscere attestazioni ADFS 1.1 installato con Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012, eseguire un aggiornamento sul posto del sistema operativo del computer che ospita l'agente di Windows Server 2012. Per ulteriori informazioni, vedere [l'installazione di Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). È necessaria alcuna configurazione aggiuntiva.  
  
> [!IMPORTANT]
>  L'annuncio migrato funzioni dell'agente basato su token Windows ADFS 1.1 solo con un servizio federativo 1.1 ADFS installato con Windows Server 2008 R2 o Windows Server 2008. Per ulteriori informazioni, vedere [interagisce con AD FS 1. x](Interoperating-with-AD-FS-1.x.md).  
>   
>  AD FS 1.1 grado di riconoscere attestazioni migrato web funzioni dell'agente con le operazioni seguenti:  
>   
>  -   Servizio federativo AD FS 1.1 installato con Windows Server 2008 R2 o Windows Server 2008  
> -   Servizio federativo AD FS 2.0 installato in Windows Server 2008 R2 o Windows Server 2008  
> -   Servizio federativo AD FS installato con Windows Server 2012  
>   
>  Per ulteriori informazioni, vedere [interagisce con AD FS 1. x](Interoperating-with-AD-FS-1.x.md).  
  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione di Active Directory Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di Active Directory Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione di AD FS 2.0 Server federativo](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Active Directory agenti Web ADFS 1.1](migrate-the-ad-fs-web-agent.md)