---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: Supporto di ADFS per l'associazione di nome host alternativo per l'autenticazione del certificato
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553ff059693c7b0c0e6f0364d82c1adbca661097
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>Supporto di ADFS per l'associazione di nome host alternativo per l'autenticazione del certificato

>Si applica a: Windows Server 2016

In molte reti i criteri firewall locali potrebbero non consentire il traffico tramite le porte non standard come 49443. Questo è diventato un problema durante il tentativo di eseguire l'autenticazione del certificato con AD FS prima di ADFS in Windows Server 2016. Questo avviene perché potrebbe non avere associazioni diverse per l'autenticazione del dispositivo e l'autenticazione del certificato utente nello stesso host. La porta predefinita 443 è associata a ricevere certificati del dispositivo e non può essere modificata per supportare l'associazione più il canale stesso. I risultati sono stati che non funzionava autenticazione con smart card e gli utenti erano consapevoli di cosa è successo poiché non esiste alcuna indicazione di cosa è successo veramente.  
  
Con AD FS in Windows Server 2016 questo può essere eseguito.
  
In ADFS in Windows Server 2016 situazione è cambiata. A questo punto si supportano due modalità, il primo Usa lo stesso host (ad esempio adfs.contoso.com) con porte diverse (443, 49443). Il secondo utilizza host diversi (adfs.contoso.com e certauth.adfs.contoso.com) con la stessa porta (443). Sarà necessario un certificato SSL per il supporto "certauth. < nome del servizio adfs >" come nome di soggetto alternativo. Questa operazione può essere eseguita al momento della creazione di farm o in un secondo momento tramite PowerShell.  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>Come configurare l'associazione di nome host alternativo per l'autenticazione del certificato  
Esistono due metodi che è possibile aggiungere l'associazione di nome host alternativo per l'autenticazione del certificato. Il primo è quando si configura una nuova farm ADFS con AD FS per Windows Server 2016, se il certificato contiene un nome alternativo del soggetto (SAN), che verrà automaticamente per utilizzare il secondo metodo indicato in precedenza l'installazione. Ovvero, configurerà automaticamente due host diversi (sts.contoso.com e certauth.sts.contoso.com con la stessa porta. Se il certificato non contiene una rete SAN, si vedranno un messaggio di avviso che informa che i nomi alternativi del soggetto certificato non supporta certauth. Vedere le schermate riportate di seguito. Il primo argomento viene illustrata un'installazione in cui il certificato ha una rete SAN e la seconda figura mostra un certificato che non è stato.  
  
![associazione di nome host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![associazione di nome host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
Dopo la distribuzione di ADFS in Windows Server 2016 allo stesso modo, è possibile utilizzare il cmdlet PowerShell: Set-AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

Quando richiesto, fare clic su Sì per confermare.  E che sia.

![associazione di nome host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>Riferimenti aggiuntivi

* [Gestione di certificati SSL in ADFS e WAP in Windows Server 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
