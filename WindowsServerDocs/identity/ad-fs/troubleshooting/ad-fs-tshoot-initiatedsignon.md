---
title: Risoluzione dei problemi di AD FS-accesso avviato da IDP
description: In questo documento viene descritto come risolvere i problemi della pagina di accesso AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 89ee45bd0387cf728bc126529169b6b1ca045d6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366194"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>Risoluzione dei problemi di AD FS-accesso avviato da IDP
È possibile usare la pagina di accesso AD FS per verificare se l'autenticazione funziona o meno.  Questa operazione viene eseguita passando alla pagina ed effettuando l'accesso.  È anche possibile usare la pagina di accesso per verificare che tutte le relying party SAML 2,0 siano elencate.

## <a name="enable-the-idp-initiated-sign-on-page"></a>Abilitare la pagina di accesso avviata da IDP
Per impostazione predefinita, AD FS in Windows 2016 non è abilitata la pagina di accesso.  Per abilitarla, è possibile usare il comando PowerShell set-AdfsProperties.  Per abilitare la pagina, attenersi alla procedura seguente:

1.  Apri Windows PowerShell
2.  Immettere: `Get-AdfsProperties` e premere INVIO
3.  Verificare che **EnableIdpInitiatedSignonPage** sia impostato su false ![false](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  In PowerShell immettere: `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  Non verrà visualizzata una conferma, quindi immettere di nuovo Get-AdfsProperties e verificare che **EnableIdpInitatedSignonPage** sia impostato su true.
![True](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>Test di autenticazione
Usare la procedura seguente per testare AD FS autenticazione con la pagina di accesso avviata da IDP.

1.  Aprire un Web browser e passare alla pagina di accesso IDP.  Esempio: https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
2.  Verrà richiesto di eseguire l'accesso.  Immettere le credenziali.
](media/ad-fs-tshoot-initiatedsignon/idp5.png) di accesso ![
3.  Se l'operazione ha esito positivo, è necessario aver eseguito l'accesso.


## <a name="test-authentication-using-a-seamless-logon-experience"></a>Testare l'autenticazione con un'esperienza di accesso trasparente
È possibile testare l'esperienza di accesso trasparente assicurandosi che l'URL per i server AD FS venga aggiunto all'area Intranet locale delle opzioni Internet.  Utilizzare la procedura seguente:

1.  In un client Windows 10 fare clic su Start e digitare Opzioni Internet e selezionare Opzioni Internet.
2.   Fare clic sulla scheda sicurezza, su Intranet locale e quindi sul pulsante siti.
![](media/ad-fs-tshoot-initiatedsignon/idp8.png) trasparente
1.  Fare clic su Avanzate.
2.  Immettere l'URL e fare clic su Aggiungi.  Fare clic su Chiudi.
![Aggiungi URL](media/ad-fs-tshoot-initiatedsignon/idp9.png)
1.  Fare clic su Ok.  Fare clic su Ok.  Questa operazione dovrebbe chiudere le opzioni Internet.
2.  Aprire un Web browser e passare alla pagina di accesso IDP.  Esempio: https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
3.  Fare clic sul pulsante Accedi.  Si dovrebbe accedere automaticamente e non verranno richieste le credenziali.
![](media/ad-fs-tshoot-initiatedsignon/idp6.png) trasparente

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)
