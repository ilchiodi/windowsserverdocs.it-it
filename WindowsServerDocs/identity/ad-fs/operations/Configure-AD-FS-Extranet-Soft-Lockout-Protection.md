---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurare la protezione di blocco Extranet di AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6612c05e664b50c5a50b10b712b91715cc85d230
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189878"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>Configurare la protezione di blocco Extranet di AD FS

In AD FS in Windows Server 2012 R2, abbiamo introdotto una funzionalità di sicurezza denominata blocco Extranet.  Con questa funzionalità, AD FS viene interrotto, l'account utente "non autorizzato" all'esterno di autenticazione per un periodo di tempo.  Ciò impedisce che gli account utente di essere bloccati in Active Directory.  Oltre a proteggere gli utenti da un blocco degli account di Active Directory, blocco della extranet di AD FS anche protegge contro gli attacchi di individuazione delle password, attacchi di forza bruta

> [!NOTE]
> Questa funzionalità funziona solo per i **scenario extranet** in cui l'autenticazione, le richieste sempre passare attraverso il Proxy applicazione Web e si applica solo ai **autenticazione nome utente e password**.

## <a name="advantages-of-extranet-lockout"></a>Vantaggi di blocco Extranet
Blocco Extranet offre i vantaggi chiave seguenti:
- Consente di proteggere gli account utente dalla **attacchi di forza bruta** in cui un utente malintenzionato tenta di indovinare la password dell'utente inviando in modo continuo le richieste di autenticazione. In questo caso, ADFS bloccheranno l'account utente non autorizzato per l'accesso extranet
- Consente di proteggere gli account utente dalla **blocco degli account dannoso** in cui un utente malintenzionato vuole bloccare un account utente inviando richieste di autenticazione con password errata. In questo caso, anche se l'account utente verrà bloccato da AD FS per l'accesso extranet, l'account utente effettivo in Active Directory non sia bloccato e l'utente può comunque accedere alle risorse aziendali all'interno dell'organizzazione. Questo è noto come un **blocco soft**.

## <a name="how-it-works"></a>Come funziona
Sono disponibili 3 impostazioni in AD FS che è necessario configurare per abilitare questa funzionalità: 
- **EnableExtranetLockout &lt;booleana&gt;**  impostare questo valore booleano True se si vuole abilitare il blocco della Extranet.
- **ExtranetLockoutThreshold &lt;Integer&gt;**  definisce il numero massimo di tentativi con password errate. Quando viene raggiunta la soglia, AD FS verrà immediatamente rifiuta le richieste dalla extranet senza effettuare alcun tentativo di contattare il controller di dominio per l'autenticazione, non di dominio se la password è valido o non valido, fino a quando non viene passata la finestra di osservazione extranet. Ciò significa che il valore di **badPwdCount** attributo di un account di Active Directory non aumenteranno anche se l'account è temporaneamente-bloccato.
- **ExtranetObservationWindow &lt;TimeSpan&gt;**  determina per quanto tempo l'utente account sarà soft-bloccato. AD FS inizierà a eseguire nuovamente l'autenticazione di nome utente e password quando viene passata la finestra. ADFS utilizza il badPasswordTime attributo di Active Directory come riferimento per determinare se la finestra di osservazione extranet ha superato o non. La finestra ha superato se corrente ora > badPasswordTime ExtranetObservationWindow +. 

> [!NOTE]
> Funzioni il blocco della extranet di AD FS in modo indipendente dai criteri di blocco di AD. Tuttavia, è consigliabile impostare il **ExtranetLockoutThreshold** valore del parametro su un valore che è inferiore alla soglia di blocco degli account AD. Se non si esegue questa operazione comporta in AD FS sia in grado di proteggere gli account da essere bloccati in Active Directory. 

Un esempio di abilitazione della funzionalità di blocco Extranet con 15 numero massimo di tentativi con password errate e alla durata del blocco di soft 30 minuti è come segue:

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

Queste impostazioni verranno applicate a tutti i domini in grado di autenticare il servizio AD FS. Il modo in cui il funzionamento è che quando ADFS riceve una richiesta di autenticazione, verrà accedere il Controller di dominio primario (PDC) tramite una chiamata LDAP ed eseguire una ricerca per il **badPwdCount** attributo per l'utente nel controller di dominio primario. Se AD FS rileva che il valore di **badPwdCount** > = impostazione ExtranetLockoutThreshold e non ha superato il periodo definito nella finestra di osservazione Extranet ancora, AD FS rifiuterà la richiesta immediatamente, il che significa indipendentemente da se il utente di immettere una password valida o non valida dalla extranet, l'account di accesso avrà esito negativo poiché AD FS non invia le credenziali di Active Directory. ADFS non mantiene uno stato in relazione alla **badPwdCount** o blocco dell'account utente. ADFS utilizza Active Directory per tutti gli stati di rilevamento. 

> [!warning]
> Quando è abilitato il blocco della Extranet di AD FS su Server 2012 R2 tutte le richieste di autenticazione tramite WAP vengono convalidate da AD FS nel controller di dominio primario. Quando il PDC non è disponibile, gli utenti sarà possibile eseguire l'autenticazione dalla extranet.

Server 2016 offre un parametro aggiuntivo che consente AD FS per il fallback a un altro controller di dominio quando il PDC non è disponibile:

- **ExtranetLockoutRequirePDC &lt;booleana&gt;**  : quando abilitato: il blocco della extranet richiede un controller di dominio primario (PDC). Quando disabilitata: il blocco della extranet eseguiranno il fallback a un altro controller di dominio nel caso in cui il PDC non è disponibile.

Per configurare il blocco extranet di AD FS nei Server 2016, è possibile utilizzare il comando Windows PowerShell seguente:

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>Utilizzo con il criterio di blocco di Active Directory
La funzionalità di blocco della Extranet di AD FS funziona in modo indipendente dai criteri di blocco di AD. Tuttavia, è necessario assicurarsi che le impostazioni per il blocco della Extranet è configurato correttamente, in modo che possa servire lo scopo di sicurezza con il criterio di blocco di AD.
Diamo un'occhiata ai criteri di blocco degli annunci prima. Sono disponibili tre impostazioni riguardanti il criterio di blocco di AD:
- **Soglia di blocco dell'account**: questa impostazione è simile all'impostazione ExtranetLockoutThreshold in AD FS. Determina il numero di tentativi di accesso che determinano un account utente venga bloccato. Per proteggere gli account utente da un attacco di blocco degli account dannoso, si desidera impostare il valore di ExtranetLockoutThreshold in AD FS &lt; il valore di soglia di blocco Account in Active Directory
- **Durata del blocco dell'account**: questa impostazione determina per quanto tempo un utente account è bloccato. Questa impostazione non importa molto in questa conversazione perché il blocco della Extranet deve sempre eseguite prima del blocco di AD si verifica se è configurato correttamente
- **Reimposta blocco Account dopo**: questa impostazione determina la quantità di tempo deve trascorrere dall'ultimo accesso esito negativo dell'utente prima **badPwdCount** viene reimpostato su 0. In ordine per funzionalità di blocco della Extranet di AD FS per l'uso con il criterio di blocco di AD, si desidera assicurarsi che il valore di ExtranetObservationWindow in AD FS &gt; il valore di Reimposta blocco Account dopo di AD. Gli esempi seguenti verranno spiegato il motivo.  

È possibile esaminare due esempi e vedere come **badPwdCount** cambiano nel tempo in base a diverse impostazioni e gli stati. Si supponga che in entrambi gli esempi **soglia di blocco Account** = 4 e **ExtranetLockoutThreshold** = 2. Il **rossa** freccia rappresenta il tentativo di password errata, il **verde** freccia rappresenta un tentativo di password valida. Nell'esempio #1 **ExtranetObservationWindow** &gt; **reimpostazione blocco Account dopo**. Nell'esempio #2, **ExtranetObservationWindow** &lt; **reimpostazione blocco Account dopo**. 

### <a name="example-1"></a>Esempio 1
![Esempio 1](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>Esempio 2
![Esempio 1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

Come può notare da quanto sopra, sono disponibili due condizioni quando **badPwdCount** verrà reimpostato su 0. Uno è quando è presente un accesso corretto. L'altro è al momento per reimpostare il contatore come definito in **Reimposta blocco Account dopo** impostazione. Quando **Reimposta blocco Account dopo** &lt; **ExtranetObservationWindow**, un account non dispone di alcun rischio di essere bloccati da AD. Tuttavia, se **Reimposta blocco Account dopo** &gt; **ExtranetObservationWindow**, è probabile che un account potrebbe essere bloccato da AD, ma in una "modalità ritardata". Potrebbe richiedere un po' di tempo per ottenere un account bloccato da AD a seconda della configurazione come AD FS consente solo un tentativo di password errata durante l'intervallo di osservazione finché **badPwdCount** raggiunge **soglia di blocco Account** .

Per altre informazioni, vedere [configurazione di blocco degli Account](https://blogs.technet.microsoft.com/secguide/2014/08/13/configuring-account-lockout/). 

## <a name="known-issues"></a>Problemi noti
Si verifica un problema noto in cui l'account utente di Active Directory non è possibile eseguire l'autenticazione con AD FS poiché il **badPwdCount** attributo non viene replicato al controller di dominio che sta eseguendo una query ad FS. Visualizzare [2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc) per altri dettagli. È possibile trovare tutti i QFE ADFS di Active Directory che sono state rilasciate finora [qui](../deployment/updates-for-active-directory-federation-services-ad-fs.md).

## <a name="key-points-to-remember"></a>Punti importanti da ricordare
- La funzionalità di blocco Extranet funziona solo per i **scenario extranet** in cui le richieste di autenticazione sempre passare attraverso il Proxy applicazione Web
- La funzionalità di blocco Extranet si applica solo ai **autenticazione nome utente e password**
- ADFS non tenere traccia qualsiasi della **badPwdCount** o gli utenti che sono soft-bloccato. ADFS utilizza Active Directory per tutti gli stati di rilevamento
- ADFS esegue una ricerca per il **badPwdCount** attributo tramite chiamata LDAP per l'utente nel controller di dominio primario per ogni tentativo di autenticazione  
- AD FS meno recente 2016 avrà esito negativo se non può accedere ai controller di dominio primario. AD FS 2016 ha introdotto miglioramenti che consentiranno di AD FS per eseguire il fallback per altri controller di dominio in caso di controller di dominio primario non è disponibile. 
- AD FS consentirà le richieste di autenticazione dalla rete extranet se badPwdCount < ExtranetLockoutThreshold 
- Se **badPwdCount** >= **ExtranetLockoutThreshold** AND **badPasswordTime** + **ExtranetObservationWindow** < Ora corrente, AD FS rifiuterà le richieste di autenticazione dalla extranet
- Per evitare il blocco degli account dannoso, è necessario assicurarsi **ExtranetLockoutThreshold** < **soglia di blocco Account** AND **ExtranetObservationWindow**  >  **Reimposta contatore blocchi Account**


## <a name="additional-references"></a>Altri riferimenti  
- [Le procedure consigliate per la protezione di Active Directory Federation Services](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [Delegare l'accesso ad AD FS tramite cmdlet di PowerShell agli utenti senza privilegi di amministratore](delegate-ad-fs-pshell-access.md)
- [Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
