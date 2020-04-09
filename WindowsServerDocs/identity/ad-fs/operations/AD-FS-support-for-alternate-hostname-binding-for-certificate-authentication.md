---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: Supporto di AD FS per l'associazione di nomi host alternativi per l'autenticazione del certificato
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e3764977e29413ea1e361fa78cadd040adabcf04
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858094"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>Supporto di AD FS per l'associazione di nomi host alternativi per l'autenticazione del certificato

In molte reti i criteri firewall locali potrebbero non consentire il traffico attraverso le porte non standard come 49443. Questo è un problema quando si tenta di eseguire l'autenticazione del certificato con AD FS prima di AD FS in Windows Server 2016. Questo avviene perché potrebbe non avere associazioni diverse per l'autenticazione del dispositivo e l'autenticazione del certificato utente nello stesso host. La porta predefinita 443 è associata a ricevere i certificati del dispositivo e non può essere modificata per supportare l'associazione più il canale stesso. I risultati sono stati che non è possibile utilizzare l'autenticazione della smart card e gli utenti erano consapevoli di ciò che si è verificato poiché non esiste alcuna indicazione di cosa è successo veramente.  
  
Con AD FS in Windows Server 2016 questa operazione può essere eseguita.
  
In ADFS in Windows Server 2016 situazione è cambiata. A questo punto si supportano due modalità, il primo Usa lo stesso host (ad esempio adfs.contoso.com) con porte diverse (443, 49443). Il secondo utilizza host diversi (adfs.contoso.com e certauth.adfs.contoso.com) con la stessa porta (443). Ciò richiede un certificato SSL per il supporto "certauth. < nome del servizio adfs >" come nome di soggetto alternativo. Questa operazione può essere eseguita al momento della creazione della farm o in un secondo momento tramite PowerShell.  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>Procedura: configurare l'associazione del nome host alternativo per l'autenticazione del certificato  
Esistono due metodi che è possibile aggiungere l'associazione di nome host alternativo per l'autenticazione del certificato. Il primo è quando si configura una nuova farm ADFS con AD FS per Windows Server 2016, se il certificato contiene un nome alternativo del soggetto (SAN), che verrà automaticamente per utilizzare il secondo metodo indicato in precedenza l'installazione. Ovvero, configurerà automaticamente due host diversi (sts.contoso.com e certauth.sts.contoso.com con la stessa porta. Se il certificato non contiene una SAN, verrà visualizzato un avviso che informa che i nomi alternativi del soggetto del certificato non supportano certauth. *. Vedere le schermate riportate di seguito. Il primo argomento viene illustrata un'installazione in cui il certificato ha una rete SAN e la seconda figura mostra un certificato che non è stato eseguito.  
  
![associazione di nome host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![associazione di nome host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
Allo stesso modo, dopo la distribuzione di AD FS in Windows Server 2016, è possibile usare il cmdlet di PowerShell: set-AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

Quando richiesto, fare clic su Sì per confermare.  E che sia.

![associazione di nome host alternativo](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>Altri riferimenti

* [Gestione dei certificati SSL in AD FS e WAP in Windows Server 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
