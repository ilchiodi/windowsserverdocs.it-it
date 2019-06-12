---
title: Tema Web dell'esperienza utente di Azure AD in AD FS
description: Il documento seguente viene descritto come modificare lo sign-in form di AD FS in modo che somigli l'esperienza utente di Azure AD.
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1064084fe357e54d7230f58e486aa4e62958f6ae
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445016"
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>Utilizzo di un tema Web dell'esperienza utente di Azure AD in Active Directory Federation Services
Form di AD FS sign in attualmente non rispecchia l'esperienza di accesso di Azure/Office 365.  Per fornire un'esperienza più uniforme e trasparente per gli utenti finali, Microsoft ha rilasciato il follow a cascata tema foglio di stile web che può essere applicato ai server AD FS.  Attualmente, il form Accedi per AD FS in Windows Server 2016 simile al seguente:

![Accedi corrente](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


Con il nuovo foglio di stile, l'esperienza utente avrà un aspetto più simile di Azure e Office 365 esperienze di accesso.

## <a name="download-the-css-style-sheet"></a>Scaricare il foglio di stile CSS
È possibile scaricare il tema web da Github seguente [posizione](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi).


## <a name="enabling-the-new-web-theme"></a>Abilitare il nuovo tema web
Per abilitare il nuovo tema web usare la procedura seguente:

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>Per abilitare il nuovo tema web dell'esperienza utente di Azure AD in AD FS
1. Avviare PowerShell come amministratore
2. Creare un nuovo tema web usando PowerShell:  `New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3. Impostare il nuovo tema come il tema attivo usando PowerShell:  `Set-AdfsWebConfig -ActiveThemeName custom`
   ![PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4. Testare l'accesso, passare a https://<AD FS name.domain>/adfs/ls/idpinitiatedsignon.htm ![Sign-on](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

> ! [NOTA] È necessario assicurarsi che tale idpinitiatedsignon è stata abilitata.  Non è abilitato per impostazione predefinita.  Per abilitare idpinitiatedsignon usare il comando PowerShell seguente:  `Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>Consigli di immagine
Abilitazione dell'interfaccia utente centrato consente di usare le stesse immagini per sfondi e logo che se si dispone già di branding aziendale Azure Active Directory. In genere, si applicano le stesse raccomandazioni per le dimensioni, rapporto e il formato.

### <a name="logo"></a>Logo

Descrizione | Vincoli | Consigli
------- | ------- | ----------
Il logo viene visualizzato nella parte superiore del Pannello di accesso. | JPG o PNG trasparente<br>Altezza massima: 36 px<br>Larghezza massima: 245 px | Usare il logo dell'organizzazione.<br>Usare un'immagine trasparente. Non dare per scontato che lo sfondo sarà bianco.<br>Non aggiungere spaziatura interna intorno al logo nell'immagine o il logo apparirà sproporzionatamente piccolo.

### <a name="background"></a>Informazioni

Descrizione | Vincoli | Consigli
------- | ------- | ----------
Questa opzione viene visualizzata sullo sfondo della pagina di accesso, è ancorata al centro dell'area visualizzabile e viene ridimensionata e ritagliata per riempire la finestra del browser.    <br>Negli schermi stretti, come telefoni cellulari, questa immagine non viene visualizzata.<br>Una maschera nera con opacità 0,55 viene applicata su questa immagine viene caricata la pagina. | JPG o PNG<br>Dimensioni dell'immagine: 1920 x 1080 pixel<br>Dimensioni del file: &lt; 300 KB | <br>Usare le immagini in cui non è un argomento sicuro specifico. Il modulo di accesso opaco viene visualizzato sopra il centro di questa immagine e può coprire qualsiasi parte dell'immagine, a seconda delle dimensioni della finestra del browser.<br>Mantenere le dimensioni del file piccole per garantire tempi di caricamento rapidi.

## <a name="next-steps"></a>Passaggi successivi
- [Personalizzazione di AD FS in Windows Server 2016](AD-FS-Customization-in-Windows-Server-2016.md)
- [Personalizzazione avanzata](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [Temi web personalizzati](Custom-Web-Themes-in-AD-FS.md)
