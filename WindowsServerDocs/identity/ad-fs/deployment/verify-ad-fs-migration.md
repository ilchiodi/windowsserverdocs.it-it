---
title: Eseguire la migrazione di Active Directory server federativo ADFS 2.0
description: Fornisce informazioni sulla migrazione di un server ADFS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a3e8e444f715fe2ae0f0ccd858d90e8664be00c
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2017
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>Verificare di AD FS 2.0 migrazione a Windows Server 2012 R2

Dopo aver completato la migrazione di un server della farm di Active Directory Federation Services (ADFS) a Windows Server 2012 R2, è possibile utilizzare la procedura seguente per verificare che i server federativi nella farm siano operativi; vale a dire che qualsiasi client nella stessa rete può raggiungere i server federtation.  
  
Appartenenza al gruppo **utenti**, **Backup Operators**, **Power Users**, **amministratori** o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>Per verificare che un server federativo sia operativo  
  
1.  Aprire una finestra del browser e nella barra degli indirizzi, digitare il nome del server federativo e quindi aggiungere `federationmetadata/2007-06/federationmetadata.xml` per passare all'endpoint dei metadati del servizio federativo. Ad esempio, `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` .  
  
Se nella finestra del browser è possibile visualizzare i metadati del server federativo senza SSL errori o avvisi, il server federativo è operativo.  
  
2.  È inoltre possibile passare alla pagina di accesso di ADFS (nome del servizio federativo con l'aggiunta `adfs/ls/idpinitiatedsignon.htm`, ad esempio `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`).  Verrà visualizzata la AD FS sign-in di pagina in cui è possibile accedere con le credenziali di amministratore di dominio.  
  
> [!IMPORTANT]
>  Assicurarsi di configurare le impostazioni del browser per considerare attendibile il ruolo server federativo aggiungendo il nome del servizio federativo (ad esempio, `https://fs.contoso.com`) all'area intranet locale del browser.  
  
## <a name="next-steps"></a>Passaggi successivi
 [Eseguire la migrazione di ruolo di Active Directory Federation Services a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparazione alla migrazione del Server federativo ADFS](prepare-migrate-ad-fs-server-r2.md)  
 [La migrazione del Server federativo ADFS](migrate-ad-fs-fed-server-r2.md)   
 [La migrazione di Proxy Server federativo AD FS](migrate-fed-server-proxy-r2.md)   
