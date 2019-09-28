---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Registrare un certificato SSL per ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: efa7c7aee848a5bbb68d3ce7140e135d37c2161d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408360"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Registrare un certificato SSL per ADFS

Active Directory Federation Services \(AD FS @ no__t-1 richiede un certificato per Secure Socket Layer \(SSL @ no__t-3 autenticazione server in ogni server federativo del server farm di Federazione. Lo stesso certificato può essere utilizzato in ogni server federativo di una farm. È necessario che siano disponibili sia il certificato che la relativa chiave privata. Ad esempio, se il certificato e la relativa chiave privata sono disponibili in un file con estensione pfx, è possibile importare il file direttamente nella configurazione guidata di Active Directory Federation Services. Questo certificato SSL deve contenere quanto segue:  
  
1.  Il nome del soggetto e il nome alternativo del soggetto devono contenere il nome del servizio federativo, ad esempio fs.contoso.com.  
  
2.  Il nome alternativo del soggetto deve contenere il valore **enterpriseregistration** seguito dal nome dell'entità utente \(UPN @ no__t-2 suffisso dell'organizzazione, ad esempio **enterpriseregistration.Corp.contoso.com**.  
  
    > [!WARNING]  
    > Specificare il nome alternativo del soggetto se si intende abilitare il servizio registrazione dispositivo \(DRS @ no__t-1 per Workplace Join.  
  
> [!IMPORTANT]  
> Se l'organizzazione usa più suffissi UPN e si prevede di abilitare DRS, il certificato SSL deve contenere una voce di nome alternativo del soggetto per ogni suffisso.  
  
## <a name="see-also"></a>Vedere anche
[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

