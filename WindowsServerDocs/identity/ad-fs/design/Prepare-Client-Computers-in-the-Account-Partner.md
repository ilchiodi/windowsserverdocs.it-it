---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Preparazione dei computer client nel partner account
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5725f4a7761d08a25ee8c67c0568977e3646397e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407943"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Preparazione dei computer client nel partner account

Il modo più semplice per un amministratore in un'organizzazione partner account di preparare i computer client per l'accesso alle applicazioni federate Active Directory Federation Services \(AD FS @ no__t-1 consiste nell'utilizzare Criteri di gruppo. Criteri di gruppo fornisce un modo pratico per effettuare il push di certificati specifici e delle impostazioni necessarie per la federazione a tutti i computer client che verranno usati per accedere alle applicazioni federate.  
  
In modo che i computer client possano accedere senza problemi alle applicazioni federate senza prompt dei certificati o da un sito attendibile, si consiglia di preparare prima ogni computer client prima di distribuire AD FS in modo estensivo nell'organizzazione. Si consideri l'uso dei Criteri di gruppo per eseguire automaticamente le operazioni seguenti:  
  
-   Configurare Internet Explorer in ogni computer client per considerare attendibile il server federativo di account.  
  
    Per altre informazioni, vedere [Configurare i computer Client per considerare attendibile il Server federativo di Account](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Installare il server federativo di account appropriato, il server federativo di risorsa e il server Web Secure Sockets Layer \(SSL @ no__t-1 Certificates \(OR certificati equivalenti concatenati a una radice attendibile @ no__t-3 in ogni computer client.  
  
    Per altre informazioni, vedere [distribuire i certificati ai computer Client mediante criteri di gruppo](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
