---
title: Guida introduttiva alla funzionalità Account del servizio gestiti del gruppo
description: Sicurezza di Windows Server
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
ms.openlocfilehash: 3d07f137aa40b26b4f4fd69c050415b82608ed7e
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546362"
---
# <a name="getting-started-with-group-managed-service-accounts"></a>Guida introduttiva alla funzionalità Account del servizio gestiti del gruppo

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016


In questa guida vengono fornite istruzioni dettagliate e informazioni complementari per l'abilitazione e l'utilizzo di account del servizio gestiti del gruppo in Windows Server 2012.

**Contenuto del documento**

-   [Prerequisiti](#BKMK_Prereqs)

-   [Introduzione](#BKMK_Intro)

-   [Distribuzione di un nuovo server farm](#BKMK_DeployNewFarm)

-   [Aggiunta di host membri a un server farm esistente](#BKMK_AddMemberHosts)

-   [Aggiornamento delle proprietà dell'account del servizio gestito del gruppo](#BKMK_Update_gMSA)

-   [Rimozione di host membri da un server farm esistente](#BKMK_DecommMemberHosts)


> [!NOTE]
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="BKMK_Prereqs"></a>Prerequisiti
Vedere la sezione in questo argomento [Requisiti per gli account del servizio gestiti del gruppo](#BKMK_gMSA_Req).

## <a name="BKMK_Intro"></a>Introduzione
Quando un computer client si connette a un servizio ospitato in una server farm che usa il bilanciamento carico di rete (NBL) o un altro metodo in base al quale tutti i server sembrano essere lo stesso servizio per il client, non è possibile usare i protocolli di autenticazione che supportano l'autenticazione reciproca come Kerberos a meno che tutte le istanze dei servizi utilizzino la stessa entità. Di conseguenza, ogni servizio deve usare le stesse password/chiavi per dimostrare la propria identità.

> [!NOTE]
> I cluster di failover non supportano gli account del servizio gestiti del gruppo. Tuttavia, i servizi eseguiti sul servizio cluster possono usare un account del servizio gestito del gruppo o un account del servizi gestito (autonomo) se si tratta di un servizio Windows, di un pool di applicazioni, di un'attività pianificata o se supportano gli account del servizio gestito del gruppo o gli account del servizio gestiti (autonomi) a livello nativo.

I servizi consentono di scegliere tra le entità seguenti, ognuna delle quali presenta determinate limitazioni.

|Entità|`Scope`|Servizi supportati|Gestione delle password|
|-------|-----|-----------|------------|
|Account computer del sistema Windows|Domain|Limitato a un server aggiunto al dominio|Gestite dal computer|
|Account computer senza sistema Windows|Domain|Qualsiasi server aggiunto al dominio|Nessuna|
|Account virtuale|Locale|Limitato a un server|Gestite dal computer|
|Account del servizio gestito (autonomo) Windows 7|Domain|Limitato a un server aggiunto al dominio|Gestite dal computer|
|Account utente|Domain|Qualsiasi server aggiunto al dominio|Nessuna|
|Account del servizio gestito del gruppo|Domain|Qualsiasi server aggiunto al dominio di Windows Server 2012|Gestite dal controller di dominio e recuperate dall'host|

Non è possibile condividere tra più sistemi un account computer Windows o un account del servizio gestito autonomo (sMSA) Windows 7 o account virtuali. Se si configura un account per i servizi da condividere in server farm, è necessario scegliere un account utente o un account computer diverso da quello del sistema Windows. In entrambi i casi, questi account non dispongono della funzionalità di gestione delle password come singolo punto di controllo. Si crea quindi un problema perché ogni organizzazione deve elaborare una soluzione costosa per aggiornare le chiavi del servizio in Active Directory e distribuirle a tutte le istanze di tali servizi.

Con Windows Server 2012, i servizi o gli amministratori del servizio non devono gestire la sincronizzazione delle password tra le istanze del servizio quando si usano gli account del servizio gestito del gruppo (gMSA). Dopo avere eseguito il provisioning degli account del servizio gestiti del gruppo in Active Directory, si configura il servizio che supporta gli account del servizio gestiti del gruppo. È possibile effettuare il provisioning di un account del servizio gestito del gruppo con i cmdlet *-ADServiceAccount che fanno parte del modulo di Active Directory. La configurazione dell'identità del servizio nell'host è supportata da:

-   Le stesse API dell'account del servizio gestito (autonomo), in modo tale che i prodotti che supportano gli account del servizio gestiti (autonomo) supportino anche gli account del servizio gestiti del gruppo

-   Servizi che usano Gestione controllo servizi per configurare l'identità di accesso

-   Servizi che usano Gestione IIS per la configurazione dell'identità da parte di pool di applicazioni

-   Attività che usano l'Utilità di pianificazione.

### <a name="BKMK_gMSA_Req"></a>Requisiti per gli account del servizio gestiti del gruppo
Nella tabella seguente sono indicati i requisiti del sistema operativo per l'uso dell'autenticazione Kerberos con servizi che usano account del servizio gestiti del gruppo. I requisiti di Active Directory sono riportati in calce alla tabella.

Per eseguire i comandi di Windows PowerShell utilizzati per amministrare account del servizio gestiti del gruppo, è richiesta un'architettura a 64 bit.

**Requisiti del sistema operativo**

|Elemento|Requisito|Sistema operativo|
|------|--------|----------|
|Host dell'applicazione client|Client Kerberos conforme a RFC|Almeno Windows XP|
|Controller di dominio dell'account utente|KDC conforme a RFC|Almeno Windows Server 2003|
|Host membri del servizio condiviso|| Windows Server 2012 |
|Controller di dominio dell'host membro|KDC conforme a RFC|Almeno Windows Server 2003|
|Controller di dominio dell'account gMSA| Controller di dominio Windows Server 2012 disponibili per l'host per recuperare la password|Dominio con Windows Server 2012 che può avere alcuni sistemi precedenti a Windows Server 2012 |
|Host del servizio back-end|Server applicazioni Kerberos conforme a RFC|Almeno Windows Server 2003|
|Controller di dominio dell'account del servizio back-end|KDC conforme a RFC|Almeno Windows Server 2003|
|Windows PowerShell per Active Directory|Windows PowerShell per Active Directory installato localmente in un computer che supporta un'architettura a 64 bit o nel computer di gestione remota (ad esempio con gli strumenti di amministrazione remota del server).| Windows Server 2012 |

**Requisiti del servizio Dominio di Active Directory**

-   Per creare un gMSA, è necessario aggiornare lo schema Active Directory nella foresta del dominio gMSA a Windows Server 2012.

    È possibile aggiornare lo schema installando un controller di dominio che esegue Windows Server 2012 o eseguendo la versione di Adprep. exe da un computer che esegue Windows Server 2012. Il valore dell'attributo della versione dell'oggetto per l'oggetto CN=Schema,CN=Configuration,DC=Contoso,DC=Com deve essere 52.

-   Provisioning del nuovo account del servizio gestito del gruppo

-   Se si gestisce l'autorizzazione dell'host del servizio all'uso di account del servizio gestiti del gruppo per gruppo, un gruppo di sicurezza nuovo o esistente

-   Se si gestisce il controllo di accesso al servizio per gruppo, un gruppo di sicurezza nuovo o esistente

-   Se la prima chiave radice master per Active Directory non è distribuita nel dominio o non è stata creata, è necessario crearla. Il risultato della creazione può essere verificato nel log operativo KdsSvc, ID evento 4004.

Per istruzioni su come creare la chiave, vedere [creare la chiave radice KDS di servizi di distribuzione chiavi](create-the-key-distribution-services-kds-root-key.md). La chiave radice per Active Directory con il Servizio distribuzione chiavi Microsoft (kdssvc.dll).

**Vita**

In genere, il ciclo di vita di una server farm in cui è usata la funzionalità Account del servizio gestito del gruppo prevede le attività seguenti:

-   Distribuzione di una nuova server farm

-   Aggiunta di host membri a una server farm esistente

-   Rimozione di host membri da una server farm esistente

-   Rimozione di una server farm esistente

-   Rimozione di un host membro compromesso da una server farm, se necessario.

## <a name="BKMK_DeployNewFarm"></a>Distribuzione di un nuovo server farm
Nella distribuzione di una nuova server farm, l'amministratore del servizio dovrà stabilire:

-   Se il servizio supporta l'uso di account del servizio gestiti del gruppo

-   Se il servizio richiede connessioni autenticate in ingresso o in uscita

-   I nomi degli account computer per gli host membri del servizio che usa gli account del servizio gestiti del gruppo

-   Il nome NetBIOS del servizio

-   Il nome host DNS del servizio

-   I nomi dell'entità servizio (SPN) per il servizio

-   L'intervallo di modifica della password (per impostazione predefinita è di 30 giorni).

### <a name="BKMK_Step1"></a>Passaggio 1: Provisioning degli account del servizio gestiti del gruppo
È possibile creare un gMSA solo se lo schema della foresta è stato aggiornato a Windows Server 2012, la chiave radice master per Active Directory è stata distribuita ed è presente almeno un controller di dominio Windows Server 2012 nel dominio in cui verrà creato il gMSA.

Per eseguire le procedure seguenti, è necessaria almeno l'appartenenza ai gruppi **Domain Admins** o **Account Operators** o la capacità di creare oggetti msDS-GroupManagedServiceAccount.

#### <a name="BKMK_CreateGMSA"></a>Per creare un gMSA con il cmdlet New-ADServiceAccount

1.  Sul controller di dominio Windows Server 2012, eseguire Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi di Windows PowerShell, digitare i comandi seguenti e quindi premere INVIO. (Il modulo Active Directory verrà caricato automaticamente.)

    **New-ADServiceAccount [-name] <string> -dNSHostName <string> [-KerberosEncryptionType <ADKerberosEncryptionType>] [-ManagedPasswordIntervalInDays < Nullable [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >]-sAMAccountName <string> -ServicePrincipalNames < stringa [] >**

    |Parametro|String|Esempio|
    |-------|-----|------|
    |Name|Nome dell'account|ITFarm1|
    |DNSHostName|Nome host DNS del servizio|ITFarm1.contoso.com|
    |KerberosEncryptionType|Qualsiasi tipo di crittografia supportata dai server host|RC4, AES128, AES256|
    |ManagedPasswordIntervalInDays|Intervallo di modifica della password espresso in giorni (se non è indicato, l'intervallo predefinito è di 30 giorni)|90|
    |PrincipalsAllowedToRetrieveManagedPassword|Gli account computer degli host membri o il gruppo di sicurezza a cui appartengono gli host membri|ITFarmHosts|
    |SamAccountName|Nome NetBIOS del servizio se diverso dal Nome|ITFarm1|
    |ServicePrincipalNames|Nomi dell'entità servizio (SPN) per il servizio|http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso|

    > [!IMPORTANT]
    > È possibile impostare l'intervallo di modifica della password solo in fase di creazione. Per modificare l'intervallo, è necessario creare un nuovo account del servizio gestito del gruppo e impostarlo al momento della creazione.

    **Esempio**

    Immettere il comando su una singola riga, anche se le parole potrebbero tornare automaticamente a capo e quindi apparire su più righe a causa dei limiti di formattazione.

    ```
    New-ADServiceAccount ITFarm1 -DNSHostName ITFarm1.contoso.com -PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts -KerberosEncryptionType RC4, AES128, AES256 -ServicePrincipalNames http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso

    ```

Per eseguire questa procedura, è necessaria almeno l'appartenenza ai gruppi **Domain Admins** o **Account Operators** o la possibilità di creare oggetti msDS-GroupManagedServiceAccount. Per altre informazioni sull'uso degli account appropriati e sull'appartenenza a gruppi, vedere [Gruppi predefiniti locali e di dominio](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

##### <a name="to-create-a-gmsa-for-outbound-authentication-only-using-the-new-adserviceaccount-cmdlet"></a>Per creare un account del servizio gestito del gruppo per l'autenticazione in uscita usando solo il cmdlet New-ADServiceAccount

1.  Sul controller di dominio Windows Server 2012, eseguire Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi del modulo Active Directory per Windows PowerShell, digitare i comandi seguenti, quindi premere INVIO:

    **New-ADServiceAccount [-name] <string> -RestrictToOutboundAuthenticationOnly [-ManagedPasswordIntervalInDays < Nullable [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >]**

    |Parametro|String|Esempio|
    |-------|-----|------|
    |Name|Nome dell'account|ITFarm1|
    |ManagedPasswordIntervalInDays|Intervallo di modifica della password espresso in giorni (se non è indicato, l'intervallo predefinito è di 30 giorni)|75|
    |PrincipalsAllowedToRetrieveManagedPassword|Gli account computer degli host membri o il gruppo di sicurezza a cui appartengono gli host membri|ITFarmHosts|

    > [!IMPORTANT]
    > È possibile impostare l'intervallo di modifica della password solo in fase di creazione. Per modificare l'intervallo, è necessario creare un nuovo account del servizio gestito del gruppo e impostarlo al momento della creazione.

**Esempio**

```
New-ADServiceAccount ITFarm1 -RestrictToOutboundAuthenticationOnly - PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts

```

### <a name="BKMK_ConfigureServiceIdentity"></a>Passaggio 2: Configurazione del servizio Identità applicazione del servizio
Per configurare i servizi in Windows Server 2012, vedere la documentazione della funzionalità seguente:

-   Pool di applicazioni IIS

    Per altre informazioni, vedere [Specificare l'identità del pool di applicazioni usato dai siti portale (IIS 7)](https://technet.microsoft.com/library/cc771170(WS.10).aspx).

-   Servizi Windows

    Per altre informazioni, vedere [Servizi](https://technet.microsoft.com/library/cc772408.aspx).

-   Attività

    Per altre informazioni, vedere [Panoramica dell'Utilità di pianificazione](https://technet.microsoft.com/library/cc721871.aspx).

L'account del servizio gestito del gruppo potrebbe essere supportato da altri servizi. Per altre informazioni sulle modalità di configurazione di tali servizi, vedere la relativa documentazione di prodotto.

## <a name="BKMK_AddMemberHosts"></a>Aggiunta di host membri a un server farm esistente
Se si utilizzano gruppi di sicurezza per la gestione di host membri, aggiungere l'account computer per il nuovo host membro al gruppo di sicurezza (che gli host membri di gMSA sono membri di) utilizzando uno dei metodi seguenti.

Per eseguire queste procedure, è necessaria almeno l'appartenenza al gruppo **Domain Admins**o la possibilità di aggiungere membri all'oggetto gruppo di sicurezza.

-   Metodo 1: Utenti e computer di Active Directory

    Per le procedure sull'uso di questo metodo, vedere [Aggiungere un account computer a un gruppo](https://technet.microsoft.com/library/cc733097.aspx) usando l'interfaccia di Windows e [Gestire domini diversi nel Centro di amministrazione di Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Metodo 2: dsmod

    Per le procedure su come usare questo metodo, vedere [Aggiungere un account computer a un gruppo](https://technet.microsoft.com/library/cc733097.aspx) usando la riga di comando.

-   Metodo 3: cmdlet Add-ADPrincipalGroupMembership di Active Directory per Windows PowerShell

    Per le procedure su come usare questo metodo, vedere [Add-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617203.aspx).

Se si usano account computer, trovare gli account esistenti e aggiungere il nuovo account computer.

Per eseguire questa procedura, è necessaria almeno l'appartenenza ai gruppi **Domain Admins**o **Account Operators**o la possibilità di gestire oggetti msDS-GroupManagedServiceAccount. Per altre informazioni sull'uso degli account appropriati e sull'appartenenza a gruppi, vedere Gruppi predefiniti locali e di dominio.

#### <a name="to-add-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Per aggiungere host membri con il cmdlet Set-ADServiceAccount

1.  Sul controller di dominio Windows Server 2012, eseguire Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi del modulo Active Directory per Windows PowerShell, digitare i comandi seguenti, quindi premere INVIO:

    **Get-ADServiceAccount [-name] <string> -PrincipalsAllowedToRetrieveManagedPassword**

3.  Al prompt dei comandi del modulo Active Directory per Windows PowerShell, digitare i comandi seguenti, quindi premere INVIO:

    **Set-ADServiceAccount [-name] <string> -PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >**

|Parametro|String|Esempio|
|-------|-----|------|
|Name|Nome dell'account|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|Gli account computer degli host membri o il gruppo di sicurezza a cui appartengono gli host membri|Host1, Host2, Host3|

**Esempio**

Per aggiungere host membri, digitare i comandi seguenti e premere INVIO.

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1,Host2,Host3

```

## <a name="BKMK_Update_gMSA"></a>Aggiornamento delle proprietà dell'account del servizio gestito del gruppo
Per eseguire queste procedure, è necessaria almeno l'appartenenza ai gruppi **Domain Admins**o **Account Operators**o la possibilità di scrivere oggetti msDS-GroupManagedServiceAccount.

Aprire il modulo Active Directory per Windows PowerShell e impostare le proprietà con il cmdlet Set-ADServiceAccount.

Per altre informazioni su come impostare queste proprietà, vedere [Set-ADServiceAccount](https://technet.microsoft.com/library/ee617252.aspx) nella libreria TechNet Library o digitare **Get-Help Set-ADServiceAccount** al prompt dei comandi del modulo Active Directory per Windows PowerShell e premere INVIO.

## <a name="BKMK_DecommMemberHosts"></a>Rimozione di host membri da un server farm esistente
Per eseguire queste procedure, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o la possibilità di rimuovere membri dall'oggetto gruppo di sicurezza.

### <a name="step-1-remove-member-host-from-gmsa"></a>Passaggio 1: Rimuovere host membri dall'account del servizio gestito del gruppo
Se si utilizzano gruppi di sicurezza per la gestione di host membri, rimuovere l'account computer per l'host membro ritirato dal gruppo di sicurezza a cui appartengono gli host membri di gMSA utilizzando uno dei metodi seguenti.

-   Metodo 1: Utenti e computer di Active Directory

    Per le procedure sull'uso di questo metodo, vedere [Eliminare un account computer](https://technet.microsoft.com/library/cc754624.aspx) usando l'interfaccia di Windows e [Gestire domini diversi nel Centro di amministrazione di Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Metodo 2: drsm

    Per le procedure su come usare questo metodo, vedere [Eliminare un account computer](https://technet.microsoft.com/library/cc754624.aspx) usando la riga di comando.

-   Metodo 3: cmdlet Remove-ADPrincipalGroupMembership di Active Directory per Windows PowerShell

    Per altre informazioni, vedere  [Remove-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617243.aspx) nella libreria TechNet o digitare **Get-Help Remove-ADPrincipalGroupMembership** al prompt dei comandi del modulo Active Directory per Windows PowerShell e premere INVIO.

Nel caso di elenchi di account computer, recuperare gli account esistenti e aggiungerli tutti tranne l'account computer rimosso.

Per eseguire questa procedura, è necessaria almeno l'appartenenza ai gruppi **Domain Admins**o **Account Operators**o la possibilità di gestire oggetti msDS-GroupManagedServiceAccount. Per altre informazioni sull'uso degli account appropriati e sull'appartenenza a gruppi, vedere Gruppi predefiniti locali e di dominio.

##### <a name="to-remove-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Per rimuovere host membri con il cmdlet Set-ADServiceAccount

1.  Sul controller di dominio Windows Server 2012, eseguire Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi del modulo Active Directory per Windows PowerShell, digitare i comandi seguenti, quindi premere INVIO:

    **Get-ADServiceAccount [-name] <string> -PrincipalsAllowedToRetrieveManagedPassword**

3.  Al prompt dei comandi del modulo Active Directory per Windows PowerShell, digitare i comandi seguenti, quindi premere INVIO:

    **Set-ADServiceAccount [-name] <string> -PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >**

|Parametro|String|Esempio|
|-------|-----|------|
|NOME|Nome dell'account|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|Gli account computer degli host membri o il gruppo di sicurezza a cui appartengono gli host membri|Host1, Host3|

**Esempio**

Per rimuovere host membri, digitare i comandi seguenti e premere INVIO.

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1,Host3

```

### <a name="BKMK_RemoveGMSA"></a>Passaggio 2: Rimozione di un account del servizio gestito del gruppo dal sistema
Rimuovere dall'host membro le credenziali dell'account del servizio gestito del gruppo memorizzate nella cache usando il cmdlet Uninstall-ADServiceAccount o l'API NetRemoveServiceAccount nel sistema host.

Per completare queste procedure è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.

##### <a name="to-remove-a-gmsa-using-the-uninstall-adserviceaccount-cmdlet"></a>Per rimuovere un account del servizio gestito del gruppo con il cmdlet Uninstall-ADServiceAccount

1.  Sul controller di dominio Windows Server 2012, eseguire Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi del modulo Active Directory per Windows PowerShell, digitare i comandi seguenti, quindi premere INVIO:

    **Uninstall-ADServiceAccount < ADServiceAccount >**

    **Esempio**

    Per rimuovere le credenziali memorizzate nella cache di un account del servizio gestito del gruppo denominato ITFarm1, digitare ad esempio il comando seguente e quindi premere INVIO:

    ```
    Uninstall-ADServiceAccount ITFarm1
    ```

Per altre informazioni sul cmdlet Uninstall-ADServiceAccount, al prompt dei comandi del modulo Active Directory per Windows PowerShell, digitare **Get-Help Uninstall-ADServiceAccount**e quindi premere INVIO o vedere la pagina del sito Web TechNet dedicata al cmdlet [Uninstall-ADServiceAccount](https://technet.microsoft.com/library/ee617202.aspx).



## <a name="BKMK_Links"></a>Vedere anche

-   [Panoramica degli account del servizio gestiti del gruppo](group-managed-service-accounts-overview.md)



