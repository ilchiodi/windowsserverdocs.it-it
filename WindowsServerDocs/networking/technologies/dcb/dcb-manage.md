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
ms.openlocfilehash: dbe9e5e4af2ddd834b5b8f38e9ffd1b403e92793
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="manage-data-center-bridging-dcb"></a>Gestire Data Center Bridging (DCB)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento vengono fornite istruzioni su come usare i comandi di Windows PowerShell per configurare \(DCB\) Data Center Bridging in una scheda di rete compatibili con DCB\ che viene installata in un computer che esegue Windows Server 2016 o Windows 10.

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Installare Data Center Bridging in Windows Server 2016 o Windows 10

Per informazioni sui prerequisiti per l'utilizzo e come installare DCB, vedere [installare Data Center Bridging (DCB) in Windows Server 2016 o Windows 10](dcb-install.md).


## <a name="dcb-configurations"></a>Configurazioni di DCB 

Prima di Windows Server 2016, tutte le configurazioni DCB è stato applicato universalmente per tutte le schede di rete supportati DCB. 

In Windows Server 2016, è possibile applicare le configurazioni di DCB per l'archivio dei criteri globali o di singoli criteri Store\(s\). Quando vengono applicati criteri singoli sostituiscono tutte le impostazioni di criteri globali.

Le configurazioni di assegnazione di priorità di classe, applicazioni e di base alla priorità del traffico a livello di sistema non viene applicato alle schede di rete fino a quando non eseguire le operazioni seguenti.

1. Attivare il bit disposti DCBX su false

2. Abilitare DCB sulle schede di rete. Vedere [abilitare e visualizzare le impostazioni di DCB sulle schede di rete](#bkmk_enabledcb).

>[!NOTE]
>Se si desidera configurare DCB da un commutatore tramite DCBX, vedere [impostazioni DCBX](#BKMK_DCBX_Settings)

Il bit disposti DCBX è descritta nella specifica di DCB. Se il bit disposto in un dispositivo è impostato su true, il dispositivo è disposto ad accettare le configurazioni da un dispositivo remoto tramite DCBX. Se il bit disposto in un dispositivo è impostato su false, il dispositivo verrà rifiutare tutti i tentativi di configurazione da dispositivi remoti e applicare solo le configurazioni locali.

Se DCB non è installato in Windows Server 2016 il valore del bit disposti è irrilevante per quanto riguarda il sistema operativo è perché il sistema operativo non sono disponibili impostazioni locali si applicano alle schede di rete. Dopo l'installazione di Data Center Bridging, il valore predefinito del bit disposti è true. Questa progettazione consente le schede di rete mantenere le configurazioni ricevute dal loro peer remoti.

Se una scheda di rete non supporta DCBX, non riceverà le configurazioni da un dispositivo remoto. Configurazioni ricevuto dal sistema operativo, ma solo dopo il DCBX disposti bit è impostato su false.

## <a name="set-the-willing-bit"></a>Impostare il bit disposto

Per applicare le configurazioni del sistema operativo di base alla priorità, classe di traffico e assegnazione di priorità delle applicazioni in schede di rete, o semplicemente sostituire le configurazioni da dispositivi remoti \, se vi sono \, è possibile eseguire il comando seguente.

>[!NOTE]
>I nomi dei comandi di Windows PowerShell DCB includono "QoS" anziché "DCB" nella stringa di nome. Questo avviene perché QoS e Data Center Bridging sono integrati in Windows Server 2016 per fornire un'esperienza di gestione QoS.

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

Per visualizzare lo stato del bit disposti impostazione, è possibile utilizzare il comando seguente:

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>Configurazione di DCB sulle schede di rete

L'abilitazione di DCB in una scheda di rete consente di visualizzare la configurazione da un commutatore propagata alla scheda di rete.

DCB configurazioni includono le seguenti operazioni.

1.  Configurare le impostazioni di Data Center Bridging a livello di sistema, che include:

    un. Gestione della classe di traffico
    
    b. Impostazioni di priorità del flusso di controllo (base alla priorità)
    
    c. Assegnazione di priorità delle applicazioni
    
    d. Impostazioni DCBX

2. Configurare DCB nella scheda di rete.



##  <a name="dcb-traffic-class-management"></a>Gestione della classe di traffico DCB

Di seguito sono esempi di comandi Windows PowerShell per la gestione di classe di traffico.

### <a name="create-a-traffic-class"></a>Creare una classe di traffico

È possibile utilizzare il **New-NetQosTrafficClass** comando per creare una classe di traffico.

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

Per impostazione predefinita, tutti i valori di 802.1p vengono mappati a una classe di traffico predefinito, che dispone di 100% della larghezza di banda del collegamento fisico. Il **New-NetQosTrafficClass** comando crea una nuova classe di traffico, in cui qualsiasi pacchetto che viene contrassegnato con priorità 802.1p valore 4 viene eseguito il mapping. \(TSA\) l'algoritmo di selezione di trasmissione è ETS e ha il 30% della larghezza di banda.

È possibile creare nuove classi di traffico fino a 7. Tra cui la classe di traffico predefinito, possono essere presenti al massimo 8 traffico classi nel sistema. Tuttavia, una scheda di rete che supportano DCB potrebbe non supportare che molte classi nell'hardware del traffico. Se si creano più classi di traffico che possono essere gestite in una scheda di rete e abilitare DCB della scheda di rete, al driver miniport segnala un errore al sistema operativo. L'errore viene registrato nel registro eventi.

La somma delle prenotazioni della larghezza di banda per tutte le classi di traffico creato non può superare 99% della larghezza di banda. La classe di traffico predefinito ha sempre almeno 1% della larghezza di banda riservata per se stesso.

### <a name="display-traffic-classes"></a>Visualizzare le classi di traffico

È possibile utilizzare il **Get-NetQosTrafficClass** comando per visualizzare le classi di traffico.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>Modificare una classe di traffico

È possibile utilizzare il **Set-NetQosTrafficClass** comando per creare una classe di traffico. 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

È quindi possibile utilizzare il **Get-NetQosTrafficClass** comando per visualizzare le impostazioni.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

Dopo aver creato una classe di traffico, è possibile modificarne le impostazioni in modo indipendente. Le impostazioni che modificabili includono:

1. \(-BandwidthPercentage\) allocazione della larghezza di banda

2. TSA (\-Algorithm\)

3. Priorità mapping \(-Priority\)

### <a name="remove-a-traffic-class"></a>Rimuovere una classe di traffico

È possibile utilizzare il **Remove-NetQosTrafficClass** comando per eliminare una classe di traffico.

>[!IMPORTANT]
>È possibile rimuovere la classe di traffico predefinita.


    Remove-NetQosTrafficClass -Name SMB

È quindi possibile utilizzare il **Get-NetQosTrafficClass** comando per visualizzare le impostazioni.
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

Dopo la rimozione di una classe di traffico, il valore 802.1p è mappato a che viene modificato il mapping di classe di traffico per la classe di traffico predefinita. Larghezza di banda che era stato riservato per una classe di traffico viene restituito per l'allocazione di classe di traffico predefinita quando viene rimosso la classe di traffico.

## <a name="per-network-interface-policies"></a>Interfaccia i criteri di rete

Tutti gli esempi precedenti impostati criteri globali. Di seguito sono esempi di come è possibile impostare e ottenere i criteri per ogni scheda di rete. 

Il campo "PolicySet" diventa AdapterSpecific da globale. Quando sono visualizzati AdapterSpecific criteri, vengono visualizzate anche l'indice di interfaccia \(ifIndex\) e \(ifAlias\) nome dell'interfaccia.

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

Di seguito sono esempi di comandi per le impostazioni di controllo di flusso di priorità. Queste impostazioni possono anche essere specificate per singole schede.

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>Abilitare e priorità di flusso di controllo per la visualizzazione per globali e specifici di interfaccia di casi d'uso

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

##  <a name="application-priority-assignment"></a>Assegnazione di priorità delle applicazioni

Di seguito sono esempi di assegnazione di priorità.

### <a name="create-qos-policy"></a>Creare criteri QoS

```
PS C:\> New-NetQosPolicy -Name "SMB Policy" -PriorityValue8021Action 4

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
PriorityValue  : 4

```

Il comando precedente crea un nuovo criterio per SMB. -SMB è un filtro della posta in arrivo corrispondente porta TCP 445 (riservato per SMB). Se un pacchetto viene inviato alla porta TCP 445 che verrà contrassegnato dal sistema operativo con valore 802.1p 4 prima che il pacchetto venga passato a un driver miniport di rete.

Oltre ai: SMB, altri filtri predefiniti includono – iSCSI (porta TCP corrispondente 3260), - NFS (porta TCP corrispondente 2049), - LiveMigration (porta TCP corrispondente 6600), - FCOE (corrispondenza EtherType 0x8906) e – NetworkDirect.

NetworkDirect è un livello astratto che creiamo sopra qualsiasi implementazione RDMA in una scheda di rete. NetworkDirect – deve essere seguito da una porta di rete diretto.

Oltre ai filtri predefiniti, è possibile classificare il traffico in base al nome di eseguibile dell'applicazione (ad esempio il primo esempio riportato di seguito) o indirizzo IP, porta o protocollo (come illustrato nel secondo esempio):

**Nome eseguibile**

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


**Porta di indirizzo IP o protocollo**

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

È possibile modificare i criteri QoS, come illustrato di seguito.


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

### <a name="remove-qos-policy"></a>Rimuovere il criterio QoS

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

Se si configura DCB da un commutatore e si basano su DCBX per propagare le configurazioni per schede di rete, è possibile esaminare le configurazioni vengono ricevuti e applicati alle schede di rete dal lato del sistema operativo dopo aver abilitato DCB sulle schede di rete.

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
## <a name="bkmk_wps"></a>Comandi di Windows PowerShell per DCB

Sono disponibili i comandi di Windows PowerShell DCB per Windows Server 2016 e Windows Server 2012 R2. È possibile utilizzare tutti i comandi per Windows Server 2012 R2 in Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandi di Windows Server 2016 Windows PowerShell per DCB

L'argomento seguente per Windows Server 2016 fornisce le descrizioni di cmdlet di Windows PowerShell e la sintassi per tutti i \(DCB\) Data Center Bridging Quality of Service \ cmdlet \-specific (QoS\). Elenca i cmdlet in ordine alfabetico in base al verbo all'inizio del cmdlet.

- [Modulo DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows Server 2012 R2 comandi di Windows PowerShell per DCB

L'argomento seguente per Windows Server 2012 R2 fornisce le descrizioni di cmdlet di Windows PowerShell e la sintassi per tutti i \(DCB\) Data Center Bridging Quality of Service \ cmdlet \-specific (QoS\). Elenca i cmdlet in ordine alfabetico in base al verbo all'inizio del cmdlet.

- [Data Center Bridging (DCB) cmdlet per qualità del servizio (QoS) in Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
