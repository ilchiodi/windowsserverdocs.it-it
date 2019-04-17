---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Preparazione dei computer Client nel Partner Account
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c5bdcb0a80b15a1905109229ddd20ee642a8dd7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Preparazione dei computer Client nel Partner Account

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il modo più semplice per un amministratore in un'organizzazione partner account per preparare i computer client per l'accesso alle applicazioni di Active Directory Federation Services \(AD FS\) federato consiste nell'usare criteri di gruppo. Criteri di gruppo fornisce un modo pratico per effettuare il push di certificati specifici e le impostazioni necessarie per la federazione a tutti i computer client che verrà utilizzato per accedere alle applicazioni federate.  
  
In modo che i computer client possono accedere facilmente alle applicazioni federate senza prompt relativi ai siti attendibili o richieste di certificato, è consigliabile preparare ogni computer client prima di distribuire ADFS su larga scala nell'organizzazione. È consigliabile usare automaticamente i criteri di gruppo:  
  
-   Configurare Internet Explorer in ogni computer client per considerare attendibile il server federativo di account.  
  
    Per ulteriori informazioni, vedere [configurare i computer Client per considerare attendibile il Server federativo di Account](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Installare il server federativo di account appropriato, server federativo di risorsa e certificati SSL (Secure Sockets Layer) \(SSL\) server Web \ (o equivalente certificati concatenati a un'attendibile root) in ogni computer client.  
  
    Per ulteriori informazioni, vedere [distribuire i certificati ai computer Client mediante criteri di gruppo](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
