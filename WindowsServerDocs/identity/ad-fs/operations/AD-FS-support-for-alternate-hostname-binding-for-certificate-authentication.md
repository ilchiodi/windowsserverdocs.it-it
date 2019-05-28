---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: Supporto di AD FS per l'associazione di nomi host alternativi per l'autenticazione del certificato
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3e3d1e5d86afbef2fdabd211047f513d31a40300
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190317"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>Supporto di AD FS per l'associazione di nomi host alternativi per l'autenticazione del certificato

In molte reti i criteri firewall locali potrebbero non consentire il traffico attraverso le porte non standard come 49443. Questo è diventato un problema durante il tentativo di eseguire l'autenticazione del certificato con AD FS prima di ADFS in Windows Server 2016. Questo avviene perché potrebbe non avere associazioni diverse per l'autenticazione del dispositivo e l'autenticazione del certificato utente nello stesso host. La porta predefinita 443 è associata a ricevere i certificati del dispositivo e non può essere modificata per supportare l'associazione più il canale stesso. I risultati sono stati che non è possibile utilizzare l'autenticazione della smart card e gli utenti erano consapevoli di ciò che si è verificato poiché non esiste alcuna indicazione di cosa è successo veramente.  
  
Con AD FS in Windows Server 2016 a questo scopo.
  
In ADFS in Windows Server 2016 situazione è cambiata. A questo punto si supportano due modalità, il primo Usa lo stesso host (ad esempio adfs.contoso.com) con porte diverse (443, 49443). Il secondo utilizza host diversi (adfs.contoso.com e certauth.adfs.contoso.com) con la stessa porta (443). Ciò richiede un certificato SSL per il supporto "certauth. < nome del servizio adfs >" come nome di soggetto alternativo. Questa operazione può essere eseguita al momento della creazione della farm o in un secondo momento tramite PowerShell.  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>Procedura: configurare l'associazione del nome host alternativo per l'autenticazione del certificato  
Esistono due metodi che è possibile aggiungere l'associazione di nome host alternativo per l'autenticazione del certificato. Il primo è quando si configura una nuova farm ADFS con AD FS per Windows Server 2016, se il certificato contiene un nome alternativo del soggetto (SAN), che verrà automaticamente per utilizzare il secondo metodo indicato in precedenza l'installazione. Ovvero, configurerà automaticamente due host diversi (sts.contoso.com e certauth.sts.contoso.com con la stessa porta. Se il certificato non contiene una rete SAN, quindi verrà visualizzato un avviso che informa che i nomi alternativi del soggetto certificato non supporta certauth. Vedere le schermate riportate di seguito. Il primo argomento viene illustrata un'installazione in cui il certificato ha una rete SAN e la seconda figura mostra un certificato che non è stato eseguito.  
  
![associazione di nome host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![associazione di nome host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
Analogamente, dopo la distribuzione di AD FS in Windows Server 2016 è possibile utilizzare il cmdlet di PowerShell: Set-AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

Quando richiesto, fare clic su Sì per confermare.  E che sia.

![associazione di nome host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>Altri riferimenti

* [La gestione dei certificati SSL in AD FS e WAP in Windows Server 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
