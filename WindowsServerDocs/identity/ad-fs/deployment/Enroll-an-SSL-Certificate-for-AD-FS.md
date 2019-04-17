---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Registrare un certificato SSL per ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 12f544ad0d037c4ae7a9789238186b7ded311bdf
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Registrare un certificato SSL per ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Active Directory Federation Services \(AD FS\) richiede un certificato per l'autenticazione server SSL (Secure Socket Layer) \(SSL\) in ogni server federativo nella server farm federativa. Lo stesso certificato può essere utilizzato in ogni server federativo in una farm. È necessario disporre le chiavi private disponibili sia il certificato. Ad esempio, se hai il certificato e la relativa chiave privata in un file con estensione pfx, è possibile importare il file direttamente nella configurazione guidata servizi di federazione Active Directory. Questo certificato SSL deve contenere quanto segue:  
  
1.  Il nome soggetto e nome alternativo del soggetto deve contenere il nome del servizio federativo, ad esempio fs.contoso.com.  
  
2.  Il nome alternativo del soggetto deve contenere il valore **enterpriseregistration** seguito dal nome dell'entità utente \(UPN\) suffisso dell'organizzazione, ad esempio, **enterpriseregistration.corp.contoso.com**.  
  
    > [!WARNING]  
    > Se si intende abilitare il servizio Registrazione dispositivi \(DRS\) per l'unione all'area di lavoro, specificare il nome alternativo del soggetto.  
  
> [!IMPORTANT]  
> Se l'organizzazione Usa più suffissi UPN e si prevede di abilitare il servizio DRS, il certificato SSL deve contenere una voce di nome alternativo del soggetto per ogni suffisso.  
  
## <a name="see-also"></a>Vedere anche
[Distribuzione di ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una Server Farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

