---
title: Gestire Data Center Bridging (DCB)
description: Questo argomento fornisce istruzioni su come usare i comandi di Windows PowerShell per gestire Data Center Bridging in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d61287b82cd6d3b869b1120d3cb21b3c8792bd1e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312757"
---
# <a name="manage-data-center-bridging-dcb"></a>Gestire Data Center Bridging (DCB)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento fornisce istruzioni su come usare i comandi di Windows PowerShell per configurare Data Center Bridging \(DCB\) in una scheda di rete compatibile con DCB\-installata in un computer che esegue Windows Server 2016 o Windows 10.

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Installare DCB in Windows Server 2016 o Windows 10

Per informazioni sui prerequisiti per l'uso di e su come installare DCB, vedere [installare Data Center Bridging (DCB) in Windows Server 2016 o Windows 10](dcb-install.md).


## <a name="dcb-configurations"></a>Configurazioni di DCB 

Prima di Windows Server 2016, tutta la configurazione di DCB veniva applicata universalmente a tutte le schede di rete che supportavano DCB. 

In Windows Server 2016 è possibile applicare le configurazioni DCB all'archivio criteri globale o a un singolo archivio criteri\(s\). Quando vengono applicati criteri singoli, sostituiscono tutte le impostazioni dei criteri globali.

Le configurazioni di classe di traffico, PFC e assegnazione di priorità dell'applicazione a livello di sistema non vengono applicate alle schede di rete fino a quando non vengono eseguite le operazioni seguenti.

1. Trasforma il bit DCBX disposto su false

2. Abilitare DCB nelle schede di rete. Vedere [abilitare e visualizzare le impostazioni DCB nelle schede di rete](#bkmk_enabledcb).

>[!NOTE]
>Se si vuole configurare DCB da un passaggio tramite DCBX, vedere [Impostazioni DCBX](#dcb-configuration-on-network-adapters).

Il bit DCBX disposto è descritto nella specifica DCB. Se il bit disponibile in un dispositivo è impostato su true, il dispositivo è disposto ad accettare le configurazioni da un dispositivo remoto tramite DCBX. Se il bit disponibile in un dispositivo è impostato su false, il dispositivo rifiuterà tutti i tentativi di configurazione dai dispositivi remoti e imporrà solo le configurazioni locali.

Se DCB non è installato in Windows Server 2016, il valore del bit disposto è irrilevante per quanto riguarda il sistema operativo, perché il sistema operativo non dispone di impostazioni locali valide per le schede di rete. Dopo l'installazione di DCB, il valore predefinito del bit disposto è true. Questa progettazione consente alle schede di rete di gestire qualsiasi configurazione ricevuta dai peer remoti.

Se una scheda di rete non supporta DCBX, le configurazioni non vengono mai ricevute da un dispositivo remoto. Riceve le configurazioni dal sistema operativo, ma solo dopo che il bit DCBX è impostato su false.

## <a name="set-the-willing-bit"></a>Imposta il bit disponibile

Per applicare le configurazioni del sistema operativo di classe di traffico, PFC e assegnazione di priorità alle applicazioni sulle schede di rete o semplicemente eseguire l'override delle configurazioni da dispositivi remoti \, se presente, è possibile eseguire il comando seguente.

>[!NOTE]
>I nomi dei comandi di Windows PowerShell per DCB includono "QoS" invece di "DCB" nella stringa del nome. Questo perché QoS e DCB sono integrati in Windows Server 2016 per offrire un'esperienza di gestione QoS senza problemi.

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

Per visualizzare lo stato dell'impostazione del bit disponibile, è possibile usare il comando seguente:

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>Configurazione di DCB nelle schede di rete

L'abilitazione di DCB in una scheda di rete consente di visualizzare la configurazione propagata da un passaggio alla scheda di rete.

Le configurazioni di DCB includono i passaggi seguenti.

1.  Configurare le impostazioni di DCB a livello di sistema, incluse le seguenti:

    a. Gestione delle classi di traffico
    
    b. Impostazioni del controllo di flusso prioritario (PFC)
    
    c. Assegnazione priorità applicazione
    
    d. Impostazioni DCBX

2. Configurare DCB nella scheda di rete.



##  <a name="dcb-traffic-class-management"></a>Gestione delle classi di traffico DCB

Di seguito sono riportati alcuni comandi di Windows PowerShell per la gestione delle classi di traffico.

### <a name="create-a-traffic-class"></a>Creare una classe di traffico

Per creare una classe di traffico, è possibile usare il comando **New-NetQosTrafficClass** .

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

Per impostazione predefinita, viene eseguito il mapping di tutti i valori 802.1 p a una classe di traffico predefinita, che presenta il 100% della larghezza di banda del collegamento fisico. Il comando **New-NetQosTrafficClass** crea una nuova classe di traffico a cui viene eseguito il mapping di tutti i pacchetti contrassegnati con il valore di priorità 4 802.1 p. L'algoritmo di selezione della trasmissione \(TSA\) è ETS e il 30% della larghezza di banda.

È possibile creare fino a 7 nuove classi di traffico. Includendo la classe di traffico predefinita, nel sistema possono essere presenti al massimo 8 classi di traffico. Tuttavia, una scheda di rete con supporto per DCB potrebbe non supportare la maggior parte delle classi di traffico nell'hardware. Se si creano più classi di traffico rispetto a quelle che possono essere incluse in una scheda di rete e si Abilita DCB sulla scheda di rete, il driver miniport segnala un errore al sistema operativo. L'errore viene registrato nel registro eventi.

La somma delle prenotazioni della larghezza di banda per tutte le classi di traffico create non può superare il 99% della larghezza di banda. Per la classe di traffico predefinita è sempre disponibile almeno l'1% della larghezza di banda riservata.

### <a name="display-traffic-classes"></a>Visualizzare le classi di traffico

È possibile usare il comando **Get-NetQosTrafficClass** per visualizzare le classi di traffico.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>Modificare una classe di traffico

È possibile usare il comando **set-NetQosTrafficClass** per creare una classe di traffico. 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

È quindi possibile usare il comando **Get-NetQosTrafficClass** per visualizzare le impostazioni.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

Dopo aver creato una classe di traffico, è possibile modificarne le impostazioni in modo indipendente. Le impostazioni che è possibile modificare includono:

1. Allocazione della larghezza di banda \(-BandwidthPercentage\)

2. TSA (\-algoritmo\)

3. Mapping priorità \(-priorità\)

### <a name="remove-a-traffic-class"></a>Rimuovere una classe di traffico

Per eliminare una classe di traffico, è possibile usare il comando **Remove-NetQosTrafficClass** .

>[!IMPORTANT]
>Non è possibile rimuovere la classe di traffico predefinita.


    Remove-NetQosTrafficClass -Name SMB

È quindi possibile usare il comando **Get-NetQosTrafficClass** per visualizzare le impostazioni.
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

Dopo la rimozione di una classe di traffico, il valore 802.1 p mappato a tale classe di traffico viene rimappato alla classe di traffico predefinita. Una larghezza di banda riservata a una classe di traffico viene restituita all'allocazione della classe di traffico predefinita quando la classe di traffico viene rimossa.

## <a name="per-network-interface-policies"></a>Criteri dell'interfaccia per rete

Tutti gli esempi precedenti impostano i criteri globali. Di seguito sono riportati alcuni esempi di come è possibile impostare e ottenere i criteri per NIC. 

Il campo "PolicySet" passa da globale a AdapterSpecific. Quando vengono visualizzati i criteri di AdapterSpecific, vengono visualizzati anche l'indice dell'interfaccia \(ifIndex\) e il nome dell'interfaccia \(ifAlias\).

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

## <a name="priority-flow-control-settings"></a>Impostazioni di controllo del flusso di priorità:

Di seguito sono riportati esempi di comando per le impostazioni di controllo del flusso di priorità. Queste impostazioni possono essere specificate anche per i singoli adapter.

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>Abilitare e visualizzare il controllo di flusso prioritario per casi d'uso specifici dell'interfaccia e globali

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


### <a name="disable-priority-flow-control-global-and-interface-specific"></a>Disabilitare il controllo di flusso prioritario (globale e specifico dell'interfaccia)

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

##  <a name="application-priority-assignment"></a>Assegnazione priorità applicazione

Di seguito sono riportati alcuni esempi di assegnazione di priorità.

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

Il comando precedente crea un nuovo criterio per SMB. -SMB è un filtro della posta in arrivo che corrisponde alla porta TCP 445 (riservata per SMB). Se viene inviato un pacchetto alla porta TCP 445, questo verrà contrassegnato dal sistema operativo con il valore 802.1 p 4 prima che il pacchetto venga passato a un driver miniport di rete.

Oltre a-SMB, altri filtri predefiniti includono-iSCSI (la porta TCP corrispondente 3260),-NFS (corrispondente alla porta TCP 2049),-LiveMigration (corrispondente alla porta TCP 6600),-FCOE (corrispondente a EtherType 0x8906) e – NetworkDirect.

NetworkDirect è un livello astratto creato in base a qualsiasi implementazione di RDMA in una scheda di rete. – NetworkDirect deve essere seguito da una porta diretta di rete.

Oltre ai filtri predefiniti, è possibile classificare il traffico in base al nome dell'eseguibile dell'applicazione (come nel primo esempio riportato di seguito) o in base all'indirizzo IP, alla porta o al protocollo (come illustrato nel secondo esempio):

**Per nome eseguibile**

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


**Per porta o protocollo indirizzo IP**

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

È possibile modificare i criteri QoS come illustrato di seguito.


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

## <a name="dcb-configuration-on-network-adapters"></a>Configurazione di DCB nelle schede di rete

La configurazione di DCB nelle schede di rete è indipendente dalla configurazione di DCB a livello di sistema descritto in precedenza. 

Indipendentemente dal fatto che DCB sia installato in Windows Server 2016, è sempre possibile eseguire i comandi seguenti. 

Se si configura DCB da un'opzione e si utilizza DCBX per propagare le configurazioni a schede di rete, è possibile esaminare le configurazioni ricevute e applicate alle schede di rete dal lato sistema operativo dopo aver abilitato DCB sulle schede di rete.

###  <a name="enable-and-display-dcb-settings-on--network-adapters"></a><a name="bkmk_enabledcb"></a>Abilitare e visualizzare le impostazioni DCB nelle schede di rete

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

### <a name="disable-dcb-on-network-adapters"></a>Disabilitare DCB nelle schede di rete

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
## <a name="windows-powershell-commands-for-dcb"></a><a name="bkmk_wps"></a>Comandi di Windows PowerShell per DCB

Sono disponibili comandi di Windows PowerShell DCB per Windows Server 2016 e Windows Server 2012 R2. È possibile usare tutti i comandi per Windows Server 2012 R2 in Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandi di Windows PowerShell per Windows Server 2016 per DCB

L'argomento seguente per Windows Server 2016 fornisce le descrizioni e la sintassi dei cmdlet di Windows PowerShell per tutti i Data Center Bridging \(DCB\) qualità del servizio \(QoS\)\-cmdlet specifici. I cmdlet sono elencati in ordine alfabetico, in base al verbo iniziale.

- [Modulo DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Comandi di Windows PowerShell per Windows Server 2012 R2 per DCB

L'argomento seguente per Windows Server 2012 R2 fornisce le descrizioni e la sintassi dei cmdlet di Windows PowerShell per tutti i Data Center Bridging \(DCB\) qualità del servizio \(QoS\)\-cmdlet specifici. I cmdlet sono elencati in ordine alfabetico, in base al verbo iniziale.

- [Cmdlet di qualità del servizio (QoS) di Data Center Bridging (DCB) in Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
