---
title: Metodi di autenticazione aggiuntivi in AD FS 2019
description: Questo documento descrive i nuovi metodi di autenticazione in AD FS 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ff129f2b049d3e17e6b39d653cc3962eba75090b
ms.sourcegitcommit: 2cc251eb5bc3069bf09bc08e06c3478fcbe1f321
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/03/2020
ms.locfileid: "84333902"
---
# <a name="configure-3rd-party-authentication-providers-as-primary-authentication-in-ad-fs-2019"></a>Configurare i provider di autenticazione di terze parti come autenticazione primaria in AD FS 2019


Le organizzazioni stanno riscontrando attacchi che tentano di forzare la forza bruta, compromettere o bloccare gli account utente inviando richieste di autenticazione basate su password.  Per proteggere le organizzazioni dalla compromissione, AD FS ha introdotto funzionalità come il blocco "intelligente" della Extranet e il blocco basato su indirizzi IP.  

Tuttavia, queste attenuazioni sono reattive.  Per fornire un modo proattivo, per ridurre la gravità di questi attacchi, AD FS è in grado di richiedere i fattori non password prima di raccogliere la password.  

Ad esempio, in AD FS 2016 è stata introdotta l'autenticazione a più fattori di Azure come autenticazione primaria in modo che i codici OTP dall'app Authenticator possano essere usati come primo fattore.
Con AD FS 2019 è possibile configurare i provider di autenticazione esterni come fattori di autenticazione primari.

Esistono due scenari principali che consentono di:

## <a name="scenario-1-protect-the-password"></a>Scenario 1: proteggere la password
Proteggere l'accesso basato su password da attacchi di forza bruta e blocchi richiedendo prima un fattore esterno aggiuntivo.  Solo se l'autenticazione esterna viene completata correttamente, l'utente visualizza una richiesta di conferma della password.  In questo modo è possibile evitare che gli utenti malintenzionati stiano provando a compromettere o disabilitare gli account.

Questo scenario è costituito da due componenti:
- Richiesta di autenticazione a più fattori di Azure (disponibile in AD FS 2016 e versioni successive) o un fattore di autenticazione esterno come autenticazione primaria
- Nome utente e password come autenticazione aggiuntiva in AD FS

## <a name="scenario-2-password-free"></a>Scenario 2: senza password
Elimina completamente le password, ma completa una potente autenticazione a più fattori usando metodi interamente non basati su password in AD FS
- Autenticazione a più fattori di Azure con l'app Authenticator
- Windows 10 Hello for business
- Autenticazione del certificato
- Provider di autenticazione esterni

## <a name="concepts"></a>Concetti
L' **autenticazione primaria** significa realmente che è il metodo a cui l'utente viene richiesto per primo, prima di altri fattori.  In precedenza gli unici metodi principali disponibili in AD FS sono stati compilati in metodi per Active Directory o multi-factor authentication di Azure o altri archivi di autenticazione LDAP.  I metodi esterni potrebbero essere configurati come autenticazione "aggiuntiva", che viene eseguita dopo il completamento dell'autenticazione principale.

In AD FS 2019, l'autenticazione esterna come funzionalità primaria significa che tutti i provider di autenticazione esterni registrati nella farm di AD FS (tramite Register-AdfsAuthenticationProvider) diventano disponibili per l'autenticazione primaria, oltre che per l'autenticazione "aggiuntiva". Possono essere abilitate allo stesso modo dei provider predefiniti, ad esempio l'autenticazione basata su form e l'autenticazione del certificato, per l'uso di Intranet e/o Extranet.

![autenticazione](media/Additional-Authentication-Methods-AD-FS/auth1.png)

Quando un provider esterno è abilitato per Extranet, Intranet o entrambi, diventa disponibile per l'uso da parte degli utenti.  Se è abilitato più di un metodo, gli utenti visualizzeranno una pagina di scelta e saranno in grado di scegliere un metodo primario, proprio come per l'autenticazione aggiuntiva.

## <a name="pre-requisites"></a>Prerequisiti
Prima di configurare i provider di autenticazione esterni come primari, assicurarsi di disporre dei prerequisiti seguenti.
- Il livello di comportamento della farm AD FS (FBI) è stato elevato a' 4' (questo valore viene convertito in AD FS 2019)
    - Questo è il valore predefinito dell'FBI per le nuove farm AD FS 2019
    - Per AD FS farm basate su Windows Server 2012 R2 o 2016, è possibile generare l'FBI usando PowerShell cmdlet Invoke-AdfsFarmBehaviorLevelRaise.  Per ulteriori informazioni sull'aggiornamento di una farm di AD FS, vedere l'articolo relativo all'aggiornamento della farm per le farm SQL o WID 
    - È possibile controllare il valore dell'FBI usando il cmdlet Get-AdfsFarmInformation
- La farm AD FS 2019 è configurata in modo da utilizzare le nuove pagine rivolte all'utente 2019' impaginato '
    - Questo è il comportamento predefinito per le nuove farm AD FS 2019
    - Per AD FS Farm aggiornate da Windows Server 2012 R2 o 2016, i flussi impaginati vengono abilitati automaticamente quando l'autenticazione esterna come primaria (la funzionalità descritta in questo documento) è abilitata come descritto di seguito.

## <a name="enable-external-authentication-methods-as-primary"></a>Abilitare i metodi di autenticazione esterni come primari
Dopo aver verificato i prerequisiti, esistono due modi per configurare AD FS provider di autenticazione aggiuntivi come primari:

### <a name="using-powershell"></a>Uso di PowerShell


```powershell
PS C:\> Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true
``` 


Il servizio di AD FS deve essere riavviato dopo l'abilitazione o la disabilitazione dell'autenticazione aggiuntiva come primaria.

### <a name="using-the-ad-fs-management-console"></a>Utilizzo di AD FS Management Console
Nella console di gestione ad FS, in metodi di autenticazione del **servizio**  ->  **Authentication Methods**, in **metodi di autenticazione primari**, fare clic su modifica.

Fare clic sulla casella di controllo **Consenti provider di autenticazione aggiuntivi come primario**.

Il servizio di AD FS deve essere riavviato dopo l'abilitazione o la disabilitazione dell'autenticazione aggiuntiva come primaria.

## <a name="enable-username-and-password-as-additional-authentication"></a>Abilitare nome utente e password come autenticazione aggiuntiva
Per completare lo scenario "Proteggi password", abilitare nome utente e password come autenticazione aggiuntiva usando PowerShell o la console di gestione di AD FS
### <a name="using-powershell"></a>Uso di PowerShell



```powershell
PS C:\> $providers = (Get-AdfsGlobalAuthenticationPolicy).AdditionalAuthenticationProvider

PS C:\>$providers = $providers + "FormsAuthentication"

PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider $providers
``` 

### <a name="using-the-ad-fs-management-console"></a>Utilizzo di AD FS Management Console
Nella console di gestione ad FS, in metodi di autenticazione del **servizio**  ->  **Authentication Methods**, in **metodi di autenticazione aggiuntivi**, fare clic su **modifica** .

Fare clic sulla casella di controllo per **l'autenticazione basata su form** per abilitare nome utente e password come autenticazione aggiuntiva.
