---
title: Personalizzare le attestazioni devono essere create in id_token quando si utilizza la connessione OpenID o OAuth con AD FS 2016
description: Panoramica tecnica di conecpts token l'id personalizzato in AD FS 2016
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: c17d50bb6d90726eafc3e585f16a7a8189a68392
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016"></a>Personalizzare le attestazioni devono essere create in id_token quando si utilizza la connessione OpenID o OAuth con AD FS 2016

## <a name="overview"></a>Panoramica
The article [here](enabling-openId-connect-with-ad-fs.md) shows how to build an app that uses AD FS for OpenID Connect sign on. Tuttavia, per impostazione predefinita sono disponibili solo un set di attestazioni disponibili in id_token fisso. AD FS 2016 ha la possibilità di personalizzare id_token in OpenID Connect scenari.

## <a name="when-are-custom-id-token-used"></a>Quando sono ID personalizzato token utilizzato?
In alcuni scenari è possibile che l'applicazione client non dispone di una risorsa che sta tentando di accedere. Pertanto, non deve effettivamente un token di accesso. In questi casi, l'applicazione client deve essenzialmente solo, ma token un ID con alcuni attestazioni per consentire la funzionalità aggiuntive.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>Quali sono le restrizioni su come ottenere attestazioni personalizzate in ID token?

### <a name="scenario-1"></a>Scenario 1

![Limitare](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode viene impostato come form_post
2.  Solo i client pubblici ottenere attestazioni personalizzate nell'ID di token
3.  Identificatore della relying Party deve essere lo stesso identificatore client

### <a name="scenario-2"></a>Scenario 2

![Limitare](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Con [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) installato nel server ADFS
1.  response_mode viene impostato come form_post
2.  Assegnare al client – coppia RP allatclaims ambito.
È possibile assegnare l'ambito utilizzando il cmdlet Grant-ADFSApplicationPermission come indicato nell'esempio seguente:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Creazione di un'applicazione di OAuth per gestire le attestazioni personalizzate nel token ID
Utilizzare l'articolo [qui](Enabling-OpenId-Connect-with-AD-FS-2016.md) per creare un'app di applicazione che utilizza ADFS per OpenID Connect accedere. Seguire la procedura seguente per configurare l'applicazione in ADFS per la ricezione di token ID con attestazioni personalizzate.

### <a name="create-the-application-group-in-ad-fs-2016"></a>Creare il gruppo di applicazioni in AD FS 2016

1.  Creare un gruppo di applicazioni in base al nuovo modello, mostrato di seguito, denominato CustomTokenClient.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

2. Questo modello crea un client riservato. Prendere nota dell'identificatore e specificare l'URI restituito come l'URL SSL del progetto di Visual Studio.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

3.  Nel passaggio successivo, selezionare **generare un segreto condiviso** per creare credenziali di client e copiare le credenziali del client generate.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

4. Fare clic su **Avanti** e passare a completare la procedura guidata.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

### <a name="create-the-relying-party"></a>Creare una Relying Party
Per aggiungere attestazioni personalizzate in ID di token, è necessario creare un'applicazione relying Party verranno aggiunto il cui attestazioni nel token di ID. Utilizzare la procedura guidata Aggiungi attendibilità per creare un nuovo componente, come illustrato di seguito:
 
![Relying Party](media/Custom-Id-Tokens-in-AD-FS/rpsnap1.png)

Dopo la creazione di relying party, fare clic sulla voce relying party e selezionare **Modifica criteri di rilascio di attestazioni** aggiungere regole di rilascio delle attestazioni. Aggiungere le attestazioni personalizzate necessarie per il token di ID, come illustrato di seguito:

![Relying Party](media/Custom-Id-Tokens-in-AD-FS/rpsnap2.png)

### <a name="assign-allatclaims-scope-to-the-pair-of-client-and-relying-party"></a>Assegnare la coppia di client e relying party "allatclaims" ambito
Utilizzo di PowerShell nel server AD FS, assegnare il nuovo ambito allatclaims, come indicato nell'esempio seguente (modificare il clientID e il server:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "5db77ce4-cedf-4319-85f7-cc230b7022e0" -ServerRoleIdentifier "https://customidrp1/" -ScopeNames "allatclaims","openid"
```

>[!NOTE]
>Modificare il ClientRoleIdentifier e ServerRoleIdentifier in base alle impostazioni dell'applicazione

## <a name="test-the-custom-claims-in-id-token"></a>Testare le attestazioni personalizzate nel token ID

Quindi, Usa il bit del codice che sempre utilizzato per accedere attestazioni stesso, è possibile visualizzare le attestazioni aggiuntive che faranno parte dell'id_token.
Ad esempio, in un'app di esempio MVC .NET, aprire uno dei file del controller e immettere codice simile di seguito:


``` code
    [Authorize]
    public ActionResult About()
    {

        ClaimsPrincipal cp = ClaimsPrincipal.Current;

        string userName = cp.FindFirst(ClaimTypes.GivenName).Value;
        ViewBag.Message = String.Format("Hello {0}!", userName);
        return View();

    }

```

>[!NOTE]
>Please be aware that the resource parameter is required in the Oauth2 request.
>
>Bad example:
>
>**https&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token&scope=openid&redirect_uri=https&#58;//myportal/auth&nonce=b3ceb943fc756d927777&client_id=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7&state=42c2c156aef47e8d0870&resource=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7**
>
>Good example:
>
>**https&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token&scope=openid&redirect_uri=https&#58;//myportal/auth&nonce=b3ceb943fc756d927777&client_id=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7&state=42c2c156aef47e8d0870&resource=https&#58;//customidrp1/**
>
>If the resource parameter is not in the request you may recieve an error or a token without any custom claims.

## <a name="next-steps"></a>Passaggi successivi
[Sviluppare AD FS](../../ad-fs/AD-FS-Development.md)  