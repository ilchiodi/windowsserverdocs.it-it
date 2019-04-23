---
title: AD FS risoluzione dei problemi - avviato dal provider di identità di accesso
description: Questo documento descrive come risolvere i problemi di accesso di AD FS nella pagina.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 61e9adc708e95a6ab4a82550280737b2f4bad0a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844942"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>AD FS risoluzione dei problemi - avviato dal provider di identità di accesso
Pagina di sign-on di AD FS è utilizzabile per verificare se l'autenticazione funzioni o meno.  Questa operazione viene eseguita passando alla pagina e l'accesso.  Inoltre, è possibile usare la pagina di accesso per verificare che siano elencate tutte le relying party SAML 2.0.

## <a name="enable-the-idp-intiated-sign-on-page"></a>Abilitare la pagina di accesso provider di identità avviato
Per impostazione predefinita, ADFS in Windows 2016 non dispone di accesso nella pagina abilitata.  Per attivarli è possibile usare il comando di PowerShell Set-AdfsProperties.  Usare la procedura seguente per abilitare la pagina:

1.  Aprire Windows PowerShell
2.  Immettere: `Get-AdfsProperties` e premere INVIO
3.  Verificare che **EnableIdpInitiatedSignonPage** è impostata su false ![False](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  In PowerShell, immettere:  `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  Non verrà visualizzato un messaggio di conferma quindi, immettere di nuovo Get-AdfsProperties e verificare che **EnableIdpInitatedSignonPage** è impostato su true.
![True](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>Test di autenticazione
Utilizzare la procedura seguente per testare l'autenticazione di AD FS con il segno Idp-Initiated nella pagina.

1.  Aprire un web browser e passare alla pagina di accesso provider di identità.  Esempio:  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
2.  Verrà richiesto di accedere.  Immettere le credenziali.
![Sign-on](media/ad-fs-tshoot-initiatedsignon/idp5.png)
3.  Se questo ha avuto esito positivo dovrebbe accedere.


## <a name="test-authentication-using-a-seamless-logon-experience"></a>Testare l'autenticazione usando un'esperienza di accesso trasparente
È possibile testare l'esperienza di accesso trasparente, verificare che l'URL per i server AD FS vengano aggiunti all'area intranet locale delle opzioni di internet.  Utilizzare la procedura seguente:

1.  In un client Windows 10, fare clic su start e digitare le opzioni di internet e selezionare le opzioni di internet.
2.   Fare clic sulla scheda sicurezza, fare clic sulla rete intranet locale e fare clic sul pulsante con i siti.
![Senza problemi](media/ad-fs-tshoot-initiatedsignon/idp8.png)
1.  Fare clic su Avanzate.
2.  Immettere l'url e fare clic su Aggiungi.  Fare clic su Chiudi.
![Aggiungere l'url](media/ad-fs-tshoot-initiatedsignon/idp9.png)
1.  Fare clic su Ok.  Fare clic su Ok.  Questo deve essere chiuso opzioni internet.
2.  Aprire un web browser e passare alla pagina di accesso provider di identità.  Esempio:  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
3.  Fare clic sul pulsante Accedi.  Dovrebbe accedere automaticamente e non verranno richieste le credenziali.
![Senza problemi](media/ad-fs-tshoot-initiatedsignon/idp6.png)

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi di AD FS](ad-fs-tshoot-overview.md)