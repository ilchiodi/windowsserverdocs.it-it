---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Registrare un certificato SSL per ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 12f544ad0d037c4ae7a9789238186b7ded311bdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825242"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Registrare un certificato SSL per ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Active Directory Federation Services \(ADFS\) richiede un certificato per SSL (Secure Socket Layer) \(SSL\) autenticazione server in ogni server federativo nella server farm federativa. Lo stesso certificato può essere utilizzato in ogni server federativo in una farm. È necessario che siano disponibili sia il certificato che la relativa chiave privata. Ad esempio, se il certificato e la relativa chiave privata sono disponibili in un file con estensione pfx, è possibile importare il file direttamente nella configurazione guidata di Active Directory Federation Services. Questo certificato SSL deve contenere quanto segue:  
  
1.  Il nome del soggetto e nome alternativo del soggetto deve contenere il nome del servizio federativo, ad esempio fs.contoso.com.  
  
2.  Il nome alternativo del soggetto deve contenere il valore **enterpriseregistration** seguito dal nome dell'entità utente \(UPN\) suffisso dell'organizzazione, ad esempio,  **enterpriseregistration.corp.contoso.com**.  
  
    > [!WARNING]  
    > Se si prevede di abilitare il servizio registrazione dispositivo, specificare il nome alternativo del soggetto \(DRS\) aggiunta alla rete aziendale.  
  
> [!IMPORTANT]  
> Se l'organizzazione Usa più suffissi UPN, e si prevede di abilitare il servizio DRS, il certificato SSL deve contenere una voce di nome alternativo del soggetto per ogni suffisso.  
  
## <a name="see-also"></a>Vedere anche
[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una Server Farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

