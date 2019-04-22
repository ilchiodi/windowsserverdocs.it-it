---
title: Personalizzare le attestazioni da generare nel token ID quando si usa OAuth o OpenID Connect con AD FS 2016
description: Panoramica tecnica del conecpts token id personalizzato di AD FS 2016
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 8c8e14f22a4bca5b6d32e841814a58a4b4ddca01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820642"
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016"></a>Personalizzare le attestazioni da generare nel token ID quando si usa OAuth o OpenID Connect con AD FS 2016

## <a name="overview"></a>Panoramica
L'articolo [qui](enabling-openId-connect-with-ad-fs.md) illustra come compilare un'app che usa AD FS per OpenID Connect di accesso. Tuttavia, per impostazione predefinita sono presenti solo un set fisso di attestazioni disponibili nel token ID. AD FS 2016 ha la possibilità di personalizzare il parametro id_token negli scenari di OpenID Connect.

## <a name="when-are-custom-id-token-used"></a>Quando vengono personalizzate ID token usato?
In alcuni scenari è possibile che l'applicazione client non dispone di una risorsa che sta provando ad accedere. Pertanto, non necessita del token di accesso. In questi casi, l'applicazione client deve essenzialmente solo un ma token ID con alcune attestazioni aggiuntive per agevolare la funzionalità.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>Quali sono le restrizioni su come ottenere le attestazioni personalizzate in ID token?

### <a name="scenario-1"></a>Scenario 1

![limitare](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode viene impostato come form_post
2.  Solo i client pubblici possono ottenere le attestazioni personalizzate nell'ID token
3.  Identificatore della relying Party deve essere lo stesso identificatore client

### <a name="scenario-2"></a>Scenario 2

![limitare](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Con [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) installati nei server AD FS
1.  response_mode viene impostato come form_post
2.  Assegnare allatclaims ambito ai client e che la coppia di relying Party.
È possibile assegnare l'ambito utilizzando i cmdlet Grant-ADFSApplicationPermission come indicato nell'esempio seguente:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Creazione di un'applicazione OAuth per la gestione personalizzata delle attestazioni nel token ID
Vedere l'articolo [qui](Enabling-OpenId-Connect-with-AD-FS-2016.md) per creare un'app dell'applicazione che usa AD FS per OpenID Connect accedano. Seguire quindi la procedura seguente per configurare l'applicazione in AD FS per la ricezione di token ID con le attestazioni personalizzate.

### <a name="create-the-application-group-in-ad-fs-2016"></a>Creare il gruppo di applicazioni in AD FS 2016

1.  Creare un gruppo di applicazioni basato sul modello di nuovo, illustrato di seguito, chiamato CustomTokenClient.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

2. Questo modello crea un client riservato. Prendere nota dell'identificatore e specificare l'URI restituito come l'URL SSL del progetto di Visual Studio.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

3.  Nel passaggio successivo, selezionare **generare un segreto condiviso** per creare le credenziali client e copiare le credenziali del client generate.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

4. Fare clic su **successivo** e continuare per completare la procedura guidata.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

### <a name="create-the-relying-party"></a>Creare la Relying Party
Per aggiungere attestazioni personalizzate nell'ID token, è necessario creare un'applicazione relying Party verranno aggiunti il cui attestazioni nel token ID. Usare la procedura guidata Aggiungi Trust Relying Party per creare un nuovo relying party, come illustrato di seguito:
 
![Relying Party](media/Custom-Id-Tokens-in-AD-FS/rpsnap1.png)

Dopo la creazione della relying party, fare clic con il pulsante destro sulla voce della relying party e selezionare **Modifica criteri di rilascio di attestazioni** per aggiungere regole di rilascio delle attestazioni. Aggiungere le attestazioni personalizzate necessarie per il token ID, come illustrato di seguito:

![Relying Party](media/Custom-Id-Tokens-in-AD-FS/rpsnap2.png)

### <a name="assign-allatclaims-scope-to-the-pair-of-client-and-relying-party"></a>Assegnare l'ambito "allatclaims" alla coppia di client e della relying party
Uso di PowerShell nel server AD FS, assegnare il nuovo ambito allatclaims come indicato nell'esempio seguente (modificare l'ID client e server:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "5db77ce4-cedf-4319-85f7-cc230b7022e0" -ServerRoleIdentifier "https://customidrp1/" -ScopeNames "allatclaims","openid"
```

>[!NOTE]
>Modificare il ClientRoleIdentifier e ServerRoleIdentifier in base alle impostazioni dell'applicazione

## <a name="test-the-custom-claims-in-id-token"></a>Testare le attestazioni personalizzate nel token ID

Quindi, utilizza la stessa porzione di codice che è sempre stato utilizzato per accedere alle attestazioni, è possibile visualizzare le attestazioni aggiuntive che diventeranno parte del token ID.
Ad esempio, in un'app MVC .NET di esempio, aprire uno dei file del controller e immettere il codice come il seguente:


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
>Tenere presente che il parametro di risorsa è obbligatorio nella richiesta Oauth2.
>
>Esempio non valida:
>
>**HTTPS&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token a & mbito = openid & redirect_uri = https&#58;//myportal/auth & nonce = b3ceb943fc756d927777 & client_id = 6db3ec2a-075a - 4c 72-9b22-ca7ab60cb4e7 & stato = 42c2c156aef47e8d0870 & resource = 6db3ec2a-075a - 4c 72-9b22-ca7ab60cb4e7**
>
>Ottimo esempio:
>
>**HTTPS&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token a & mbito = openid & redirect_uri = https&#58;//myportal/auth & nonce = b3ceb943fc756d927777 & client_id = 6db3ec2a-075a - 4c 72-9b22-ca7ab60cb4e7 & stato = 42c2c156aef47e8d0870 & resource = https&#58;//customidrp1/ & response_mode = form_post**
>
>Se il parametro della risorsa non è presente nella richiesta che è possibile che venga visualizzato un errore o un token senza eventuali attestazioni personalizzate.

## <a name="next-steps"></a>Passaggi successivi
[Sviluppare AD FS](../../ad-fs/AD-FS-Development.md)  
