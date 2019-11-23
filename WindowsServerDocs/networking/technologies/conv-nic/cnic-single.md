---
title: Configurazione NIC convergente con una singola scheda di rete
description: In questo argomento vengono fornite le istruzioni per configurare una scheda di interfaccia di rete convergente con una singola scheda di interfaccia di rete nell'host Hyper-V.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: 2ad7592fd9faf1e92893e6271daabdad907d3aaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405800"
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>Configurazione NIC convergente con una singola scheda di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono fornite le istruzioni per configurare una scheda di interfaccia di rete convergente con una singola scheda di interfaccia di rete nell'host Hyper-V.

La configurazione di esempio in questo argomento descrive due host Hyper-V, **Hyper-v host A**e **Hyper-v host B**. Per entrambi gli host è installata una sola scheda di interfaccia di rete fisica (installare) e le schede di interfaccia di rete sono connesse a una parte superiore del rack \(ToR\) commutatore fisico. Inoltre, gli host si trovano nella stessa subnet, ovvero 192.168.1. x/24.

![Host Hyper-V](../../media/Converged-NIC/1-single-test-conn.jpg)


## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Passaggio 1. Testare la connettività tra l'origine e la destinazione

Verificare che la scheda di interfaccia di rete fisica sia in grado di connettersi all'host di destinazione. Questo test illustra la connettività usando il livello 3 \(L3\) o il livello IP, nonché il livello 2 \(L2\).

1. Visualizzare le proprietà della scheda di rete.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Risultati**_  


   | Nome |    InterfaceDescription     | IfIndex | Stato |    MacAddress     | LinkSpeed |
   |------|-----------------------------|---------|--------|-------------------|-----------|
   |  M1  | Mellanox ConnectX-3 Pro... |    4    |   Su   | 7C-FE-90-93-8F-A1 |  40 Gbps  |

   ---

2. Visualizzare le proprietà aggiuntive dell'adapter, incluso l'indirizzo IP.

   ```PowerShell
   Get-NetAdapter M1 | fl *
   ```

   _**Risultati**_

   ```PowerShell   
    MacAddress   : 7C-FE-90-93-8F-A1
    Status   : Up
    LinkSpeed: 40 Gbps
    MediaType: 802.3
    PhysicalMediaType: 802.3
    AdminStatus  : Up
    MediaConnectionState : Connected
    DriverInformation: Driver Date 2016-08-28 Version 5.25.12665.0 NDIS 6.60
    DriverFileName   : mlx4eth63.sys
    NdisVersion  : 6.60
    ifOperStatus : Up
    ifAlias  : M1
    InterfaceAlias   : M1
    ifIndex  : 4
    ifDesc   : Mellanox ConnectX-3 Pro Ethernet Adapter
    ifName   : ethernet_32773
    DriverVersion: 5.25.12665.0
    LinkLayerAddress : 7C-FE-90-93-8F-A1
    Caption  :
    Description  :
    ElementName  :
    InstanceID   : {39B58B4C-8833-4ED2-A2FD-E105E7146D43}
    CommunicationStatus  :
    DetailedStatus   :
    HealthState  :
    InstallDate  :
    Name : M1
    OperatingStatus  :
    OperationalStatus:
    PrimaryStatus:
    StatusDescriptions   :
    AvailableRequestedStates :
    EnabledDefault   : 2
    EnabledState : 5
    OtherEnabledState:
    RequestedState   : 12
    TimeOfLastStateChange:
    TransitioningToState : 12
    AdditionalAvailability   :
    Availability :
    CreationClassName: MSFT_NetAdapter
   ``` 

## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Passaggio 2. Verificare che l'origine e la destinazione siano in grado di comunicare

In questo passaggio viene usato il comando **test-NetConnection** di Windows PowerShell, ma se si preferisce è possibile usare il comando **ping** . 

>[!TIP]
>Se si è certi che gli host possano comunicare tra loro, è possibile ignorare questo passaggio.

1. Verificare la comunicazione bidirezionale.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Risultati**_


   |        Parametro         |    Valore    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      Indirizzo remoto       | 192.168.1.5 |
   |      InterfaceAlias      |     M1      |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0 ms     |

   ---

   In alcuni casi, potrebbe essere necessario disabilitare Windows Firewall con sicurezza avanzata per eseguire correttamente questo test. Se si disabilita il firewall, tenere presente la sicurezza e assicurarsi che la configurazione soddisfi i requisiti di sicurezza dell'organizzazione.

2. Disabilitare tutti i profili firewall.

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```

3. Dopo aver disabilitato i profili firewall, testare di nuovo la connessione. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Risultati**_


   |        Parametro         |    Valore    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      Indirizzo remoto       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | PingReplyDetails \(RTT\) |    0 ms     |

   ---



## <a name="step-3-optional-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Passaggio 3. Opzionale Configurare gli ID VLAN per le schede di rete installate negli host Hyper-V

Molte configurazioni di rete usano VLAN e, se si prevede di usare VLAN nella rete, è necessario ripetere il test precedente con VLAN configurate. Inoltre, se si prevede di usare RoCE per i servizi RDMA, è necessario abilitare le VLAN.

Per questo passaggio, le schede di rete sono in modalità di **accesso** . Tuttavia, quando si crea un commutire virtuale Hyper-V \(vSwitch\) più avanti in questa guida, le proprietà VLAN vengono applicate a livello di porta vSwitch. 

Poiché un commutatore può ospitare più VLAN, è necessario che la parte superiore del rack \(ToR\) commutatore fisico per avere la porta a cui l'host è connesso configurato in modalità trunk.

>[!NOTE]
>Per istruzioni su come configurare la modalità trunk nel commutatore, vedere la documentazione del commutatore ToR.

Nella figura seguente vengono illustrati due host Hyper-V, ognuno con una scheda di rete fisica e ciascuno configurato per la comunicazione con la VLAN 101.

![Configurare reti locali virtuali](../../media/Converged-NIC/2-single-configure-vlans.jpg)


>[!IMPORTANT]
>Eseguire questa operazione nel server locale e in quello di destinazione. Se il server di destinazione non è configurato con lo stesso ID VLAN del server locale, i due non possono comunicare.


1. Configurare l'ID VLAN per le schede di rete installate negli host Hyper-V.

   >[!IMPORTANT]
   >Non eseguire questo comando se si è connessi all'host in modalità remota tramite questa interfaccia, perché ciò comporta una perdita di accesso all'host.

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
   ```

   _**Risultati**_


   | Nome | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------|-------------|--------------|-----------------|---------------|
   |  M1  |   ID VLAN   |     101      |     VlanID      |     {101}     |

   ---

2. Riavviare la scheda di rete per applicare l'ID VLAN.

   ```PowerShell
   Restart-NetAdapter -Name "M1"
   ```

3. Verificare che lo stato sia **attivo**.

   ```PowerShell
   Get-NetAdapter -Name "M1"
   ```

   _**Risultati**_


   | Nome |          InterfaceDescription           | IfIndex | Stato |    MacAddress     | LinkSpeed |
   |------|-----------------------------------------|---------|--------|-------------------|-----------|
   |  M1  | Mellanox ConnectX-3 Pro Ethernet Ada... |    4    |   Su   | 7C-FE-90-93-8F-A1 |  40 Gbps  |

   ---

   >[!IMPORTANT]
   >Potrebbe essere necessario attendere alcuni secondi prima che il dispositivo venga riavviato e reso disponibile sulla rete. 

4. Verificare la connettività.<p>Se la connettività non riesce, controllare la configurazione della VLAN del Commuter o la partecipazione alla destinazione nella stessa VLAN. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

## <a name="step-4-configure-quality-of-service-qos"></a>Passaggio 4. Configurare la qualità del servizio \(QoS\)

>[!NOTE]
>È necessario eseguire tutti i passaggi di configurazione DCB e QoS seguenti in tutti gli host che hanno lo scopo di comunicare tra loro.

1. Installare Data Center Bridging \(DCB\) in ogni host Hyper-V.

   - **Facoltativo** per le configurazioni di rete che usano iWarp per i servizi RDMA.
   - **Obbligatorio** per le configurazioni di rete che usano roce \(qualsiasi versione\) per i servizi RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

2. Impostare i criteri QoS per SMB-Direct:

   - **Facoltativo** per le configurazioni di rete che usano iWarp.
   - **Obbligatorio** per le configurazioni di rete che usano roce.

   Nel comando di esempio seguente il valore "3" è arbitrario. È possibile usare qualsiasi valore compreso tra 1 e 7, purché si usi costantemente lo stesso valore per tutta la configurazione dei criteri QoS.

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**Risultati**_


   |   Parametro    |          Valore           |
   |----------------|--------------------------|
   |      Nome      |           SMB            |
   |     Proprietario      | \) computer \(Criteri di gruppo |
   | NetworkProfile |           Tutte            |
   |   Precedenza   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

   ---

3. Per le distribuzioni RoCE, attivare il **controllo del flusso di priorità** per il traffico SMB, che non è necessario per iWarp.

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**Risultati**_


   | Priority | Enabled | PolicySet | IfIndex | IfAlias |
   |----------|---------|-----------|---------|---------|
   |    0     |  False  |  Globale   | &nbsp;  | &nbsp;  |
   |    1     |  False  |  Globale   | &nbsp;  | &nbsp;  |
   |    2     |  False  |  Globale   | &nbsp;  | &nbsp;  |
   |    3     |  True   |  Globale   | &nbsp;  | &nbsp;  |
   |    4     |  False  |  Globale   | &nbsp;  | &nbsp;  |
   |    5     |  False  |  Globale   | &nbsp;  | &nbsp;  |
   |    6     |  False  |  Globale   | &nbsp;  | &nbsp;  |
   |    7     |  False  |  Globale   | &nbsp;  | &nbsp;  |

   ---

4. Abilitare QoS per le schede di rete locali e di destinazione.

   - **Non necessario** per le configurazioni di rete che usano iWarp.
   - **Obbligatorio** per le configurazioni di rete che usano roce.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "M1"
   Get-NetAdapterQos -Name "M1"
   ```

   _**Risultati**_

   **Nome**: M1  
   **Abilitato**: true  

   _**Funzionalità**_   


   |      Parametro      |   Hardware   |   Corrente    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     Nessuno     |     Nessuno     |
   | NumTCs (max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses:**_ 


   | TC | RICONOSCIMENTO timestamp | Banda | Priorità |
   |----|-----|-----------|------------|
   | 0  | ETS |    70%    |  0-2, 4-7   |
   | 1  | ETS |    30    |     3      |

   ---

   _**OperationalFlowControl:**_  

   Priorità 3 abilitata  

   _**OperationalClassifications:**_  


   | Protocollo  | Porta/tipo | Priority |
   |-----------|-----------|----------|
   |  Impostazione predefinita  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

5. Riservare una percentuale della larghezza di banda per SMB diretto \(RDMA\).

    In questo esempio viene usata una prenotazione di larghezza di banda del 30%. È necessario selezionare un valore che rappresenta quello che si prevede debba essere richiesto dal traffico di archiviazione. 

   ```PowerShell
   New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS
   ```

   _**Risultati**_


   | Nome | Algoritmo | Larghezza di banda (%) | Priority | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      30      |    3     |  Globale   | &nbsp;  | &nbsp;  |

   ---                                      

6. Visualizzare le impostazioni di prenotazione della larghezza di banda.  

   ```PowerShell
   Get-NetQosTrafficClass
   ```

   _**Risultati**_


   |   Nome    | Algoritmo | Larghezza di banda (%) | Priority | PolicySet | IfIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | Predefinita |    ETS    |      70      | 0-2, 4-7  |  Globale   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      30      |    3     |  Globale   | &nbsp;  | &nbsp;  |

   ---

## <a name="step-5-optional-resolve-the-mellanox-adapter-debugger-conflict"></a>Passaggio 5. Opzionale Risolvere il conflitto del debugger dell'adattatore Mellanox 

Per impostazione predefinita, quando si usa una scheda Mellanox, il debugger associato blocca NetQos, che è un problema noto. Se pertanto si utilizza un adapter di Mellanox e si intende alleghiare un debugger, utilizzare il comando seguente per risolvere il problema. Questo passaggio non è necessario se non si intende alleghi un debugger o se non si usa una scheda Mellanox.

   ```PowerShell    
   Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
   ``` 

## <a name="step-6-verify-the-rdma-configuration-native-host"></a>Passaggio 6. Verificare la configurazione di RDMA (host nativo)

Si vuole verificare che l'infrastruttura sia configurata correttamente prima di creare un vSwitch e passare a RDMA (NIC convergente). 

La figura seguente mostra lo stato corrente degli host Hyper-V.

![Test RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

1. Verificare la configurazione RDMA.

   ```PowerShell
   Get-NetAdapterRdma
   ```
   _**Risultati**_


   | Nome |           InterfaceDescription           | Enabled |
   |------|------------------------------------------|---------|
   |  M1  | Mellanox ConnectX-3 scheda Ethernet Pro |  True   |

   ---

2. Determinare il valore **ifindex** dell'adapter di destinazione.<p>Questo valore viene usato nei passaggi successivi quando si esegue lo script scaricato.

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**Risultati**_ 


   | InterfaceAlias | IndiceInterfaccia |  Indirizzo IPv4  |
   |----------------|----------------|---------------|
   |       M2       |       14       | 192.168.1.5 |

   ---

3. Scaricare l' [utilità DiskSpd. exe](https://aka.ms/diskspd) ed estrarla in C:\test\.

4. Scaricare lo [script di PowerShell test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) in una cartella di test nell'unità locale, ad esempio C:\test\.

5. Eseguire lo script di PowerShell **test-RDMA. ps1** per passare il valore ifindex allo script, insieme all'indirizzo IP della scheda remota nella stessa VLAN.<p>In questo esempio lo script passa il valore **ifindex** 14 nell'indirizzo IP della scheda di rete remota 192.168.1.5.

   ```PowerShell
    C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\

    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter M2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
    VERBOSE: Remote IP 192.168.1.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 662979201 RDMA bytes written per second
    VERBOSE: 37561021 RDMA bytes sent per second
    VERBOSE: 1023098948 RDMA bytes written per second
    VERBOSE: 8901349 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
   ```

   >[!NOTE]
   >Se il traffico RDMA ha esito negativo, per il caso RoCE, consultare la configurazione del commutatore ToR per le impostazioni appropriate di PFC/ETS che devono corrispondere alle impostazioni host. Per i valori di riferimento, vedere la sezione QoS in questo documento.

## <a name="step-7-remove-the-access-vlan-setting"></a>Passaggio 7. Rimuovere l'impostazione di accesso VLAN

Per preparare la creazione del Commuter Hyper-V, è necessario rimuovere le impostazioni VLAN installate in precedenza.  

1. Rimuovere l'impostazione della VLAN di accesso dalla scheda di interfaccia di rete fisica per impedire che la scheda di interfaccia di rete possa contrassegnare automaticamente il traffico in uscita con l'ID VLAN errato.<p>La rimozione di questa impostazione impedisce anche di filtrare il traffico in ingresso che non corrisponde all'ID VLAN di accesso.

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
   ```    

2. Verificare che l' **impostazione VLANID** indichi il valore ID VLAN come zero.

   ```PowerShell    
   Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
   ```  


## <a name="step-8-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Passaggio 8. Creare un vSwitch Hyper-V negli host Hyper-V

Nell'immagine seguente viene illustrato Hyper-V host 1 con un vSwitch.

![Creazione di un Commuter virtuale Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


1. Creare un vSwitch esterno di Hyper-V in Hyper-V nell'host Hyper-v A. <p>In questo esempio, l'opzione è denominata VMSTEST. Il parametro **AllowManagementOS** crea anche un vNIC host che eredita gli indirizzi Mac e IP della scheda di interfaccia di rete fisica.

   ```PowerShell
   New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true
   ```
   _**Risultati**_


   |  Nome   | SwitchType |      Parametro netadapterinterfacedescription      |
   |---------|------------|------------------------------------------|
   | VMSTEST |  Esterno  | Mellanox ConnectX-3 scheda Ethernet Pro |

   ---

2. Visualizzare le proprietà della scheda di rete.

   ```PowerShell
   Get-NetAdapter | ft -AutoSize
   ```

   _**Risultati**_


   |         Nome          |        InterfaceDescription         | IfIndex | Stato |    MacAddress     | LinkSpeed |
   |-----------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet \(VMSTEST\) | Scheda Ethernet virtuale Hyper-V #2 |   27    |   Su   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---

3. Gestire l'host vNIC in uno dei due modi seguenti. 

   - La vista **NetAdapter** funziona in base al nome "VETHERNET \(VMSTEST\)". Non tutte le proprietà della scheda di rete vengono visualizzate in questa visualizzazione.
   - **VMNetworkAdapter** View Elimina il prefisso "vEthernet" e usa semplicemente il nome VMSwitch. (Consigliato) 

   ```PowerShell
   Get-VMNetworkAdapter –ManagementOS | ft -AutoSize
   ```

   _**Risultati**_


   |         Nome         | Gestione dei |        VMName        |  SwitchName  | MacAddress | Stato | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-External-switch |      True      | CORP-External-switch | 001B785768AA |    OK    | &nbsp; |             |
   |       VMSTEST        |      True      |       VMSTEST        | E41D2D074071 |    OK    | &nbsp; |             |

   ---

4. Testare la connessione.

   ```Powershell    
   Test-NetConnection 192.168.1.5
   ```

   _**Risultati**_ 

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

5. Assegnare e visualizzare le impostazioni VLAN della scheda di rete.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```    

   _**Risultati**_


   | VMName | VMNetworkAdapterName |  Modalità  | VlanList |
   |--------|----------------------|--------|----------|
   | &nbsp; |       VMSTEST        | Accesso |   101    |

   ---  

6. Testare la connessione.<p>Il completamento dell'operazione potrebbe richiedere alcuni secondi prima che sia possibile eseguire correttamente il ping dell'altra scheda.  

   ```PowerShell    
   Test-NetConnection 192.168.1.5
   ```

   _**Risultati**_

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-9-test-hyper-v-virtual-switch-rdma-mode-2"></a>Passaggio 9: Test del Commuter virtuale Hyper-V RDMA (modalità 2)

La figura seguente illustra lo stato corrente degli host Hyper-V, incluso vSwitch in Hyper-V host 1.

![Testare il Commuter virtuale Hyper-V](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


1. Impostare l'assegnazione di tag di priorità nell'host vNIC.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
   ```  

   _**Risultati**_

    Nome: VMSTEST IeeePriorityTag: on


2. Visualizzare le informazioni di RDMA sulla scheda di rete. 

   ```PowerShell
   Get-NetAdapterRdma
   ```   

   _**Risultati**_


   |         Nome          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST\) | Scheda Ethernet virtuale Hyper-V #2 |  False  |

   ---

   >[!NOTE]
   >Se il parametro **Enabled** ha il valore **false**, significa che RDMA non è abilitato.


3. Visualizzare le informazioni sulla scheda di rete.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Risultati**_   


   |        Nome         |        InterfaceDescription         | IfIndex | Stato |    MacAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (VMSTEST) | Scheda Ethernet virtuale Hyper-V #2 |   27    |   Su   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---


4. Abilitare RDMA nell'host vNIC.

   ```PowerShell
   Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   ```

   _**Risultati**_


   |         Nome          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST\) | Scheda Ethernet virtuale Hyper-V #2 |  True   |

   ---

   >[!NOTE]
   >Se il parametro **Enabled** ha il valore **true**, significa che RDMA è abilitato.

5. Eseguire il test del traffico RDMA.

   ```PowerShell    
    C:\TEST\Test-RDMA.PS1 -IfIndex 27 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter vEthernet (VMSTEST) is a virtual adapter
    VERBOSE: Retrieving vSwitch bound to the virtual adapter
    VERBOSE: Found vSwitch: VMSTEST
    VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
    VERBOSE: Remote IP 192.168.1.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 9162492 RDMA bytes sent per second
    VERBOSE: 938797258 RDMA bytes written per second
    VERBOSE: 34621865 RDMA bytes sent per second
    VERBOSE: 933572610 RDMA bytes written per second
    VERBOSE: 35035861 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
   ```

La riga finale in questo output, "test del traffico RDMA riuscito: il traffico RDMA è stato inviato a 192.168.1.5", indica che è stata configurata correttamente la scheda di interfaccia di rete convergente nella scheda.

## <a name="related-topics"></a>Argomenti correlati
- [Configurazione NIC convergente con gruppo NIC](cnic-datacenter.md)
- [Configurazione del Commuter fisico per NIC convergente](cnic-app-switch-config.md)
- [Risoluzione dei problemi relativi alle configurazioni NIC convergenti](cnic-app-troubleshoot.md)
