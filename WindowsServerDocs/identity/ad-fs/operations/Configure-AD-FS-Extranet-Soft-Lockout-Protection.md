---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurare la protezione del blocco Extranet AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bb5958f8205271fe3ab2258ed9812ae03f2a0be0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358215"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>Configurare la protezione del blocco Extranet AD FS

In AD FS in Windows Server 2012 R2 è stata introdotta una funzionalità di sicurezza denominata blocco Extranet.  Con questa funzionalità, AD FS "interrompe" l'autenticazione dell'account utente "dannoso" dall'esterno per un certo periodo di tempo.  In questo modo si impedisce che gli account utente vengano bloccati in Active Directory.  Oltre a proteggere gli utenti da un blocco degli account di AD, AD FS blocco Extranet protegge anche dagli attacchi di forza bruta per la password.

> [!NOTE]
> Questa funzionalità funziona solo per lo **scenario Extranet** in cui le richieste di autenticazione passano attraverso il proxy dell'applicazione Web e si applica solo all' **autenticazione con nome utente e password**.

## <a name="advantages-of-extranet-lockout"></a>Vantaggi del blocco Extranet
Il blocco Extranet offre i vantaggi principali seguenti:
- Protegge gli account utente da **attacchi di forza bruta** in cui un utente malintenzionato tenta di indovinare la password di un utente inviando continuamente richieste di autenticazione. In questo caso, AD FS bloccherà l'account utente malintenzionato per l'accesso Extranet
- Protegge gli account utente dal **blocco degli account dannosi** , in cui un utente malintenzionato desidera bloccare un account utente inviando richieste di autenticazione con password errate. In questo caso, anche se l'account utente viene bloccato da AD FS per l'accesso Extranet, l'account utente effettivo in Active Directory non è bloccato e l'utente può comunque accedere alle risorse aziendali all'interno dell'organizzazione. Questa operazione è nota come **blocco soft**.

## <a name="how-it-works"></a>Come funziona
AD FS è necessario configurare per abilitare questa funzionalità, sono disponibili tre impostazioni: 
- **EnableExtranetLockout &lt;Boolean @ no__t-2** impostare questo valore booleano su true se si vuole abilitare il blocco Extranet.
- **ExtranetLockoutThreshold &lt;Integer @ no__t-2** definisce il numero massimo di tentativi di accesso con password errata. Una volta raggiunta la soglia, AD FS rifiuterà immediatamente le richieste dalla rete Extranet senza tentare di contattare il controller di dominio per l'autenticazione, indipendentemente dal fatto che la password sia valida o negativa, fino a quando non viene passata la finestra di osservazione Extranet. Ciò significa che il valore dell'attributo **badPwdCount** di un account di Active Directory non aumenterà mentre l'account è temporaneamente bloccato.
- **ExtranetObservationWindow &lt;TimeSpan @ no__t-2** determina per quanto tempo l'account utente verrà bloccato temporaneamente. AD FS inizierà a eseguire di nuovo l'autenticazione con nome utente e password quando viene passata la finestra. AD FS utilizza l'attributo AD badPasswordTime come riferimento per determinare se la finestra di osservazione Extranet è stata superata o meno. La finestra è stata passata se l'ora corrente > badPasswordTime + ExtranetObservationWindow. 

> [!NOTE]
> AD FS funzioni di blocco Extranet indipendentemente dai criteri di blocco AD. È tuttavia consigliabile impostare il valore del parametro **ExtranetLockoutThreshold** su un valore inferiore alla soglia di blocco dell'account ad. In caso contrario, il AD FS non sarà in grado di proteggere gli account dal blocco del Active Directory. 

Di seguito è riportato un esempio di abilitazione della funzionalità di blocco Extranet con un massimo di 15 tentativi di accesso con password errata e durata del blocco temporaneo di 30 minuti:

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

Queste impostazioni verranno applicate a tutti i domini che possono essere autenticati dal servizio AD FS. Il modo in cui funziona è che, quando AD FS riceve una richiesta di autenticazione, accederà al controller di dominio primario (PDC) tramite una chiamata LDAP ed eseguirà una ricerca dell'attributo **badPwdCount** per l'utente nel PDC. Se AD FS trova il valore dell'impostazione **badPwdCount** > = ExtranetLockoutThreshold e l'ora definita nella finestra di osservazione Extranet non è ancora stata superata, ad FS rifiuterà immediatamente la richiesta, il che significa che, indipendentemente dal fatto che l'utente entri in uno stato positivo o negativo password dalla Extranet, l'accesso avrà esito negativo perché AD FS non invia le credenziali ad Active Directory. AD FS non mantiene alcuno stato per quanto riguarda gli account utente **badPwdCount** o bloccati. AD FS USA AD per il rilevamento dello stato. 

> [!warning]
> Quando AD FS blocco Extranet sul Server 2012 R2 è abilitato, tutte le richieste di autenticazione tramite WAP vengono convalidate da AD FS sul PDC. Quando il PDC non è disponibile, gli utenti non saranno in grado di eseguire l'autenticazione dalla rete Extranet.

Il server 2016 offre un parametro aggiuntivo che consente AD FS di eseguire il fallback a un altro controller di dominio quando il PDC non è disponibile:

- **ExtranetLockoutRequirePDC &lt;Boolean @ no__t-2** -se abilitata: il blocco Extranet richiede un controller di dominio primario (PDC). Quando è disabilitato, il blocco Extranet verrà fallback a un altro controller di dominio nel caso in cui il PDC non sia disponibile.

È possibile utilizzare il comando di Windows PowerShell seguente per configurare il blocco Extranet AD FS sul server 2016:

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>Uso dei criteri di blocco Active Directory
La funzionalità di blocco Extranet in AD FS funziona indipendentemente dai criteri di blocco AD. Tuttavia, è necessario assicurarsi che le impostazioni per il blocco Extranet siano configurate correttamente, in modo da poter gestire lo scopo di sicurezza con i criteri di blocco AD.
Esaminare prima di tutto i criteri di blocco AD. Sono disponibili tre impostazioni relative ai criteri di blocco in Active Directory:
- **Soglia di blocco dell'account**: questa impostazione è simile all'impostazione ExtranetLockoutThreshold in ad FS. Determina il numero di tentativi di accesso non riusciti che comporteranno il blocco di un account utente. Per proteggere gli account utente da un attacco di blocco degli account dannosi, è necessario impostare il valore di ExtranetLockoutThreshold in AD FS &lt; il valore soglia di blocco dell'account in AD
- **Durata blocco account**: questa impostazione determina per quanto tempo un account utente viene bloccato. Questa impostazione non è rilevante in questa conversazione perché il blocco Extranet deve sempre verificarsi prima che il blocco di Active Directory venga eseguito se configurato correttamente
- **Reimposta contatore blocco account dopo**: questa impostazione determina la quantità di tempo che deve trascorrere dall'ultimo errore di accesso dell'utente prima che **badPwdCount** venga reimpostato su 0. Affinché la funzionalità di blocco Extranet in AD FS funzioni correttamente con i criteri di blocco di Active Directory, è necessario assicurarsi che il valore di ExtranetObservationWindow in AD FS &gt; il contatore Reimposta blocco account dopo valore in AD. Negli esempi seguenti viene illustrato il motivo.  

Verranno ora esaminati due esempi e verrà illustrato il modo in cui le modifiche apportate a **badPwdCount** nel tempo in base a impostazioni e Stati diversi. Si supponga che in entrambi gli esempi **soglia di blocco account** = 4 e **ExtranetLockoutThreshold** = 2. La freccia **rossa** rappresenta un tentativo di password non valida. la freccia **verde** rappresenta un tentativo di password valido. In #1, ad esempio, **ExtranetObservationWindow** &gt; **Reimposta il contatore blocco account dopo**. In #2, ad esempio, **ExtranetObservationWindow** &lt; **Reimposta il contatore blocco account dopo**. 

### <a name="example-1"></a>Esempio 1
![Example1](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>Esempio 2
![Example1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

Come si può notare, esistono due condizioni quando **badPwdCount** verrà reimpostato su 0. Uno è quando si verifica un accesso riuscito. L'altro è il momento in cui è necessario reimpostare il contatore come definito in **Reimposta contatore blocco account dopo** l'impostazione. Quando si **Reimposta il contatore blocco account dopo** &lt; **ExtranetObservationWindow**, un account non ha alcun rischio di essere bloccato da ad. Tuttavia, se **Reimposta il contatore del blocco dell'account dopo** &gt; **ExtranetObservationWindow**, è possibile che un account venga bloccato da ad ma in una "modalità ritardata". Potrebbe essere necessario un po' di tempo per ottenere un account bloccato da AD a seconda della configurazione AD FS consentirà un solo tentativo di accesso con password errata durante la finestra di osservazione finché **badPwdCount** raggiunge la **soglia di blocco dell'account**.

Per ulteriori informazioni, vedere [configurazione del blocco degli account](https://blogs.technet.microsoft.com/secguide/2014/08/13/configuring-account-lockout/). 

## <a name="known-issues"></a>Problemi noti
Si è verificato un problema noto in cui l'account utente di Active Directory non è in grado di eseguire l'autenticazione con AD FS perché l'attributo **badPwdCount** non viene replicato nel controller di dominio su cui è in esecuzione la query in ADFS. Per ulteriori informazioni, vedere [2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc) . È possibile trovare tutte AD FS QFE che sono state rilasciate finora [qui](../deployment/updates-for-active-directory-federation-services-ad-fs.md).

## <a name="key-points-to-remember"></a>Punti chiave da ricordare
- La funzionalità di blocco Extranet funziona solo per lo **scenario Extranet** in cui le richieste di autenticazione passano attraverso il proxy dell'applicazione Web
- La funzionalità di blocco Extranet si applica solo al **nome utente & autenticazione della password**
- AD FS non tiene traccia di **badPwdCount** o di utenti che sono bloccati temporaneamente. AD FS USA AD per il rilevamento dello stato
- AD FS esegue una ricerca dell'attributo **badPwdCount** tramite una chiamata LDAP per l'utente nel PDC per ogni tentativo di autenticazione  
- AD FS più vecchi di 2016 avranno esito negativo se non è possibile accedere al PDC. In AD FS 2016 sono stati introdotti miglioramenti che consentono AD FS di eseguire il fallback ad altri controller di dominio nel caso in cui il PDC non sia disponibile. 
- AD FS consentirà le richieste di autenticazione dalla rete Extranet se badPwdCount < ExtranetLockoutThreshold 
- Se **badPwdCount** >= **ExtranetLockoutThreshold** e **badPasswordTime** + **ExtranetObservationWindow** < ora corrente, ad FS rifiuterà le richieste di autenticazione dalla rete Extranet
- Per evitare il blocco degli account dannosi, è necessario assicurarsi che **ExtranetLockoutThreshold** < **soglia di blocco account** e **ExtranetObservationWindow** > **reimpostare il contatore blocco account**


## <a name="additional-references"></a>Altri riferimenti  
- [Procedure consigliate per la protezione di Active Directory Federation Services](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [Delegare l'accesso ad AD FS tramite cmdlet di PowerShell agli utenti senza privilegi di amministratore](delegate-ad-fs-pshell-access.md)
- [Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
