---
title: Metodi di autenticazione aggiuntivo in AD FS 2019
description: Questo documento descrive i nuovi metodi di autenticazione di AD FS 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 68cd67dc14d3407985579a49e2f8603634fafdb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824432"
---
# <a name="configure-3rd-party-authenticaiton-providers-as-primary-authentication-in-ad-fs-2019"></a>Configurare i provider di autenticazione di terze parti 3rd come l'autenticazione primaria in AD FS 2019

>Si applica a: Windows Server 2019

Le organizzazioni rilevano gli attacchi che tentano di forza bruta, danneggiare o bloccano in caso contrario, gli account utente inviando richieste di autenticazione basato su password.  Per proteggere dagli attacchi di organizzazioni, AD FS sono state introdotte funzionalità, ad esempio errori di blocco extranet "intelligente" e il blocco basati su indirizzi IP.  

Tuttavia, queste mitigazioni sono reattive.  Per fornire un modo proattivo, per ridurre la gravità di questi attacchi, AD FS offre la possibilità per la richiesta di fattori diverso dalle password prima di raccogliere la password.  

Ad esempio, AD FS 2016 introdotto Azure MFA come metodo di autenticazione primaria in modo che i codici di OTP dall'App Authenticator può essere usati come il primo fattore.
Compilazione a disegnarvi sopra, con AD FS 2019 è possibile configurare i provider di autenticazione esterni come fattori di autenticazione principale.

Esistono due scenari principali in questo modo:

## <a name="scenario-1-protect-the-password"></a>Scenario 1: proteggere la password
Proteggere account di accesso basato su password da attacchi di forza bruta e dei blocchi prima chiedere conferma per un fattore aggiuntivo, esterno.  Solo se è stata completata l'autenticazione esterna all'utente vedere un prompt della password.  In tal modo pratico, gli utenti malintenzionati hanno tentato di danneggiare o disabilitazione degli account.

Questo scenario è costituito da due componenti:
- Richiedere autenticazione a più fattori di Azure o un fattore di autenticazione esterni come l'autenticazione principale
- Nome utente e password come metodo di autenticazione aggiuntivo in AD FS

## <a name="scenario-2-password-free"></a>Scenario 2: privi di password.
Eliminare completamente le password ma completato con un nome sicuro, autenticazione a più fattori con password non interamente basato su metodi in AD FS
- Azure MFA con l'app Authenticator
- Windows 10 Hello for Business
- Autenticazione del certificato
- Provider di autenticazione esterni

## <a name="concepts"></a>Concetti
Che cosa **l'autenticazione principale** davvero significa è che è il metodo all'utente viene richiesto per primo, prima di altri fattori.  In precedenza i metodi primari solo disponibili in AD FS sono stati compilati nei metodi per Active Directory o Azure MFA o altro tipo di autenticazione LDAP archivi.  Metodi esterni può essere configurati come l'autenticazione "aggiuntiva", che viene eseguita dopo aver completato l'autenticazione principale.

In AD FS 2019, l'autenticazione esterna come funzionalità primarie significa che qualsiasi provider di autenticazione esterni registrati nella farm AD FS (usando Register-AdfsAuthenticationProvider) diventano disponibili per l'autenticazione principale, nonché "altri" autenticazione. Possono essere abilitate nello stesso modo i provider predefiniti, ad esempio l'autenticazione basata su form e l'autenticazione del certificato, per intranet e/o extranet utilizzare.

![autenticazione](media/Additional-Authentication-Methods-AD-FS/auth1.png)

Dopo aver abilitato un provider esterno per la rete intranet, extranet, o entrambi, diventa disponibile per gli utenti da usare.  Se più di un metodo è abilitato, gli utenti verranno visualizzata una pagina scelta e avere la possibilità di scegliere un metodo principale, come avviene per l'autenticazione aggiuntiva.

## <a name="pre-requisites"></a>Prerequisiti
Prima di configurare i provider di autenticazione esterni come primario, assicurarsi che i prerequisiti seguenti siano soddisfatti
- È stato innalzato il livello di comportamento farm di ADFS (FBL) a "4" (questo valore viene convertito in AD FS 2019)
    - Questo è il valore FBL predefinito per la nuova farm AD FS 2019
    - Per le farm di ADFS basate su Windows Server 2012 R2 o 2016, può essere generato il FBL con il commandlet di PowerShell AdfsFarmBehaviorLevelRaise Invoke.  Per informazioni dettagliate sull'aggiornamento di una farm AD FS, vedere la farm dell'aggiornamento di articolo per le farm SQL o WID farm 
    - È possibile controllare il valore FBL usando il cmdlet Get-AdfsFarmInformation
- La farm AD FS 2019 è configurata per usare il nuovo utente 'impaginati' 2019 pagine adiacenti
    - Questo è il comportamento predefinito per la nuova farm AD FS 2019
    - Per le farm AD FS aggiornate da Windows Server 2012 R2 o 2016, i flussi impaginati vengono abilitati automaticamente quando l'autenticazione esterna come primaria (la funzionalità descritta in questo documento) è abilitato come descritto di seguito.

## <a name="enable-external-authentication-methods-as-primary"></a>Abilitare i metodi di autenticazione esterno come database primario
Dopo avere verificato i prerequisiti, esistono due modi per configurare il provider di autenticazione aggiuntive di AD FS come primario:

### <a name="using-powershell"></a>Mediante PowerShell


```powershell
PS C:\> Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true
``` 


Il servizio AD FS deve essere riavviato dopo l'abilitazione o disabilitazione di autenticazione aggiuntivo come primario.

### <a name="using-the-ad-fs-management-console"></a>Utilizzando la console di gestione di ADFS
Nella console di gestione di ADFS, sotto **Service** -> **metodi di autenticazione**, in **metodi di autenticazione primaria**, fare clic su Modifica

Selezionare la casella di controllo **consentire ai provider di autenticazione aggiuntivo come primario**.

Il servizio AD FS deve essere riavviato dopo l'abilitazione o disabilitazione di autenticazione aggiuntivo come primario.

## <a name="enable-username-and-password-as-additional-authentication"></a>Abilita nome utente e password come metodo di autenticazione aggiuntiva
Per completare lo scenario "proteggere la password", abilitare il nome utente e la password come metodo di autenticazione aggiuntiva usando PowerShell o la console di gestione di ADFS
### <a name="using-powershell"></a>Mediante PowerShell



```powershell
PS C:\> $providers = (Get-AdfsGlobalAuthenticationPolicy).AdditionalAuthenticationProvider

PS C:\>$providers = $providers + "FormsAuthentication"

PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider $providers
``` 

### <a name="using-the-ad-fs-management-console"></a>Utilizzando la console di gestione di ADFS
Nella console di gestione di ADFS, sotto **assistenza** -> **metodi di autenticazione**, in **altri metodi di autenticazione**, fare clic su  **Modifica**

Selezionare la casella di controllo **autenticazione basata su form** per abilitare il nome utente e la password come metodo di autenticazione aggiuntivo.
