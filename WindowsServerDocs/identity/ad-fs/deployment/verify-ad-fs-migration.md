---
title: Eseguire la migrazione del server federativo AD FS 2,0
description: Vengono fornite informazioni sulla migrazione di un server di AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 024f7a586c58dcf5f0d6f9c3aa291e6bef8d838b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408189"
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>Verificare la migrazione di AD FS 2,0 a Windows Server 2012 R2

Dopo aver completato la stessa migrazione server della farm Active Directory Servizio federativo (AD FS) a Windows Server 2012 R2, è possibile utilizzare la procedura seguente per verificare che i server federativi della farm siano operativi. ovvero, qualsiasi client nella stessa rete può raggiungere i server federtation.  
  
Per eseguire questa procedura è richiesta almeno l'appartenenza al gruppo **Users**, **Backup Operators**, **Power Users**, **Administrators** oppure a un gruppo equivalente.
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>Per verificare che un server federativo sia operativo  
  
1.  Aprire una finestra del browser e nella barra degli indirizzi digitare il nome dei server federativi, quindi aggiungerlo con `federationmetadata/2007-06/federationmetadata.xml` per passare all'endpoint dei metadati del servizio federativo. Ad esempio, `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml`.  
  
Se nella finestra del browser è possibile vedere i metadati del server federativo senza errori o avvisi SSL, ciò indica che il server federativo è operativo.  
  
2. È inoltre possibile passare alla pagina di accesso di ADFS (nome del servizio federativo con l'aggiunta di `adfs/ls/idpinitiatedsignon.htm`, ad esempio `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`).  Verrà visualizzata la pagina di accesso di ADFS nella quale è possibile accedere con le credenziali di amministratore di dominio.  
  
> [!IMPORTANT]
>  Assicurarsi di configurare le impostazioni del browser in modo da considerare attendibile il ruolo del server federativo aggiungendo il nome del servizio federativo, ad esempio `https://fs.contoso.com`, all'area Intranet locale del browser.  
  
## <a name="next-steps"></a>Passaggi successivi
 [Eseguire la migrazione dei servizi ruolo Active Directory Federation Services a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparazione alla migrazione del server federativo di AD FS](prepare-migrate-ad-fs-server-r2.md)  
 [Migrazione del server federativo AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrazione del proxy server federativo di AD FS](migrate-fed-server-proxy-r2.md)   
