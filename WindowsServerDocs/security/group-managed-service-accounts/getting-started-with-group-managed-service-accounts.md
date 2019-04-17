---
title: Guida introduttiva a gruppo di account dei servizi gestiti
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7130ad73-9688-4f64-aca1-46a9187a46cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: cd1e92f93701e5b430d1425fcf9a2f3590d1fe5d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="getting-started-with-group-managed-service-accounts"></a>Guida introduttiva a gruppo di account dei servizi gestiti

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016


Questa guida fornisce istruzioni dettagliate e informazioni di base per l'attivazione e l'utilizzo di account del servizio gestito del gruppo in Windows Server 2012.

**In questo documento**

-   [Prerequisiti](#BKMK_Prereqs)

-   [Introduzione](#BKMK_Intro)

-   [Distribuzione di una nuova server farm](#BKMK_DeployNewFarm)

-   [Aggiunta di host membri a una server farm esistente](#BKMK_AddMemberHosts)

-   [L'aggiornamento delle proprietà dell'Account del servizio gestito del gruppo](#BKMK_Update_gMSA)

-   [Rimozione di host membri da una server farm esistente](#BKMK_DecommMemberHosts)


> [!NOTE]
> Questo argomento include cmdlet di Windows PowerShell di esempio che è possibile utilizzare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="BKMK_Prereqs"></a>Prerequisiti
Vedere la sezione in questo argomento in [requisiti per l'account del servizio gestito del gruppo](#BKMK_gMSA_Req).

## <a name="BKMK_Intro"></a>Introduzione
Quando un computer client si connette a un servizio ospitato in una server farm utilizzando bilanciamento carico di rete o un altro metodo in cui tutti i server sembrano essere lo stesso servizio per il client, non sono utilizzati protocolli di autenticazione che supportano l'autenticazione reciproca come Kerberos a meno che non tutte le istanze dei servizi utilizzino la stessa entità. Questo significa che ogni servizio da utilizzare le stesse password/chiavi per dimostrare la propria identità.

> [!NOTE]
> Questi account non supportano i cluster di failover. Tuttavia, servizi eseguiti il servizio Cluster possono usare un gestito o un (autonomo) se sono un servizio di Windows, un pool di applicazioni, un'attività pianificata o supportano in modo nativo gestito o (autonomo).

I servizi hanno le entità seguenti, tra cui scegliere, ognuno con determinate limitazioni.

|Entità|Ambito|Servizi supportati|Gestione delle password|
|-------|-----|-----------|------------|
|Sistema di Account di Windows|Dominio|Limitato a un dominio appartenenti a un server|Gestite dal computer|
|Account computer senza sistema Windows|Dominio|Qualsiasi server aggiunto al dominio|Nessuno|
|Account virtuale|Locale|Limitato a un server|Gestite dal computer|
|Account del servizio gestito autonomo di Windows 7|Dominio|Limitato a un dominio appartenenti a un server|Gestite dal computer|
|Account utente|Dominio|Qualsiasi server aggiunto al dominio|Nessuno|
|Account del servizio gestito di gruppo|Dominio|Qualsiasi server aggiunto al dominio di Windows Server 2012|Gestisce il controller di dominio e recuperate dall'host|

Un account computer Windows o un Account del servizio gestito (sMSA) di autonomo di Windows 7 o account virtuali non possono essere condivisi tra più sistemi. Se si configura un account per i servizi nel server farm di condividere, è necessario scegliere un account utente o un account computer oltre a un sistema di Windows. In entrambi i casi, questi account non sono la funzionalità di gestione delle password singolo punto di controllo. Questo crea quindi un problema in ogni organizzazione deve creare una soluzione costosa per aggiornare le chiavi per il servizio in Active Directory e quindi distribuire le chiavi per tutte le istanze di tali servizi.

Con Windows Server 2012, servizi o gli amministratori del servizio non è necessario gestire la sincronizzazione delle password tra istanze dei servizi quando si utilizza group Managed Service Accounts (gMSA). Puoi eseguire il provisioning gMSA in Active Directory e quindi configurare il servizio che supporta l'account del servizio gestito. È possibile eseguire il provisioning di un account con il *-ADServiceAccount cmdlet che fanno parte del modulo di Active Directory. La configurazione dell'identità del servizio nell'host è supportata da:

-   Stesse API di (autonomo), in modo che i prodotti che supportano sMSA supporterà gestito

-   Servizi che usano Gestione controllo servizi per configurare l'identità di accesso

-   Servizi che usano il servizio di gestione per pool di applicazioni IIS per configurare l'identità

-   L'utilità di pianificazione di attività.

### <a name="BKMK_gMSA_Req"></a>Requisiti per l'account del servizio gestito del gruppo
Nella tabella seguente sono elencati i requisiti di sistema operativo per l'autenticazione Kerberos funzionare con i servizi utilizzando gestito. Dopo la tabella sono elencati i requisiti di Active Directory.

Un'architettura a 64 bit è necessario per eseguire i comandi di Windows PowerShell utilizzati per amministrare account del servizio gestito del gruppo.

**Requisiti del sistema operativo**

|Elemento|Requisito|Sistema operativo|
|------|--------|----------|
|Host applicazione client|Client Kerberos conforme a RFC|Almeno Windows XP|
|Controller del dominio dell'account utente|KDC conforme a RFC|Almeno Windows Server 2003|
|Host membri di servizi condivisi|| Windows Server 2012 |
|Controller del dominio dell'host membro|KDC conforme a RFC|Almeno Windows Server 2003|
|Controller del dominio dell'account gestito| Windows Server 2012 controller di dominio disponibili per l'host recuperare la password|Dominio con Windows Server 2012 che possono essere presenti sistemi precedenti a Windows Server 2012 |
|Host del servizio back-end|Server di applicazioni Kerberos conforme a RFC|Almeno Windows Server 2003|
|Controller del dominio dell'account del servizio back-end|KDC conforme a RFC|Almeno Windows Server 2003|
|Windows PowerShell per Active Directory|Windows PowerShell per Active Directory installato localmente in un computer che supporta un'architettura a 64 bit o in computer di gestione remota (ad esempio, tramite strumenti di amministrazione remota del Server)| Windows Server 2012 |

**Requisiti di servizi di dominio Active**

-   Schema di Active Directory nella foresta del dominio dell'account deve essere aggiornato a Windows Server 2012 per creare un account.

    È possibile aggiornare lo schema installando un controller di dominio che esegue Windows Server 2012 o eseguendo la versione di adprep.exe da un computer che esegue Windows Server 2012. Il valore dell'attributo oggetto versione per l'oggetto CN = Schema, CN = Configuration, DC = Contoso, DC = Com deve essere 52.

-   Nuovo account gestito il provisioning

-   Se si gestisce l'autorizzazione dell'host del servizio da utilizzare gestito dal gruppo, quindi il gruppo di sicurezza nuovo o esistente

-   Se la gestione di controllo di accesso di servizio per gruppo, il gruppo di sicurezza nuovo o esistente

-   Se la prima chiave radice master per Active Directory non è stata distribuita nel dominio o non è stata creata, è necessario crearla. Il risultato della creazione può essere verificato nel log operativo KdsSvc, ID evento 4004.

Per istruzioni sulla creazione di chiave, vedere [creare la chiave radice di KDS servizi di distribuzione di chiave](create-the-key-distribution-services-kds-root-key.md). Servizio distribuzione chiavi Microsoft (kdssvc.dll) la chiave radice per Active Directory.

**Ciclo di vita**

Il ciclo di vita di una server farm utilizzando la funzionalità gestito in genere include le attività seguenti:

-   Distribuzione di una nuova server farm

-   Aggiunta di host membri a una server farm esistente

-   Rimozione di host membri da una server farm esistente

-   Rimozione di una server farm esistente

-   Rimuovere un host membro compromesso da una server farm, se necessario.

## <a name="BKMK_DeployNewFarm"></a>Distribuzione di una nuova server farm
Quando si distribuisce una nuova server farm, l'amministratore del servizio dovrà stabilire:

-   Se il servizio supporta l'utilizzo di servizi gestiti di gruppo

-   Se il servizio richiede connessioni autenticate in ingresso o in uscita

-   I nomi degli account computer per gli host membri per il servizio utilizzando l'account

-   Il nome NetBIOS per il servizio

-   Il nome host DNS per il servizio

-   I nomi dell'entità servizio (SPN) per il servizio

-   Intervallo di modificare la password (valore predefinito è 30 giorni).

### <a name="BKMK_Step1"></a>Passaggio 1: Provisioning account del servizio gestito del gruppo
È possibile creare un account solo se lo schema della foresta è stato aggiornato a Windows Server 2012, la chiave radice master per Active Directory è stata distribuita e vi sia almeno Windows Server 2012 controller di dominio nel dominio in cui verrà creato l'account.

Appartenenza al gruppo **Domain Admins**, **Account Operators** o possibilità di creare oggetti msDS-GroupManagedServiceAccount, è il requisito minimo necessario per completare le procedure seguenti.

#### <a name="BKMK_CreateGMSA"></a>Per creare un account con il cmdlet New-ADServiceAccount

1.  Nel controller di dominio di Windows Server 2012, eseguire Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi di Windows PowerShell, digitare i comandi seguenti e quindi premere INVIO. (Il modulo Active Directory verrà caricato automaticamente.)

    **New-ADServiceAccount [-Name] <string> - DNSHostName <string> [-KerberosEncryptionType <ADKerberosEncryptionType>] [-ManagedPasswordIntervalInDays < Nullable [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >] - SamAccountName <string> gli attributi - SPN < string [] >**

    |Parametro|Stringa|Esempio|
    |-------|-----|------|
    |Nome|Nome dell'account|ITFarm1|
    |DNSHostName|Nome host DNS del servizio|ITFarm1.contoso.com|
    |KerberosEncryptionType|Tutti i tipi di crittografia supportati dai server host|RC4, AES128 E AES256|
    |ManagedPasswordIntervalInDays|Intervallo di modifica delle password in giorni (impostazione predefinita è 30 giorni se non specificata)|90|
    |PrincipalsAllowedToRetrieveManagedPassword|Gli account computer appartengono gli host o al gruppo di sicurezza che appartengono gli host sono un membro di|ITFarmHosts|
    |SamAccountName|Nome NetBIOS per il servizio se non uguale al nome|ITFarm1|
    |Attributi SPN|Nomi dell'entità servizio (SPN) per il servizio|http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, ITFarm1/http/contoso|

    > [!IMPORTANT]
    > L'intervallo di modifica della password può essere impostata solo durante la creazione. Se è necessario modificare l'intervallo, è necessario creare un nuovo account e impostarlo al momento della creazione.

    **Esempio**

    Immettere il comando su una singola riga, anche se le parole potrebbero tornare a capo su più righe a causa dei limiti di formattazione.

    ```
    New-ADServiceAccount ITFarm1 -DNSHostName ITFarm1.contoso.com -PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts -KerberosEncryptionType RC4, AES128, AES256 -ServicePrincipalNames http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso

    ```

Appartenenza al gruppo **Domain Admins**, **Account Operators**, o possibilità di creare oggetti msDS-GroupManagedServiceAccount, è il requisito minimo necessario per completare questa procedura. For detailed information about using the appropriate accounts and group memberships, see [Local and Domain Default Groups](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

##### <a name="to-create-a-gmsa-for-outbound-authentication-only-using-the-new-adserviceaccount-cmdlet"></a>Per creare un account per l'autenticazione in uscita solo utilizzando il cmdlet New-ADServiceAccount

1.  Nel controller di dominio di Windows Server 2012, eseguire Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi per il modulo Active Directory per Windows PowerShell, digitare i comandi seguenti e quindi premere INVIO:

    **New-ADServiceAccount [-Name] <string> - RestrictToOutboundAuthenticationOnly [-ManagedPasswordIntervalInDays < Nullable [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >]**

    |Parametro|Stringa|Esempio|
    |-------|-----|------|
    |Nome|Nome dell'account|ITFarm1|
    |ManagedPasswordIntervalInDays|Intervallo di modifica delle password in giorni (impostazione predefinita è 30 giorni se non specificata)|75|
    |PrincipalsAllowedToRetrieveManagedPassword|Gli account computer appartengono gli host o al gruppo di sicurezza che appartengono gli host sono un membro di|ITFarmHosts|

    > [!IMPORTANT]
    > L'intervallo di modifica della password può essere impostata solo durante la creazione. Se è necessario modificare l'intervallo, è necessario creare un nuovo account e impostarlo al momento della creazione.

**Esempio**

```
New-ADServiceAccount ITFarm1 -RestrictToOutboundAuthenticationOnly - PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts

```

### <a name="BKMK_ConfigureServiceIdentity"></a>Passaggio 2: Configurazione del servizio identità applicazione servizio
Per configurare i servizi in Windows Server 2012, vedere la documentazione seguente:

-   Pool di applicazioni IIS

    Per ulteriori informazioni, vedere [specificare un'identità per un Pool di applicazioni (IIS 7)](https://technet.microsoft.com/library/cc771170(WS.10).aspx).

-   Servizi di Windows

    Per ulteriori informazioni, vedere [servizi](https://technet.microsoft.com/library/cc772408.aspx).

-   Attività

    Per ulteriori informazioni, vedere il [panoramica dell'utilità di pianificazione di attività](https://technet.microsoft.com/library/cc721871.aspx).

Altri servizi possono supportare gestito. Vedere la documentazione di prodotto appropriate per informazioni dettagliate su come configurare tali servizi.

## <a name="BKMK_AddMemberHosts"></a>Aggiunta di host membri a una server farm esistente
Se si usano gruppi di sicurezza per la gestione di host membri, aggiungere l'account computer per il nuovo host membro al gruppo di sicurezza (che gli host membri del gestito sono un membro di) utilizzando uno dei metodi seguenti.

Appartenenza al gruppo **Domain Admins**, o la possibilità di aggiungere membri all'oggetto gruppo di sicurezza, è il requisito minimo necessario per completare queste procedure.

-   Metodo 1: Gli utenti di Active Directory e computer

    Per le procedure su come utilizzare questo metodo, vedere [aggiungere un account computer a un gruppo](https://technet.microsoft.com/library/cc733097.aspx) tramite l'interfaccia di Windows e [gestire domini diversi in Centro di amministrazione di Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Metodo 2: dsmod

    Per le procedure su come utilizzare questo metodo, vedere [aggiungere un account computer a un gruppo](https://technet.microsoft.com/library/cc733097.aspx) utilizzando la riga di comando.

-   Metodo 3: Cmdlet di Active Directory per Windows PowerShell Add-ADPrincipalGroupMembership

    Per le procedure su come utilizzare questo metodo, vedere [Add-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617203.aspx).

Se si usa l'account computer, trovare gli account esistenti e quindi aggiungere il nuovo account computer.

Appartenenza al gruppo **Domain Admins**, **Account Operators**, o possibilità di gestire oggetti msDS-GroupManagedServiceAccount, è il requisito minimo necessario per completare questa procedura. Per informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze ai gruppi, vedere dominio gruppi predefiniti locali e.

#### <a name="to-add-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Per aggiungere host membri con il cmdlet Set-ADServiceAccount

1.  Nel controller di dominio di Windows Server 2012, eseguire Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi per il modulo Active Directory per Windows PowerShell, digitare i comandi seguenti e quindi premere INVIO:

    **Get-ADServiceAccount [-Name] <string> - PrincipalsAllowedToRetrieveManagedPassword**

3.  Al prompt dei comandi per il modulo Active Directory per Windows PowerShell, digitare i comandi seguenti e quindi premere INVIO:

    **Set-ADServiceAccount [-Name] <string> - PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >**

|Parametro|Stringa|Esempio|
|-------|-----|------|
|Nome|Nome dell'account|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|Gli account computer appartengono gli host o al gruppo di sicurezza che appartengono gli host sono un membro di|Host1, Host2, Host3|

**Esempio**

Ad esempio, per aggiungere membri host digitare i comandi seguenti e quindi premere INVIO.

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1-PrincipalsAllowedToRetrieveManagedPassword Host1 Host2 Host3

```

## <a name="BKMK_Update_gMSA"></a>Aggiornamento delle proprietà di Account del servizio gestito del gruppo
Appartenenza al gruppo **Domain Admins**, **Account Operators**, o la possibilità di scrivere oggetti msDS-GroupManagedServiceAccount, è il requisito minimo necessario per completare queste procedure.

Aprire il Active Directory modulo per Windows PowerShell e impostare le proprietà con il cmdlet Set-ADServiceAccount.

Per altre informazioni su come impostare queste proprietà, vedere [Set-ADServiceAccount](https://technet.microsoft.com/library/ee617252.aspx) nella libreria TechNet o digitare **Get-Help Set-ADServiceAccount** al modulo di Active Directory per Windows PowerShell comando prompt dei comandi e premere INVIO.

## <a name="BKMK_DecommMemberHosts"></a>Rimozione di host membri da una server farm esistente
Appartenenza al gruppo **Domain Admins**, o possibilità di rimuovere membri dall'oggetto gruppo di sicurezza, è il requisito minimo necessario per completare queste procedure.

### <a name="step-1-remove-member-host-from-gmsa"></a>Passaggio 1: Rimuovere host membri dall'account
Se si usano gruppi di sicurezza per la gestione di host membri, rimuovere l'account computer per l'host membro rimossi dal gruppo di sicurezza che gli host membri dell'account sono membri di utilizzando uno dei metodi seguenti.

-   Metodo 1: Gli utenti di Active Directory e computer

    Per le procedure su come utilizzare questo metodo, vedere [eliminare un Account Computer](https://technet.microsoft.com/library/cc754624.aspx) tramite l'interfaccia di Windows e [gestire domini diversi in Centro di amministrazione di Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Metodo 2: drsm

    Per le procedure su come utilizzare questo metodo, vedere [eliminare un Account Computer](https://technet.microsoft.com/library/cc754624.aspx) utilizzando la riga di comando.

-   Metodo 3: Il cmdlet Remove-ADPrincipalGroupMembership Active Directory per Windows PowerShell

    Per altre informazioni su come eseguire questa operazione, vedere [Remove-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617243.aspx) nella libreria TechNet o digitare **Get-Help Remove-ADPrincipalGroupMembership** al modulo di Active Directory per Windows PowerShell comando prompt dei comandi e premere INVIO.

Se l'elenco degli account computer, recuperare gli account esistenti e aggiungerli tutti tranne l'account computer rimosso.

Appartenenza al gruppo **Domain Admins**, **Account Operators**, o possibilità di gestire oggetti msDS-GroupManagedServiceAccount, è il requisito minimo necessario per completare questa procedura. Per informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze ai gruppi, vedere dominio gruppi predefiniti locali e.

##### <a name="to-remove-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Per rimuovere host membri con il cmdlet Set-ADServiceAccount

1.  Nel controller di dominio di Windows Server 2012, eseguire Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi per il modulo Active Directory per Windows PowerShell, digitare i comandi seguenti e quindi premere INVIO:

    **Get-ADServiceAccount [-Name] <string> - PrincipalsAllowedToRetrieveManagedPassword**

3.  Al prompt dei comandi per il modulo Active Directory per Windows PowerShell, digitare i comandi seguenti e quindi premere INVIO:

    **Set-ADServiceAccount [-Name] <string> - PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >**

|Parametro|Stringa|Esempio|
|-------|-----|------|
|Nome|Nome dell'account|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|Gli account computer appartengono gli host o al gruppo di sicurezza che appartengono gli host sono un membro di|Host1, Host3|

**Esempio**

Ad esempio, per rimuovere membro host digitare i comandi seguenti e quindi premere INVIO.

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1 Host3

```

### <a name="BKMK_RemoveGMSA"></a>Passaggio 2: Rimozione di un Account del servizio gestito di gruppo dal sistema
Rimuovere le credenziali memorizzate nella cache gestito dall'host membro mediante Uninstall-ADServiceAccount o l'API NetRemoveServiceAccount nel sistema host.

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare queste procedure.

##### <a name="to-remove-a-gmsa-using-the-uninstall-adserviceaccount-cmdlet"></a>Per rimuovere un gestito utilizzando il cmdlet Uninstall-ADServiceAccount

1.  Nel controller di dominio di Windows Server 2012, eseguire Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi per il modulo Active Directory per Windows PowerShell, digitare i comandi seguenti e quindi premere INVIO:

    **< ADServiceAccount > Uninstall-ADServiceAccount**

    **Esempio**

    Ad esempio, per rimuovere le credenziali memorizzate nella cache per un account denominato ITFarm1 digitare il comando seguente e quindi premere INVIO:

    ```
    Uninstall-ADServiceAccount ITFarm1
    ```

Per ulteriori informazioni sul cmdlet Uninstall-ADServiceAccount, del modulo di Active Directory per Windows PowerShell prompt dei comandi, digitare **Get-Help Uninstall-ADServiceAccount**e quindi premere INVIO o vedere la pagina web TechNet dedicata al [Uninstall-ADServiceAccount](https://technet.microsoft.com/library/ee617202.aspx).



## <a name="BKMK_Links"></a>Vedere anche

-   [Panoramica degli account del servizio gestito del gruppo](group-managed-service-accounts-overview.md)



