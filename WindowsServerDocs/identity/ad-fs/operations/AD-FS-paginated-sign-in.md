---
title: AD FS l'accesso impaginato
description: Questo documento descrive la nuova esperienza di accesso per AD FS 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ca13ebe29b0a9260302599110f333d166681abdb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358565"
---
# <a name="ad-fs-paginated-sign-in"></a>AD FS l'accesso impaginato


Per AD FS in Windows Server 2019, l'interfaccia utente di accesso è stata riprogettata.  A questo punto, l'accesso AD FS avrà lo stesso aspetto di Azure AD.  Ciò consente agli utenti di ottenere un'esperienza di accesso più coerente, incorporando un flusso utente centrato e impaginato.

## <a name="whats-changing"></a>Modifiche
In AD FS in Windows Server 2012 R2 e 2016, la schermata di accesso ha un aspetto simile al seguente:

![oldsignin](media/AD-FS-paginated-sign-in/signin1.png)

Viene visualizzato un solo modulo situato sul lato destro dello schermo.

In AD FS in Windows Server 2019, queste sono le principali modifiche di progettazione che verranno visualizzate:


- **Interfaccia utente centrata**. In precedenza, l'interfaccia utente di accesso era disponibile sul lato destro dello schermo, come illustrato sopra. Il front-end dell'interfaccia utente è stato spostato per modernizzare l'esperienza.
- **Impaginazione**. Invece di fornire un modulo esteso da compilare, abbiamo incorporato un nuovo flusso che ti guiderà all'esperienza di accesso in modalità dettagliata. I dati di telemetria mostrano che con questo approccio i clienti hanno accessi più riusciti. Offre inoltre maggiore flessibilità per incorporare diversi metodi di autenticazione, ad esempio l'autenticazione a fattore telefonico di US.

![newsignin](media/AD-FS-paginated-sign-in/signin2.png)

Nella prima pagina verrà richiesto di immettere il nome utente. È anche possibile selezionare l'opzione "Mantieni l'accesso" per ridurre la frequenza delle richieste di accesso e rimanere connessi quando è possibile farlo in modo sicuro. Questa opzione è disabilitata per impostazione predefinita.

![newsignin](media/AD-FS-paginated-sign-in/signin3.png)

Nella seconda pagina verranno visualizzate le opzioni di autenticazione configurate dall'amministratore. Se è abilitata l'autenticazione esterna come primaria, verrà inclusa anche questa.

![newsignin](media/AD-FS-paginated-sign-in/signin4.png)

Nella terza pagina verrà richiesto di immettere la password (presupponendo che sia stata selezionata l'opzione di autenticazione "password").

## <a name="how-to-get-the-new-experience"></a>Come ottenere la nuova esperienza

### <a name="new-installation-of-ad-fs"></a>Nuova installazione di AD FS
Se si è un nuovo cliente da AD FS, per impostazione predefinita si riceverà la nuova progettazione.

### <a name="upgrading-a-farm"></a>Aggiornamento di una farm
Se sei un cliente esistente AD FS 2012 R2 o 2016, ci sono due modi per ricevere la nuova progettazione dopo l'aggiornamento dei server AD FS 2019 e l'abilitazione dell'FBI a 2019.

- Consentire il nuovo accesso tramite PowerShell. Eseguire il comando seguente per abilitare la paginazione:``Set-AdfsGlobalAuthenticationPolicy -EnablePaginatedAuthenticationPages $true``

 - Abilitare l'autenticazione esterna come primaria, tramite PowerShell o tramite il AD FS Server Manager. Le nuove pagine di accesso impaginato verranno abilitate quando questa funzionalità è abilitata.
Se si è un nuovo cliente da AD FS, per impostazione predefinita si riceverà la nuova progettazione. Tuttavia, se si è un cliente esistente con AD FS 2012 R2 o 2016, è necessario eseguire diversi passaggi per ricevere la nuova progettazione:``Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true``

## <a name="customization"></a>Personalizzazione
Le opzioni per la personalizzazione saranno comunque valide per AD FS 2019.
Di seguito sono riportati alcuni collegamenti ad altri documenti per il riferimento.

• Per coloro che non hanno intenzione di aggiornare i server a AD FS 2019 ma vogliono comunque la nuova progettazione: [Uso di un tema Web Azure AD UX in Active Directory Federation Services](azure-ux-web-theme-in-ad-fs.md)

• Posizione centralizzata per la personalizzazione: [Personalizzazione dell'accesso utente ad AD FS](ad-fs-user-sign-in-customization.md)
