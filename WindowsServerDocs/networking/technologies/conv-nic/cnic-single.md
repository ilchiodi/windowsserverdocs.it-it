---
title: Configurazione di interfaccia di rete convergente con una sola scheda di rete
description: In questo argomento, ti offriamo le istruzioni per configurare l'interfaccia di rete convergente con una singola scheda di rete nell'host Hyper-V.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: 7777278f374984f242e44fd8a8fa94388df88a30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836472"
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>Configurazione di interfaccia di rete convergente con una sola scheda di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento, ti offriamo le istruzioni per configurare l'interfaccia di rete convergente con una singola scheda di rete nell'host Hyper-V.

La configurazione di esempio in questo argomento descrive due host Hyper-V **un Host Hyper-V**, e **Hyper-V Host B**. Entrambi gli host dispongano di una singola scheda NIC fisica (pNIC) installato, e le schede di rete sono connesse a una parte superiore del rack \(ToR\) commutatore fisico. Inoltre, l'host si trovano nella stessa subnet, ovvero 192.168.1.x/24.

![Host Hyper-V](../../media/Converged-NIC/1-single-test-conn.jpg)


## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Passaggio 1. Testare la connettività tra origine e destinazione

Assicurarsi che fisico NIC possa connettersi all'host di destinazione. Questo test dimostra la connettività con Livello3 \(L3\) - o il livello IP -, nonché di livello 2 \(L2\).

1. Visualizzare le proprietà scheda di rete.

   ```PowerShell
   Get-NetAdapter
   ```
   
   _**Risultati:**_  

   |Nome|InterfaceDescription|ifIndex|Stato|MacAddress|LinkSpeed|
   |-----|--------------------|-------|-----|----------|---------|
   |M1|Mellanox ConnectX-3 Pro...| 4| Su|7C-FE-90-93-8F-A1|40 Gbps|
   ---

2. Visualizzare le proprietà di adapter aggiuntivi, tra cui l'indirizzo IP.

   ```PowerShell
   Get-NetAdapter M1 | fl *
   ```

   _**Risultati:**_

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

## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Passaggio 2. Assicurarsi che possano comunicare l'origine e destinazione

In questo passaggio si usa la **Test-NetConnection** comando Windows PowerShell, ma se è possibile utilizzare il **ping** comando se si preferisce. 

>[!TIP]
>Se si è certi che gli host possono comunicare tra loro, è possibile ignorare questo passaggio.

1. Verificare la comunicazione bidirezionale.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```
   
   _**Risultati:**_

   |Parametro|Value|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|M1|
   |SourceAddress|192.168.1.3|
   |PingSucceeded|True|
   |PingReplyDetails \(RTT\)|0 ms|
   ---
   
   In alcuni casi, potrebbe essere necessario disabilitare Windows Firewall con sicurezza avanzata per eseguire correttamente questo test. Se si disabilita il firewall, infatti, sicurezza e assicurarsi che la configurazione soddisfi i requisiti di sicurezza dell'organizzazione.

2. Disabilitare tutti i profili del firewall.

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```
    
3. Dopo aver disabilitato i profili firewall, verificare nuovamente la connessione. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Risultati:**_

   |Parametro|Value|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|Test-40G-1|
   |SourceAddress|192.168.1.3|
   |PingSucceeded|False|
   |PingReplyDetails \(RTT\)|0 ms|
   ---



## <a name="step-3-optional-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Passaggio 3. (Facoltativo) Configurare gli ID VLAN per schede di rete installati negli host Hyper-V

Molte configurazioni di rete si avvalgono delle VLAN e se si prevede di usare reti VLAN in una rete, è necessario ripetere il test precedente con le VLAN configurate. Inoltre, se si prevede di usare RoCE per i servizi RDMA è necessario abilitare le VLAN.

Per questo passaggio, le schede di rete presenti **accesso** modalità. Tuttavia, quando si crea un commutatore virtuale Hyper-V \(vSwitch\) più avanti in questa Guida, le proprietà VLAN vengono applicate a livello della porta commutatore virtuale. 

Poiché un interruttore può ospitare più VLAN, è necessario per il Top of Rack \(ToR\) commutatore fisico per avere la porta che l'host è connesso da configurati in modalità Trunk.

>[!NOTE]
>Per istruzioni su come configurare la modalità Trunk nel commutatore, consultare la documentazione del commutatore ToR.

L'immagine seguente mostra due host Hyper-V, ognuna con una scheda di rete fisica e ogni configurato per comunicare sulla rete VLAN 101.

![Configurare le reti locali virtuali](../../media/Converged-NIC/2-single-configure-vlans.jpg)


>[!IMPORTANT]
>Eseguire questa operazione nel server locale e di destinazione. Se il server di destinazione non è configurato con lo stesso ID VLAN nel server locale, i due non possono comunicare.


1. Configurare l'ID VLAN per schede di rete installati negli host Hyper-V.

   >[!IMPORTANT]
   >Non eseguire questo comando se si è connessi all'host in modalità remota su questa interfaccia, perché ciò comporta una perdita di accesso all'host.
    
   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
   ```

   _**Risultati:**_

   |Nome |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
   |----|-----------|------------|---------------|-------------|
   |M1|ID VLAN|101|VlanID|{101}|
   ---

2. Riavviare la scheda di rete per applicare il ID. VLAN

   ```PowerShell
   Restart-NetAdapter -Name "M1"
   ```

3. Verificare che lo stato è **backup**.

   ```PowerShell
   Get-NetAdapter -Name "M1"
   ```
   
   _**Risultati:**_

   |Nome|InterfaceDescription|ifIndex| Stato|MacAddress|LinkSpeed|
   |----|--------------------|-------|------|----------| ---------|
   |M1|Ada Ethernet Pro Mellanox ConnectX-3...|4|Su|7C-FE-90-93-8F-A1|40 Gbps|
   ---

   >[!IMPORTANT]
   >Potrebbe richiedere alcuni secondi per il dispositivo per riavviare e diventano disponibili nella rete. 

4. Verificare la connettività.<p>Se si verifica un errore di connettività, esaminare il commutatore partecipazione VLAN di configurazione o di destinazione nella stessa VLAN. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```
    
## <a name="step-4-configure-quality-of-service-qos"></a>Passaggio 4. Configurare la qualità del servizio \(QoS\)

>[!NOTE]
>È necessario eseguire tutti i passaggi di configurazione Data Center Bridging e QoS seguenti in tutti gli host che devono comunicare tra loro.

1. Installare Data Center Bridging \(DCB\) in ognuno degli host Hyper-V.

   - **Facoltativo** le configurazioni di rete che utilizzano iWarp per i servizi RDMA.
   - **Obbligatorio** per le configurazioni di rete che usano RoCE \(qualsiasi versione\) per i servizi RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

2. Impostare i criteri QoS per SMB diretto:

   - **Facoltativo** le configurazioni di rete che utilizzano iWarp.
   - **Obbligatorio** le configurazioni di rete che utilizzano RoCE.
   
   Nel comando riportato di seguito viene riportato di seguito, il valore "3" è arbitrario. È possibile usare qualsiasi valore compreso tra 1 e 7, purché si utilizzino in modo coerente lo stesso valore in tutta la configurazione dei criteri QoS.

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**Risultati:**_

   |Parametro|Value|
   |---------|-----|
   |Nome |SMB|
   |Proprietario|Criteri di gruppo \(macchina\)|
   |NetworkProfile|Tutte|
   |Precedenza|127|
   |JobObject|&nbsp;| 
   |NetDirectPort|445 |
   |PriorityValue|3 |
 ---

3. Per le distribuzioni RoCE, attivare **controllo di flusso priorità** per il traffico SMB, che non è necessaria per iWarp.

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**Risultati:**_

   |Priority|Enabled|PolicySet|IfIndex|IfAlias|
   |---------|-----|--------- |-------| -------|
   |0 |False |Globale|&nbsp;|&nbsp;|
   |1 |False |Globale|&nbsp;|&nbsp;|
   |2 |False |Globale|&nbsp;|&nbsp;|
   |3 |True  |Globale|&nbsp;|&nbsp;|
   |4 |False |Globale|&nbsp;|&nbsp;|
   |5 |False |Globale|&nbsp;|&nbsp;|
   |6 |False |Globale|&nbsp;|&nbsp;|
   |7 |False |Globale|&nbsp;|&nbsp;|
   ---

4. Abilitare QoS per le schede di rete locale e di destinazione.

   - **Non è necessario** le configurazioni di rete che utilizzano iWarp.
   - **Obbligatorio** le configurazioni di rete che utilizzano RoCE.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "M1"
   Get-NetAdapterQos -Name "M1"
   ```

   _**Risultati:**_

   **Nome**: M1  
   **Enabled**: True  

   _**Funzionalità:**_   

   |Parametro|Hardware|Corrente|
   |---------|--------|-------|
   |MacSecBypass|NotSupported|NotSupported|
   |DcbxSupport|Nessuno|Nessuno|
   |NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
   ---
 
   _**OperationalTrafficClasses:**_ 

   |TC|TSA|Bandwidth|Priorità|
   |----|-----|--------|-------|
   |0| ETS|70%|0-2,4-7|
   |1|ETS|30%|3 |
   ---

   _**OperationalFlowControl:**_  
   
   Con priorità 3 abilitata  

   _**OperationalClassifications:**_  

   |Protocollo|Porta/tipo|Priority|
   |--------|---------|--------|
   |Impostazione predefinita|&nbsp;|0|
   |NetDirect| 445|3|
   ---

5. Riserva una percentuale della larghezza di banda per SMB diretto \(RDMA\).

    In questo esempio viene utilizzata una prenotazione della larghezza di banda del 30%. È necessario selezionare un valore che rappresenta ciò che si prevede che il traffico di archiviazione richiede. 

   ```PowerShell
   New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS
   ```

   _**Risultati:**_

   |Nome|Algoritmo |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |SMB | ETS     | 30 |3 |Globale |&nbsp;|&nbsp;|
   ---                                      

6. Visualizzare le impostazioni di prenotazione della larghezza di banda.  

   ```PowerShell
   Get-NetQosTrafficClass
   ```

   _**Risultati:**_
 
   |Nome|Algoritmo |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |[Predefinito]|ETS|70 |0-2,4-7| Globale|&nbsp;|&nbsp;| 
   |SMB      |ETS|30 |3 |Globale|&nbsp;|&nbsp;| 
   ---

## <a name="step-5-optional-resolve-the-mellanox-adapter-debugger-conflict"></a>Passaggio 5. (Facoltativo) Risolvere il conflitto di debugger scheda Mellanox 

Per impostazione predefinita, quando si usa una scheda Mellanox, il debugger collegato Blocca NetQos, che è un problema noto. Pertanto, se si utilizza un adapter da Mellanox e si prevede di collegare un debugger, usare seguente comando risolvono questo problema. Questo passaggio non è obbligatorio se non si prevede di collegare un debugger o se non si usa una scheda Mellanox.

   ```PowerShell    
   Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
   ``` 

## <a name="step-6-verify-the-rdma-configuration-native-host"></a>Passaggio 6. Verificare la configurazione di RDMA (host nativo)

Si desidera assicurarsi che l'infrastruttura sia configurato correttamente prima della creazione di un commutatore virtuale e la transizione per RDMA (interfaccia di rete con convergenza). 

L'immagine seguente mostra lo stato corrente degli host Hyper-V.

![Test RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

1. Verificare la configurazione di RDMA.

   ```PowerShell
   Get-NetAdapterRdma
   ```
   _**Risultati:**_

   |Nome |InterfaceDescription |Enabled|
   |----|--------------------|-------|
   |M1| Scheda Ethernet Pro Mellanox ConnectX-3 |True|
   ---

2. Determinare il **ifIndex** valore l'adattatore di destinazione.<p>È consigliabile usare questo valore nei passaggi successivi quando si esegue lo script scaricato.

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**Risultati:**_ 

   |InterfaceAlias |InterfaceIndex |Indirizzo IPv4|
   |-------------- |-------------- |-----------|
   |M2 |14 |{192.168.1.5}|
   ---

3. Scaricare il [DiskSpd.exe utilità](https://aka.ms/diskspd) e decomprimerlo in c:\test.\.

4. Scaricare il [script di powershell Test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) in una cartella di test nell'unità locale, ad esempio, c:\test.\.

5. Eseguire la **Test-Rdma.ps1** script di PowerShell per passare il valore di ifIndex allo script, con l'indirizzo IP della scheda remota nella stessa VLAN.<p>In questo esempio, lo script passa il **ifIndex** pari a 14 sull'indirizzo IP della scheda di rete remoto 192.168.1.5.
   
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
   >Se il traffico RDMA non riesce, nel caso di RoCE in particolare, consultare la configurazione di ToR Switch per le impostazioni di base alla priorità/ETS appropriate che dovrebbero corrispondere le impostazioni dell'Host. Vedere la sezione QoS in questo documento per i valori di riferimento.

## <a name="step-7-remove-the-access-vlan-setting"></a>Passaggio 7. Rimuovere l'impostazione di accesso VLAN

Impostare in preparazione per la creazione di Hyper-V, è necessario rimuovere le impostazioni VLAN che è stato installato in precedenza.  

1. Rimuovere l'impostazione di accesso VLAN dall'interfaccia di rete fisica per impedire l'assegnazione automatica di tag il traffico in uscita con l'ID di VLAN non corretto. l'interfaccia di rete<p>Rimuovere questa impostazione impedisce inoltre il filtraggio del traffico in ingresso corrisponde all'ID accesso VLAN.
    
   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
   ```    

2. Verificare che il **VlanID impostazione** Mostra il valore di ID VLAN pari a zero.

   ```PowerShell    
   Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
   ```  


## <a name="step-8-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Passaggio 8. Creare un commutatore virtuale Hyper-V negli host Hyper-V

L'immagine seguente mostra Hyper-V Host 1 con un commutatore virtuale.

![Creare un commutatore virtuale Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


1. Creare un commutatore virtuale Hyper-V esterno in Hyper-V in Host Hyper-V A. <p>In questo esempio, l'opzione è denominata VMSTEST. Inoltre, il parametro **AllowManagementOS** crea una scheda vNIC host che eredita gli indirizzi MAC e IP della scheda di rete fisica.

   ```PowerShell
   New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true
   ```
   _**Risultati:**_

   |Nome |SwitchType |NetAdapterInterfaceDescription|
   |---- |---------- |------------------------------|
   |VMSTEST |Esterno |Scheda Ethernet Pro Mellanox ConnectX-3|
   ---

2. Visualizzare le proprietà scheda di rete.

   ```PowerShell
   Get-NetAdapter | ft -AutoSize
   ```

   _**Risultati:**_

   |Nome |InterfaceDescription | ifIndex |Stato |MacAddress |LinkSpeed|
   |---- |-------------------- |-------| ------|----------|---------|
   |vEthernet \(VMSTEST\) |Adattatore Ethernet virtuale Hyper-V #2|27 |Su |E4-1D-2D-07-40-71 |40 Gbps|
   ---

3. Gestire la scheda di rete virtuale host in uno dei due modi. 

   - **NetAdapter** vista opera in base al "vEthernet \(VMSTEST\)" name. Non tutte le proprietà di scheda di rete visualizzato in questa visualizzazione.
   - **VMNetworkAdapter** vista Elimina il prefisso "vEthernet" e utilizza semplicemente il nome del commutatore virtuale. (Consigliato) 

   ```PowerShell
   Get-VMNetworkAdapter –ManagementOS | ft -AutoSize
   ```

   _**Risultati:**_

   |Nome |IsManagementOs |VMName |SwitchName |MacAddress |Stato |IPAddresses|
   |----|-------------- |------ |----------|----------|------ |-----------|
   |CORP-External-opzione |True |CORP-External-opzione| 001B785768AA |{Ok} |&nbsp;|
   |VMSTEST |True |VMSTEST | E41D2D074071| {Ok} | &nbsp;| 
   ---

4. Testare la connessione.

   ```Powershell    
   Test-NetConnection 192.168.1.5
   ```

   _**Risultati:**_ 

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

5. Assegnare e visualizzare le impostazioni della scheda di rete VLAN.
    
   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```    

   _**Risultati:**_

   |VMName |VMNetworkAdapterName |Modalità |VlanList|
   |------ |-------------------- |---- |--------|
   |&nbsp;|VMSTEST |Accesso |101   |
   ---  
 
6. Testare la connessione.<p>Potrebbe richiedere alcuni secondi per il completamento prima che è possibile effettuare correttamente il ping di altri adapter.  

   ```PowerShell    
   Test-NetConnection 192.168.1.5
   ```

   _**Risultati:**_
     
   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-9-test-hyper-v-virtual-switch-rdma-mode-2"></a>Passaggio 9: Testare il commutatore virtuale Hyper-V RDMA (modalità 2)

L'immagine seguente visualizza lo stato corrente degli host Hyper-V, tra cui il commutatore virtuale Hyper-V Host 1.

![Testare il commutatore virtuale Hyper-V](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


1. Impostare la priorità l'assegnazione di tag su vNIC host.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
   ```  
   
   _**Risultati:**_

    Nome: IeeePriorityTag VMSTEST: Attivato


2. Consente di visualizzare la scheda di rete informazioni RDMA. 

   ```PowerShell
   Get-NetAdapterRdma
   ```   

   _**Risultati:**_

   |Nome |InterfaceDescription |Enabled |
   |---- |-------------------- |-------|
   |vEthernet \(VMSTEST\)| Adattatore Ethernet virtuale Hyper-V #2|False|
   ---

   >[!NOTE]
   >Se il parametro **Enabled** ha il valore **False**, significa che RDMA non è abilitato.
    

3. Visualizzare le informazioni di scheda di rete.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Risultati:**_   
 
   |Nome |InterfaceDescription |ifIndex |Stato |MacAddress |LinkSpeed|
   |----|--------------------|-------|------|----------|---------|
   |vEthernet (VMSTEST)|Adattatore Ethernet virtuale Hyper-V #2|27|Su|E4-1D-2D-07-40-71|40 Gbps|
   ---


4. Abilitare RDMA sulle vNIC host.

   ```PowerShell
   Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   ```

   _**Risultati:**_

   |Nome |InterfaceDescription |Enabled |
   |---- |-------------------- |-------|
   |vEthernet \(VMSTEST\)| Adattatore Ethernet virtuale Hyper-V #2|True|
   ---

   >[!NOTE]
   >Se il parametro **Enabled** ha il valore **True**, significa che RDMA è abilitato.

5. Eseguire test di traffico RDMA.

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

L'ultima riga nell'output di questo "test di traffico RDMA riuscito: Il traffico RDMA è stato inviato a 192.168.1.5,"Mostra che è stato configurato correttamente interfaccia di rete convergente sulla propria scheda.

## <a name="related-topics"></a>Argomenti correlati
- [Configurazione di interfaccia di rete convergente le NIC](cnic-datacenter.md)
- [Configurazione di commutatore fisico per la scheda di rete convergente](cnic-app-switch-config.md)
- [Risoluzione dei problemi di configurazioni di interfaccia di rete convergente](cnic-app-troubleshoot.md)
