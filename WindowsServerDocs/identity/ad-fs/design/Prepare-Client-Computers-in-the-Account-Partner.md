---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Preparazione dei computer client nel partner account
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e3a746ec003cf312ffe0b9804f84a55c98aa8089
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190994"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Preparazione dei computer client nel partner account

Il modo più semplice per un amministratore in un account organizzazione partner di preparare i computer client per l'accesso a Active Directory Federation Services \(ADFS\) applicazioni federate consiste nell'usare criteri di gruppo. Criteri di gruppo fornisce un modo pratico per effettuare il push di certificati specifici e delle impostazioni necessarie per la federazione a tutti i computer client che verranno usati per accedere alle applicazioni federate.  
  
In modo che i computer client possano accedere facilmente alle applicazioni federate senza prompt relativi ai siti attendibili o richieste di certificato, è consigliabile preparare ogni computer client prima di distribuire su vasta scala AD FS nell'organizzazione. Si consideri l'uso dei Criteri di gruppo per eseguire automaticamente le operazioni seguenti:  
  
-   Configurare Internet Explorer in ogni computer client per considerare attendibile il server federativo di account.  
  
    Per altre informazioni, vedere [Configure Client Computers to Trust the Account Federation Server](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Installare il server federativo di account appropriato, un server federativo di risorsa e un server Web Secure Sockets Layer \(SSL\) certificati \(o equivalente, i certificati concatenati a una fonte attendibile\) in ogni computer client.  
  
    Per altre informazioni, vedere [distribuire i certificati ai computer Client mediante criteri di gruppo](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
