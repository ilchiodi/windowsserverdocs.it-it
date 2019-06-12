---
title: Eseguire la migrazione di AD server federativo ADFS 2.0
description: Vengono fornite informazioni sulla migrazione di un server AD FS per Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 38e44ab2f803d8ec8940dbba7574a9f37389112a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444467"
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>Verificare di AD FS 2.0 migrazione a Windows Server 2012 R2

Dopo aver completato la migrazione di un server alla farm di Active Directory Federation Services (ADFS) di Windows Server 2012 R2, è possibile utilizzare la procedura seguente per verificare che i server federativi della farm siano operativi; vale a dire che qualsiasi client nella stessa rete possa raggiungere i server federtation.  
  
Per eseguire questa procedura è richiesta almeno l'appartenenza al gruppo **Users**, **Backup Operators**, **Power Users**, **Administrators** oppure a un gruppo equivalente.
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>Per verificare che un server federativo sia operativo  
  
1.  Aprire una finestra del browser e nella barra degli indirizzi, digitare il nome del server federativo e quindi aggiungere `federationmetadata/2007-06/federationmetadata.xml` per passare all'endpoint di metadati del servizio federativo. Ad esempio, `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` .  
  
Se nella finestra del browser è possibile vedere i metadati del server federativo senza errori o avvisi SSL, ciò indica che il server federativo è operativo.  
  
2. È inoltre possibile passare alla pagina di accesso di ADFS (nome del servizio federativo con l'aggiunta di `adfs/ls/idpinitiatedsignon.htm`, ad esempio `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`).  Verrà visualizzata la pagina di accesso di ADFS nella quale è possibile accedere con le credenziali di amministratore di dominio.  
  
> [!IMPORTANT]
>  Assicurarsi di configurare le impostazioni del browser in modo da considerare attendibile il ruolo del server federativo, aggiungendo il nome del servizio federativo (ad esempio `https://fs.contoso.com`) all'area Intranet locale del browser.  
  
## <a name="next-steps"></a>Passaggi successivi
 [Eseguire la migrazione di servizi ruolo di Active Directory Federation Services a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparazione alla migrazione del Server federativo ADFS](prepare-migrate-ad-fs-server-r2.md)  
 [Migrazione del Server federativo di AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Eseguire la migrazione del Proxy Server federativo di AD FS](migrate-fed-server-proxy-r2.md)   
