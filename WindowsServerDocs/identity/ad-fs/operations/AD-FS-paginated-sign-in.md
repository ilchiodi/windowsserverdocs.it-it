---
title: AD FS impaginato Accedi
description: Questo documento descrive la nuova esperienza Accedi per AD FS 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 946e99448d13bf6782c10bce5a0b8566da4deb17
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190381"
---
# <a name="ad-fs-paginated-sign-in"></a>AD FS impaginato Accedi


Per AD FS 2019, abbiamo riprogettato l'interfaccia utente di accesso.  A questo punto, sign-in ADFS avrà lo stesso aspetto e comportamento di Azure Active Directory.  Questo fornirà agli utenti un'esperienza più coerente Accedi, che incorpora un flusso utente impaginati e centrato. 

## <a name="whats-changing"></a>Cosa cambierà
In AD FS 2012 R2 e 2016, la schermata di accesso era simile al seguente:

![oldsignin](media/AD-FS-paginated-sign-in/signin1.png)

Passiamo ora dalla visualizzazione di un singolo modulo che si trova sul lato destro dello schermo.

In AD FS 2019, queste sono le modifiche di progettazione principali che verranno visualizzati:


- **Oggetto centrato UI**. Esisteva in precedenza, l'interfaccia utente di accesso sul lato destro dello schermo, come illustrato in precedenza. È stato spostato il protagonista dell'interfaccia utente per modernizzare l'esperienza.
- **Pagination**. Anziché fornire un modulo long per la compilazione, è stato inserito un nuovo flusso che illustra in dettaglio l'esperienza di accesso dettagliata. I dati di telemetria indicano che con questo approccio, i clienti hanno maggiori possibilità di successo accessi. Fornisce inoltre a una maggiore flessibilità per incorporare i vari metodi di autenticazione, ad esempio l'autenticazione di phone factor. 

![newsignin](media/AD-FS-paginated-sign-in/signin2.png)

Nella prima pagina, verrà chiesto di immettere il nome utente. È inoltre possibile selezionare l'opzione "Mantieni l'accesso" per ridurre la frequenza delle richieste di accesso e restare connessi quando è consigliabile eseguire questa operazione. (Questa opzione è disabilitata per impostazione predefinita).

![newsignin](media/AD-FS-paginated-sign-in/signin3.png)

Nella seconda pagina, vengono fornite con le opzioni di autenticazione, configurate dall'amministratore. Se consentire l'autenticazione esterna come database primario è abilitata, questa sarà inclusa anche.

![newsignin](media/AD-FS-paginated-sign-in/signin4.png)

Nella terza pagina, verrà chiesto di immettere la password (presupponendo che si seleziona "Password" come opzione di autenticazione). 

## <a name="how-to-get-the-new-experience"></a>Come ottenere la nuova esperienza
Se sei un nuovo cliente di AD FS, si riceverà la nuova progettazione per impostazione predefinita. Tuttavia, se sei un cliente esistente con AD FS 2012 R2 o 2016, esistono alcuni passaggi che è necessario eseguire per ricevere la nuova progettazione: 

1. Aggiornare i server ad AD FS 2019. 
2.  Abilitare il FBL a 2019.
3.  Abilitare la nuova esperienza di accesso.
- Consenti il nuovo accesso aggiuntivo tramite PowerShell. In PowerShell, eseguire il comando seguente per abilitare il paging: ``Set-AdfsGlobalAuthenticationPolicy -EnablePaginatedAuthenticationPages $true``
- Consentire l'autenticazione esterna come primario, tramite PowerShell o tramite Server Manager AD FS. In PowerShell, eseguire il comando seguente per consentire l'autenticazione esterna come primario: ``Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true``

## <a name="customization"></a>Personalizzazione
Le opzioni per la personalizzazione continuerà a essere applicabile per AD FS 2019. Di seguito sono riportati alcuni collegamenti ad altri documenti per riferimento. 

• Per coloro che non si intende eseguire l'aggiornamento dei server ad AD FS 2019 ma vuole comunque che la nuova progettazione: [Utilizzo di un tema Web dell'esperienza utente di Azure AD in Active Directory Federation Services](azure-ux-web-theme-in-ad-fs.md)

• Una posizione centrale per la personalizzazione: [Personalizzazione dell'accesso utente ad AD FS](ad-fs-user-sign-in-customization.md)
