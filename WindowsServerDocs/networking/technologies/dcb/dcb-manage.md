---
title: Gestire Data Center Bridging (DCB)
description: In questo argomento vengono fornite istruzioni su come usare i comandi di Windows PowerShell per gestire Data Center Bridging in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: daed746fe798ae253956d0977827d0e205bb8b3e
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034571"
---
# <a name="manage-data-center-bridging-dcb"></a>Gestire Data Center Bridging (DCB)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento vengono fornite istruzioni su come usare i comandi di Windows PowerShell per configurare il Data Center Bridging \(DCB\) in un Data Center Bridging\-scheda di rete compatibile che viene installato in un computer su cui è in esecuzione Windows Server 2016 o Windows 10.

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Installare i Data Center Bridging in Windows Server 2016 o Windows 10

Per informazioni sui prerequisiti per l'utilizzo e come installare i Data Center Bridging, vedere [installare Data Center Bridging (DCB) in Windows Server 2016 o Windows 10](dcb-install.md).


## <a name="dcb-configurations"></a>Configurazioni di Data Center Bridging 

Prima di Windows Server 2016, tutte le configurazioni di DCB è stato applicato universalmente per tutte le schede di rete supportate DCB. 

In Windows Server 2016, è possibile applicare le configurazioni di DCB per Store i criteri globali o per singoli criteri Store\(s\). Quando vengono applicati criteri singoli queste sostituiscono tutte le impostazioni dei criteri globali.

Le configurazioni di assegnazione di priorità di classe, di base alla priorità e di applicazione il traffico a livello di sistema non viene applicato alle schede di rete fino a quando non procedere nel modo seguente.

1. Attivare il bit disposti DCBX su false

2. Abilitare DCB sulle schede di rete. Visualizzare [abilitare e visualizzare le impostazioni di DCB sulle schede di rete](#bkmk_enabledcb).

>[!NOTE]
>Se si vuole configurare DCB da un commutatore tramite DCBX, vedere [impostazioni DCBX](#dcb-configuration-on-network-adapters).

Il bit disposti DCBX è descritto nella specifica del Data Center Bridging. Se il bit disposto in un dispositivo è impostato su true, il dispositivo è disposto ad accettare le configurazioni da un dispositivo remoto tramite DCBX. Se il bit disposto in un dispositivo è impostato su false, il dispositivo verrà rifiutare tutti i tentativi di configurazione da dispositivi remoti e applicare solo le configurazioni locali.

Se DCB non è installato in Windows Server 2016 il valore del bit disposti è irrilevante per quanto riguarda il sistema operativo è perché il sistema operativo non ha nessuna impostazione locale si applicano alle schede di rete. Dopo aver installato Data Center Bridging, il valore predefinito della proprietà disposti bit è true. Ne consegue che le schede di rete mantenere tutte le configurazioni ricevute dai relativi peer remoto.

Se una scheda di rete non supporta DCBX, mai riceverà le configurazioni da un dispositivo remoto. Le configurazioni vengono ricevuti dal sistema operativo, ma solo dopo il DCBX disposti bit è impostato su false.

## <a name="set-the-willing-bit"></a>Impostare il bit disposto

Per applicare le configurazioni del sistema operativo di classe di traffico, base alla priorità e l'assegnazione di priorità dell'applicazione nelle schede di rete, o semplicemente sostituire le configurazioni da dispositivi remoti \, ovvero se sono presenti eventuali \, è possibile eseguire il comando seguente.

>[!NOTE]
>I nomi dei comandi di PowerShell di Windows DCB includono "Da QoS" anziché "DCB" nella stringa del nome. Infatti, QoS e Data Center Bridging sono integrati in Windows Server 2016 per offrire un'esperienza di gestione QoS lineare.

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

Per visualizzare lo stato del bit disposti impostazione, è possibile usare il comando seguente:

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>Configurazione di DCB sulle schede di rete

Abilitare Data Center Bridging in una scheda di rete consente di visualizzare la configurazione propagata da un commutatore alla scheda di rete.

Le configurazioni di DCB includono i passaggi seguenti.

1.  Configurare le impostazioni di Data Center Bridging a livello di sistema, che include:

    a. Gestione della classe di traffico
    
    b. Impostazioni di priorità del flusso di controllo (base alla priorità)
    
    c. Assegnazione di priorità dell'applicazione
    
    d. Impostazioni DCBX

2. Configurare Data Center Bridging nella scheda di rete.



##  <a name="dcb-traffic-class-management"></a>Gestione della classe di traffico Data Center Bridging

Di seguito sono i comandi di Windows PowerShell di esempio per la gestione della classe di traffico.

### <a name="create-a-traffic-class"></a>Creare una classe di traffico

È possibile usare la **New-NetQosTrafficClass** comando per creare una classe di traffico.

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

Per impostazione predefinita, tutti i valori 802.1P vengono eseguito il mapping a una classe di traffico predefinito, con il 100% della larghezza di banda del collegamento fisico. Il **New-NetQosTrafficClass** comando crea una nuova classe di traffico, in cui qualsiasi pacchetto che è contrassegnato con priorità 802.1P valore 4 viene eseguito il mapping. L'algoritmo di selezione di trasmissione \(autorità di riconoscimento timestamp\) è ETS e ha 30% della larghezza di banda.

È possibile creare fino a 7 nuove classi di traffico. Tra cui la classe di traffico predefinita, possono esserci al massimo 8 classi di traffico nel sistema. Tuttavia, una scheda di rete che supportano DCB potrebbe non supportare che molte classi nell'hardware del traffico. Se si creano altre classi di traffico che può essere organizzato in una scheda di rete e abilitare DCB sulle che la scheda di rete, il driver miniport segnala un errore al sistema operativo. L'errore viene registrato nel registro eventi.

La somma di prenotazioni di larghezza di banda per tutte le classi di traffico creata non può superare 99% della larghezza di banda. La classe di traffico predefinita ha sempre almeno 1% della larghezza di banda sono riservato per se stesso.

### <a name="display-traffic-classes"></a>Visualizzare le classi di traffico

È possibile usare la **Get-NetQosTrafficClass** comando per visualizzare le classi di traffico.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>Modificare una classe di traffico

È possibile usare la **Set-NetQosTrafficClass** comando per creare una classe di traffico. 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

È quindi possibile usare la **Get-NetQosTrafficClass** comando per visualizzare le impostazioni.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

Dopo aver creato una classe di traffico, è possibile modificare le impostazioni in modo indipendente. Le impostazioni che è possibile modificare includono:

1. Allocazione della larghezza di banda \(- BandwidthPercentage\)

2. TSA (\-Algorithm\)

3. Mapping di priorità \(-priorità\)

### <a name="remove-a-traffic-class"></a>Rimuovere una classe di traffico

È possibile usare la **Remove-NetQosTrafficClass** comando per eliminare una classe di traffico.

>[!IMPORTANT]
>Non è possibile rimuovere la classe predefinita del traffico.


    Remove-NetQosTrafficClass -Name SMB

È quindi possibile usare la **Get-NetQosTrafficClass** comando per visualizzare le impostazioni.
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

Dopo aver rimosso una classe di traffico, il valore 802.1P mappato a che viene modificato il mapping di classe di traffico per la classe predefinita del traffico. Larghezza di banda che è stato riservato per una classe di traffico viene restituito per l'allocazione della classe di traffico predefinito quando viene rimossa la classe di traffico.

## <a name="per-network-interface-policies"></a>I criteri di interfaccia rete

Tutti gli esempi precedenti impostare i criteri globali. Di seguito sono esempi di come è possibile impostare e ottenere i criteri per ogni interfaccia di rete. 

Il campo "PolicySet" cambia da globale a AdapterSpecific. Quando vengono visualizzati i criteri AdapterSpecific, l'indice dell'interfaccia \(ifIndex\) e il nome dell'interfaccia \(ifAlias\) vengono anche visualizzati.

```
PS C:\> Get-NetQosTrafficClass

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       100          0-7              Global


PS C:\> Get-NetQosTrafficClass -InterfaceAlias M1

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       100          0-7              AdapterSpecific  4       M1


PS C:\> New-NetQosTrafficClass -Name SMBGlobal -BandwidthPercentage 30 -Priority 4 -Algorithm ETS

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
SMBGlobal   ETS       30           4                Global


PS C:\> New-NetQosTrafficClass -Name SMBforM1 -BandwidthPercentage 30 -Priority 4 -Algorithm ETS -Interfac


Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
SMBforM1    ETS       30           4                AdapterSpecific  4       M1


PS C:\> Get-NetQosTrafficClass

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       70           0-3,5-7          Global
SMBGlobal   ETS       30           4                Global


PS C:\> Get-NetQosTrafficClass -InterfaceAlias M1

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       70           0-3,5-7          AdapterSpecific  4       M1
SMBforM1    ETS       30           4                AdapterSpecific  4       M1


```

## <a name="priority-flow-control-settings"></a>Impostazioni di controllo di flusso di priorità:

Di seguito sono esempi di comandi per le impostazioni di controllo di flusso di priorità. Queste impostazioni possono essere specificate anche per singoli adapter.

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>Abilitazione e visualizzazione con priorità flusso di controllo per globali e specifiche di interfaccia di casi d'uso

```
PS C:\> Enable-NetQosFlowControl -Priority 4
PS C:\> Enable-NetQosFlowControl -Priority 3 -InterfaceAlias M1
PS C:\> Get-NetQosFlowControl

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      Global
1          False      Global
2          False      Global
3          False      Global
4          True       Global
5          False      Global
6          False      Global
7          False      Global

PS C:\> Get-NetQosFlowControl -InterfaceAlias M1

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      AdapterSpecific  4       M1
1          False      AdapterSpecific  4       M1
2          False      AdapterSpecific  4       M1
3          True       AdapterSpecific  4       M1
4          False      AdapterSpecific  4       M1
5          False      AdapterSpecific  4       M1
6          False      AdapterSpecific  4       M1
7          False      AdapterSpecific  4       M1  

```


### <a name="disable-priority-flow-control-global-and-interface-specific"></a>Disabilitare il controllo di flusso prioritario (globali e specifiche dell'interfaccia)

```
PS C:\> Disable-NetQosFlowControl -Priority 4
PS C:\> Disable-NetQosFlowControl -Priority 3 -InterfaceAlias m1
PS C:\> Get-NetQosFlowControl

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      Global
1          False      Global
2          False      Global
3          False      Global
4          False      Global
5          False      Global
6          False      Global
7          False      Global


PS C:\> Get-NetQosFlowControl -InterfaceAlias M1

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      AdapterSpecific  4       M1
1          False      AdapterSpecific  4       M1
2          False      AdapterSpecific  4       M1
3          False      AdapterSpecific  4       M1
4          False      AdapterSpecific  4       M1
5          False      AdapterSpecific  4       M1
6          False      AdapterSpecific  4       M1
7          False      AdapterSpecific  4       M1  

```

##  <a name="application-priority-assignment"></a>Assegnazione di priorità dell'applicazione

Di seguito sono esempi di assegnazione di priorità.

### <a name="create-qos-policy"></a>Crea criteri QoS

```
PS C:\> New-NetQosPolicy -Name "SMB Policy" -PriorityValue8021Action 4

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
PriorityValue  : 4

```

Il comando precedente crea un nuovo criterio per SMB. -SMB è un filtro della posta in arrivo che corrisponde alla porta TCP 445 (riservato per SMB). Se viene inviato un pacchetto sulla porta TCP 445 dal sistema operativo con valore 802.1P 4 prima che venga contrassegnato il pacchetto venga passato a un driver miniport di rete.

Oltre a: SMB, altri filtri predefiniti includono: iSCSI (porta TCP corrisponda 3260), - NFS (porta TCP corrisponda 2049), - LiveMigration (corrispondenza la porta TCP 6600), - FCOE (corrispondenza EtherType 0x8906) e – NetworkDirect.

NetworkDirect è un livello astratto che viene creato nella parte superiore di qualsiasi implementazione RDMA su una scheda di rete. – NetworkDirect deve essere seguita da una porta di rete diretti.

Oltre ai filtri predefiniti, è possibile classificare il traffico dal nome dell'eseguibile dell'applicazione (come illustrato nel primo esempio riportato di seguito) o dall'indirizzo IP, porta o protocollo (come illustrato nel secondo esempio):

**In base al nome eseguibile**

```
PS C:\> New-NetQosPolicy -Name background -AppPathNameMatchCondition "C:\Program files (x86)\backup.exe" -PriorityValue8021Action 1

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1

```


**Tramite porte indirizzo IP o protocolli**

```
PS C:\> New-NetQosPolicy -Name "Network Management" -IPDstPrefixMatchCondition 10.240.1.0/24 -IPProtocolMatchCondition both -NetworkProfile all -PriorityValue8021Action 7

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7

```

### <a name="display-qos-policy"></a>Visualizzare i criteri QoS

```
PS C:\> Get-NetQosPolicy

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
PriorityValue  : 4

```

### <a name="modify-qos-policy"></a>Modificare i criteri QoS

È possibile modificare i criteri di QoS, come illustrato di seguito.


```
PS C:\> Set-NetQosPolicy -Name "Network Management" -IPSrcPrefixMatchCondition 10.235.2.0/24 -IPProtocolMatchCondition both -PriorityValue8021Action 7
PS C:\> Get-NetQosPolicy

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPSrcPrefix    : 10.235.2.0/24
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7


```

### <a name="remove-qos-policy"></a>Rimuovere i criteri QoS

```
PS C:\> Remove-NetQosPolicy -Name "Network Management"

Confirm
Are you sure you want to perform this action?
Remove-NetQosPolicy -Name "Network Management" -Store GPO:localhost
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y  

```

## <a name="dcb-configuration-on-network-adapters"></a>Configurazione di DCB sulle schede di rete

Configurazione di DCB sulle schede di rete è indipendente dalla configurazione Data Center Bridging a livello di sistema descritto in precedenza. 

Indipendentemente dal fatto che DCB è installato in Windows Server 2016, è sempre possibile eseguire i comandi seguenti. 

Se si configura DCB da un'opzione e si basano su DCBX per propagare le configurazioni alle schede di rete, è possibile esaminare le configurazioni vengono ricevute e applicate alle schede di rete dal lato del sistema operativo dopo aver abilitato DCB sulle schede di rete.

###  <a name="bkmk_enabledcb"></a>Abilitare e visualizzare le impostazioni di DCB sulle schede di rete

```
PS C:\> Enable-NetAdapterQos M1
PS C:\> Get-NetAdapterQos

Name                       : M1
Enabled                    : True
Capabilities               :                       Hardware     Current
                                                   --------     -------
                             MacSecBypass        : NotSupported NotSupported
                             DcbxSupport         : None         None
                             NumTCs(Max/ETS/PFC) : 8/8/8        8/8/8

OperationalTrafficClasses  : TC TSA    Bandwidth Priorities
                             -- ---    --------- ----------
                              0 ETS    70%       0-3,5-7
                              1 ETS    30%       4

OperationalFlowControl     : All Priorities Disabled
OperationalClassifications : Protocol  Port/Type Priority
                             --------  --------- --------
                             Default             1


```

### <a name="disable-dcb-on-network-adapters"></a>Disabilitare DCB sulle schede di rete

```
PS C:\> Disable-NetAdapterQos M1
PS C:\> Get-NetAdapterQos M1

Name         : M1
Enabled      : False
Capabilities :                       Hardware     Current
                                     --------     -------
               MacSecBypass        : NotSupported NotSupported
               DcbxSupport         : None         None
               NumTCs(Max/ETS/PFC) : 8/8/8        0/0/0  

```
## <a name="bkmk_wps"></a>Comandi di Windows PowerShell per il Data Center Bridging

Sono disponibili comandi DCB Windows PowerShell per Windows Server 2016 e Windows Server 2012 R2. È possibile usare tutti i comandi di Windows Server 2012 R2 in Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandi di Windows Server 2016 Windows PowerShell per Data Center Bridging

L'argomento seguente per Windows Server 2016 fornisce descrizioni dei cmdlet di Windows PowerShell e la sintassi per tutti i Data Center Bridging \(DCB\) Quality of Service \(QoS\)\-cmdlet specifici. I cmdlet sono elencati in ordine alfabetico, in base al verbo presente all'inizio del cmdlet.

- [DcbQoS Module](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Comandi Windows Server 2012 R2 Windows PowerShell per Data Center Bridging

L'argomento seguente per Windows Server 2012 R2 fornisce descrizioni dei cmdlet di Windows PowerShell e la sintassi per tutti i Data Center Bridging \(DCB\) Quality of Service \(QoS\)\-cmdlet specifici. I cmdlet sono elencati in ordine alfabetico, in base al verbo presente all'inizio del cmdlet.

- [Data Center Bridging (DCB) cmdlet per qualità del servizio (QoS) in Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
