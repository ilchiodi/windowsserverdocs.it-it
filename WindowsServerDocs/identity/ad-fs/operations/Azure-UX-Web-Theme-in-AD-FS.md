---
title: Tema Web di Azure AD UX in AD FS
description: Il documento seguente illustra come modificare l'accesso AD FS Forms in modo che sia simile all'esperienza utente Azure AD.
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f4dd1d45646475be3788cd6b615b1743976eedae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358406"
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>Uso di un tema Web Azure AD UX in Active Directory Federation Services
L'accesso AD FS Forms attualmente non esegue il mirroring dell'esperienza di accesso di Azure/O365.  Per offrire un'esperienza più uniforme e trasparente per gli utenti finali, abbiamo rilasciato il tema Web del foglio di stile CSS che è possibile applicare ai server AD FS.  Attualmente, l'accesso ai moduli per AD FS in Windows Server 2016 è simile al seguente:

![Accesso corrente](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


Con il nuovo foglio di stile, l'esperienza utente sarà più simile alle esperienze di accesso di Azure e Office 365.

## <a name="download-the-css-style-sheet"></a>Scaricare il foglio di stile CSS
È possibile scaricare il tema Web dal seguente [percorso](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi)di GitHub.


## <a name="enabling-the-new-web-theme"></a>Abilitazione del nuovo tema Web
Per abilitare il nuovo tema Web, usare la procedura seguente:

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>Per abilitare il nuovo tema Web Azure AD UX in AD FS
1. Avviare PowerShell come amministratore
2. Creare un nuovo tema Web usando PowerShell:`New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3. Impostare il nuovo tema come tema attivo usando PowerShell:  `Set-AdfsWebConfig -ActiveThemeName custom`
   ![PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4. Testare l'accesso passando a https://<AD FS name.domain>/ADFS/LS/idpinitiatedsignon.htm ![Sign-on](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

> ! Si noti È necessario assicurarsi che idpinitiatedsignon sia stato abilitato.  Non è abilitato per impostazione predefinita.  Per abilitare idpinitiatedsignon, usare il comando di PowerShell seguente:`Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>Suggerimenti per le immagini
L'abilitazione dell'interfaccia utente centrata consente di usare le stesse immagini per lo sfondo e il logo che potrebbero essere già disponibili per Azure Active Directory informazioni personalizzate distintive dell'azienda. In genere, vengono applicate le stesse raccomandazioni per le dimensioni, il rapporto e il formato.

### <a name="logo"></a>Logo

Descrizione | Vincoli | Consigli
------- | ------- | ----------
Il logo viene visualizzato nella parte superiore del pannello di accesso. | JPG trasparente o PNG<br>Altezza massima: 36 px<br>Larghezza massima: 245 px | Usare il logo dell'organizzazione qui.<br>Usare un'immagine trasparente. Non presupporre che lo sfondo sarà bianco.<br>Non aggiungere spaziatura interna intorno al logo nell'immagine o il logo apparirà in modo sproporzionato.

### <a name="background"></a>Sfondo

Descrizione | Vincoli | Consigli
------- | ------- | ----------
Questa opzione viene visualizzata sullo sfondo della pagina di accesso, è ancorata al centro dello spazio visualizzabile e viene ridimensionata e ritagliata per riempire la finestra del browser.    <br>In schermate strette, ad esempio telefoni cellulari, questa immagine non viene visualizzata.<br>Quando la pagina viene caricata, viene applicata una maschera nera con opacità 0,55 sull'immagine. | JPG o PNG<br>Dimensioni immagine: 1920x1080 px<br>Dimensioni del file: &lt;300 KB | <br>Usare le immagini in cui non è presente uno stato attivo sicuro per l'oggetto. Il form di accesso opaco viene visualizzato al centro dell'immagine e può coprire qualsiasi parte dell'immagine, a seconda delle dimensioni della finestra del browser.<br>Mantenere le dimensioni del file ridotte per garantire tempi di caricamento rapidi.

## <a name="next-steps"></a>Passaggi successivi
- [Personalizzazione AD FS in Windows Server 2016](AD-FS-Customization-in-Windows-Server-2016.md)
- [Personalizzazione avanzata](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [Temi Web personalizzati](Custom-Web-Themes-in-AD-FS.md)
