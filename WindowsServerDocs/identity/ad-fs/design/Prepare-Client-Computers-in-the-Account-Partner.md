---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Preparazione dei computer client nel partner account
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 014524c78312c6fcd478b40ec47e212a194b0531
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858594"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Preparazione dei computer client nel partner account

Il modo più semplice per un amministratore in un'organizzazione partner account di preparare i computer client per l'accesso a Active Directory Federation Services \(AD FS\) applicazioni federate consiste nell'utilizzare Criteri di gruppo. Criteri di gruppo fornisce un modo pratico per effettuare il push di certificati specifici e delle impostazioni necessarie per la federazione a tutti i computer client che verranno usati per accedere alle applicazioni federate.  
  
In modo che i computer client possano accedere senza problemi alle applicazioni federate senza prompt dei certificati o da un sito attendibile, si consiglia di preparare prima ogni computer client prima di distribuire AD FS in modo estensivo nell'organizzazione. Si consideri l'uso dei Criteri di gruppo per eseguire automaticamente le operazioni seguenti:  
  
-   Configurare Internet Explorer in ogni computer client per considerare attendibile il server federativo di account.  
  
    Per altre informazioni, vedere [Configure Client Computers to Trust the Account Federation Server](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Installare il server federativo di account appropriato, il server federativo di risorsa e il server Web Secure Sockets Layer \(certificati SSL\) \(o certificati equivalenti concatenati a una\) radice attendibile in ogni computer client.  
  
    Per ulteriori informazioni, vedere la pagina relativa [alla distribuzione di certificati ai computer client tramite criteri di gruppo](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
