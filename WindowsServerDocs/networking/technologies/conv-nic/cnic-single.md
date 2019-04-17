---
title: Configurazione di rete convergente con una sola scheda di rete
description: In questo argomento vengono fornite istruzioni su come distribuire una scheda di rete convergente con una sola scheda di rete in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d6663a966026afb301a4bad90a9573d16fc82875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>Configurazione di rete convergente con una sola scheda di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Le sezioni seguenti forniscono istruzioni per la configurazione di rete convergente con una singola scheda di rete nell'host Hyper-V.

La configurazione di esempio in questa guida illustra due host Hyper-V, **un Host Hyper-V**, e **Hyper-V Host B**.

## <a name="test-connectivity-between-source-and-destination"></a>Verificare la connettività tra origine e di destinazione

In questa sezione include i passaggi necessari per testare la connettività tra origine e destinazione host Hyper-V.

L'illustrazione seguente mostra due host Hyper-V, **un Host Hyper-V** e **Hyper-V Host B**. 

Entrambi i server hanno una singola scheda fisica (pNIC) installata e le schede NIC sono connessi a un commutatore top of rack \(ToR\) fisico. Inoltre, i server si trovano nella stessa subnet, ovvero 192.168.1.x/24.

![Host Hyper-V](../../media/Converged-NIC/1-single-test-conn.jpg)


### <a name="test-nic-connectivity-to-the-hyper-v-virtual-switch"></a>Testare la connettività di scheda di rete al commutatore virtuale Hyper\-V

Con questo passaggio, è possibile verificare che la scheda NIC fisica, per cui è in un secondo momento verrà creato un commutatore virtuale Hyper\-V, è possibile connettersi all'host di destinazione. 

Questo test dimostra la connettività tramite \(L3\) livello 3 - o il livello IP - nonché \(L2\) livello 2.

È possibile utilizzare il comando di Windows PowerShell seguente per ottenere le proprietà della scheda di rete.

    Get-NetAdapter
     
Di seguito sono risultati di esempio di questo comando.

|Nome|InterfaceDescription|ifIndex|Stato|Indirizzo MAC|LinkSpeed|
|-----|--------------------|-------|-----|----------|---------|
|
|M1|Pro Mellanox ConnectX-3...| 4| Backup|7C-FE-90-93-8F-A1|40 GB/s|

È possibile utilizzare uno dei comandi seguenti per ottenere le proprietà delle schede aggiuntive, inclusi l'indirizzo IP.

    Get-NetAdapter M1 | fl *

Di seguito sono esempio modificato risultati del comando.
    
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
    

### <a name="ensure-that-source-and-destination-can-communicate"></a>Assicurarsi che possa comunicare l'origine e destinazione

È possibile utilizzare questo passaggio per verificare la comunicazione bidirezionale \ (ping dall'origine alla destinazione e viceversa in entrambi systems\).  Nell'esempio seguente, il **Test NetConnection** viene utilizzato il comando di Windows PowerShell, ma se preferisci che è possibile utilizzare il **ping** comando. 

>[!NOTE]
>Se si è certi che gli host possono comunicare tra loro, è possibile ignorare questo passaggio.

    Test-NetConnection 192.168.1.5

Di seguito sono risultati di esempio di questo comando.

|Parametro|Valore|
|---------|-----|
|
|ComputerName|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|M1|
|IndirizzoOrigine|192.168.1.3|
|PingSucceeded|True|
|PingReplyDetails \(RTT\)|0 ms|

In alcuni casi, potrebbe essere necessario disattivare Windows Firewall con sicurezza avanzata per eseguire correttamente questo test. Se si disabilita il firewall, tenere presente protezione e assicurarsi che la configurazione soddisfi i requisiti di sicurezza dell'organizzazione.

Il comando seguente consente di disabilitare tutti i profili firewall.

    Set-NetFirewallProfile -All -Enabled False
    
Dopo la disattivazione del firewall, è possibile utilizzare il comando seguente per verificare la connessione.

    Test-NetConnection 192.168.1.5

Di seguito sono risultati di esempio di questo comando.

|Parametro|Valore|
|---------|-----|
|
|ComputerName|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|Test-40G-1|
|IndirizzoOrigine|192.168.1.3|
|PingSucceeded|False|
|PingReplyDetails \(RTT\)|0 ms|

## <a name="configure-vlans-optional"></a>Configurare le VLAN \(Optional\)

Diverse configurazioni di rete si avvale delle VLAN. Se si prevede di utilizzare VLAN della rete, è necessario ripetere il test precedente con VLAN configurata. \ (Se si prevede di utilizzare RoCE per i servizi RDMA è necessario abilitare le VLAN. \)

Per questo passaggio, le schede NIC sono **accesso** modalità. Tuttavia, quando si crea un commutatore virtuale Hyper-V di \(vSwitch\) più avanti in questa Guida, le proprietà VLAN vengono applicate a livello di porta vSwitch. 

Poiché un commutatore può ospitare più VLAN, è necessario per il commutatore fisico \(ToR\) Top of Rack di una porta che l'host è connesso a configurata in modalità Trunk.

>[!NOTE]
>Per istruzioni su come configurare la modalità Trunk nel commutatore, consultare la documentazione del commutatore ToR.

L'illustrazione seguente mostra due host Hyper-V, ognuna con una scheda di rete fisica, e ogni configurato per la comunicazione sulla VLAN 101.

![Configurare reti locali virtuali](../../media/Converged-NIC/2-single-configure-vlans.jpg)

### <a name="configure-the-vlan-id"></a>Configurare l'ID VLAN

Per configurare gli ID VLAN per le schede NIC installata nell'host Hyper-V, è possibile utilizzare questo passaggio.

#### <a name="configure-nic-m1"></a>Configurare M1 NIC

Con i comandi seguenti, configurare l'ID VLAN per la prima scheda NIC, M1, quindi visualizzare la configurazione risultante.

>[!IMPORTANT]
>Non eseguire questo comando se si è connessi all'host in modalità remota attraverso questa interfaccia, perché tale operazione genererà perdita dell'accesso all'host.
    
    Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
    Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
    
Di seguito sono risultati di esempio di questo comando.

|Nome |DisplayName| Disponibili DisplayValue| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|M1|ID VLAN|101|VlanID|{101}|


Assicurarsi che l'ID VLAN ha effetto indipendente dell'implementazione di scheda di rete utilizzando il comando seguente per riavviare la scheda di rete.

    Restart-NetAdapter -Name "M1"

È possibile utilizzare il comando seguente per verificare che lo stato di scheda di rete sia **backup** prima di procedere.

    Get-NetAdapter -Name "M1"

Di seguito sono risultati di esempio di questo comando.

|Nome|InterfaceDescription|ifIndex| Stato|Indirizzo MAC|LinkSpeed|
|----|--------------------|-------|------|----------| ---------|
|
|M1|Ada Ethernet Pro Mellanox ConnectX-3...|4|Backup|7C-FE-90-93-8F-A1|40 GB/s|

Assicurarsi di eseguire questo passaggio in server locali e di destinazione. Se il server di destinazione non è configurato con lo stesso ID VLAN del server locale, non possono comunicare le due.

### <a name="verify-connectivity"></a>Verificare la connettività

È possibile utilizzare questa sezione per verificare la connettività dopo le schede di rete vengono riavviate. È possibile verificare la connettività dopo l'applicazione di tag VLAN a entrambe le schede. Se si verifica un errore di connettività, è possibile esaminare il commutatore partecipazione VLAN di configurazione o di destinazione nella stessa VLAN. 

>[!IMPORTANT]
>Dopo aver eseguito i passaggi nella sezione precedente, potrebbe richiedere alcuni secondi per il dispositivo per riavviare e diventano disponibili nella rete.

#### <a name="verify-connectivity-for-nic-test-40g-1"></a>Verificare la connettività per NIC Test-40G-1

Per verificare la connettività per la prima scheda di rete, è possibile eseguire il comando seguente.

    Test-NetConnection 192.168.1.5
    
## <a name="configure-data-center-bridging-dcb"></a>Configurazione Data Center Bridging \(DCB\)

Il passaggio successivo consiste nel configurare \(QoS\) DCB e Quality of Service, che è necessario innanzitutto installare la funzionalità di Windows Server 2016 DCB.

>[!NOTE]
>È necessario eseguire tutti i seguenti passaggi di configurazione Data Center Bridging e QoS in tutti i server allo scopo di comunicare tra loro.

### <a name="install-data-center-bridging-dcb"></a>Installazione di Data Center Bridging \(DCB\)

Per installare e abilitare DCB, è possibile utilizzare questo passaggio. 

>[!IMPORTANT]
>- Installazione e configurazione di DCB è **facoltativo** le configurazioni di rete che utilizzano iWarp per i servizi RDMA.
>- Installazione e configurazione di DCB è **necessari** le configurazioni di rete che utilizzano RoCE \(any version\) per i servizi RDMA.


È possibile utilizzare il comando seguente per installare DCB in tutti gli host Hyper-V. 

    Install-WindowsFeature Data-Center-Bridging

### <a name="set-the-qos-policies-for-smb-direct"></a>Impostare i criteri QoS per SMB diretto 

È possibile utilizzare il comando seguente per configurare i criteri QoS per SMB diretto.

>[!IMPORTANT]
>- Questo passaggio è facoltativo per le configurazioni di rete che utilizzano iWarp.
>- Questo passaggio è obbligatorio per le configurazioni di rete che utilizzano RoCE.
>- Nell'esempio di comando seguente, il valore "3" è arbitrario. È possibile utilizzare qualsiasi valore compreso tra 1 e 7, purché si usano in modo coerente lo stesso valore in tutta la configurazione dei criteri QoS.

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

Di seguito sono risultati di esempio di questo comando.

|Parametro|Valore|
|---------|-----|
|
|Nome |SMB|
|Proprietario|\(Machine\) criteri di gruppo|
|NetworkProfile|Tutti|
|Precedenza|127|
|JobObject|&nbsp;| 
|NetDirectPort|445
|PriorityValue|3

### <a name="for-roce-deployments-turn-on-priority-flow-control-for-smb-traffic"></a>Per RoCE distribuzioni attivare il controllo di flusso di priorità per il traffico SMB 

Se si utilizza RoCE per i servizi RDMA, è possibile utilizzare i comandi seguenti per abilitare il controllo di flusso SMB e visualizzare i risultati. Controllo di flusso priorità è obbligatorio per RoCE, ma non è necessario quando si utilizza iWarp.

    Enable-NetQosFlowControl -priority 3
    Get-NetQosFlowControl

Risultati di esempio di seguito sono riportati i **Get-NetQosFlowControl** comando.

|Priorità|Abilitato|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |False |Globale|&nbsp;|&nbsp;|
|1 |False |Globale|&nbsp;|&nbsp;|
|2 |False |Globale|&nbsp;|&nbsp;|
|3 |True  |Globale|&nbsp;|&nbsp;|
|4 |False |Globale|&nbsp;|&nbsp;|
|5 |False |Globale|&nbsp;|&nbsp;|
|6 |False |Globale|&nbsp;|&nbsp;|
|7 |False |Globale|&nbsp;|&nbsp;|

### <a name="enable-qos-for-the-local-and-destination-network-adapters"></a>Abilitare QoS per le schede di rete locali e di destinazione
Con questo passaggio è possibile abilitare DCB sulle schede di rete specifiche.

>[!IMPORTANT]
>-  Questo passaggio non è necessaria per le configurazioni di rete che utilizzano iWarp.
>-  Questo passaggio è obbligatorio per le configurazioni di rete che utilizzano RoCE.




#### <a name="enable-qos-for-nic-m1"></a>Abilitare QoS per M1 NIC

È possibile utilizzare i comandi seguenti per abilitare QoS e visualizzare i risultati della configurazione.

    Enable-NetAdapterQos -InterfaceAlias "M1"
    Get-NetAdapterQos -Name "M1"

Di seguito sono risultati di esempio di questo comando.

**Nome**: M1 **abilitato**: True **funzionalità**:   

|Parametro|Hardware|Corrente|
|---------|--------|-------|
|
|MacSecBypass|NotSupported|NotSupported|
|DcbxSupport|Nessuno|Nessuno|
|NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
 
**OperationalTrafficClasses**: 

|TC|TSA|Larghezza di banda|Priorità|
|----|-----|--------|-------|
|
|0| ETS|70%|0-2,4-7|
|1|ETS|30%|3

**OperationalFlowControl**: priorità 3 Enabled **OperationalClassifications**:

|Protocollo|Tipo di porta /|Priorità|
|--------|---------|--------|
|
|Impostazione predefinita|&nbsp;|0|
|NetDirect| 445|3|

### <a name="reserve-a-percentage-of-the-bandwidth-for-smb-direct-rdma"></a>Riservare una percentuale della larghezza di banda per \(RDMA\) SMB diretto

È possibile utilizzare il comando seguente per riservare una percentuale della larghezza di banda per SMB diretto.  

In questo esempio viene utilizzata una prenotazione della larghezza di banda 30%. È necessario selezionare un valore corrispondente a quello previsto che richiederà il traffico di archiviazione. Il valore di **- bandwidthpercentage** parametro deve essere un multiplo del 10%.

    New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS

Di seguito sono risultati di esempio di questo comando.

|Nome|Algoritmo |Bandwidth(%)| Priorità |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|SMB | ETS     | 30 |3 |Globale |&nbsp;|&nbsp;|                                      
 
È possibile utilizzare il comando seguente per visualizzare le informazioni di prenotazione della larghezza di banda.

    Get-NetQosTrafficClass

Di seguito sono risultati di esempio di questo comando.
 
|Nome|Algoritmo |Bandwidth(%)| Priorità |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[Predefinito]|ETS|70 |0-2,4-7| Globale|&nbsp;|&nbsp;| 
|SMB      |ETS|30 |3 |Globale|&nbsp;|&nbsp;| 

## <a name="remove-debugger-conflict-mellanox-adapter-only"></a>Rimuovere il conflitto Debugger \(Mellanox adapter only\)

Se usi un adattatore da è necessario eseguire questo passaggio per configurare il debugger Mellanox. Per impostazione predefinita, quando si utilizza una scheda Mellanox, il debugger collegato Blocca NetQos. È possibile utilizzare il comando seguente per sostituire il debugger.

    
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    

## <a name="test-rdma-native-host"></a>Test RDMA (host nativo)

È possibile utilizzare questo passaggio per assicurarsi che l'infrastruttura sia configurato correttamente prima della creazione di un vSwitch e passando a \(Converged NIC\) RDMA.

L'illustrazione seguente mostra lo stato corrente degli host Hyper-V.

![Test RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

Per verificare la configurazione di RDMA, è possibile eseguire il comando seguente.

    Get-NetAdapterRdma

Di seguito sono risultati di esempio di questo comando.

|Nome |InterfaceDescription |Abilitato|
|----|--------------------|-------|
|
|M1| Scheda Ethernet Pro Mellanox ConnectX-3 |True|

### <a name="download-diskspdexe-and-a-powershell-script"></a>Download di DiskSpd.exe e uno Script di PowerShell

Per continuare, è necessario prima scaricare gli elementi seguenti.

- Scaricare l'utilità DiskSpd.exe ed estrarre l'utilità in C:\TEST\ [utilità di Diskspd: un solido archiviazione Testing Tool (sostitutivi SQLIO)](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223)

- Scaricare lo script di powershell Test-RDMA a C:\TEST\[https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>Determinare il valore ifIndex destinazione della scheda di rete

È possibile utilizzare il comando seguente per individuare il valore ifIndex della scheda di destinazione. Quando si esegue lo script che hai scaricato, è possibile utilizzare questo valore nei passaggi successivi.

    Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

Di seguito sono risultati di esempio di questo comando.

|InterfaceAlias |InterfaceIndex |IPv4Address|
|-------------- |-------------- |-----------|
|
|M2 |14 |{192.168.1.5}|

### <a name="run-the-powershell-script"></a>Eseguire lo script PowerShell

Quando si esegue il ps1 Test-Rdma script di Windows PowerShell, è possibile passare il valore ifIndex dello script, con l'indirizzo IP della scheda remota nella stessa VLAN.

È possibile utilizzare il comando seguente per eseguire lo script con un ifIndex del 14 nella scheda di rete 192.168.1.5.
    
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
    

>[!NOTE]
>Se il traffico RDMA non riesce, il caso RoCE in particolare, consultare la configurazione ToR Switch per le impostazioni di base alla priorità/ETS appropriate che devono corrispondere alle impostazioni di Host. Vedere la sezione di QoS in questo documento per i valori di riferimento.

## <a name="remove-the-access-vlan-setting"></a>Rimuovere l'impostazione di accesso VLAN

In preparazione per la creazione del commutatore Hyper-V è necessario rimuovere le impostazioni VLAN che è stato installato in precedenza.  È possibile utilizzare il comando seguente per rimuovere l'impostazione di accesso VLAN dalla scheda di rete fisica. Questa azione impedisce l'assegnazione automatica di tag il traffico in uscita con l'ID VLAN non corretto la scheda di rete e impedisce inoltre filtrano il traffico in ingresso che non corrisponde l'ID della VLAN accesso.

    
    Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
    

È possibile utilizzare il comando seguente per verificare l'impostazione VlanID e visualizzare i risultati, che mostrano che il valore di ID VLAN è zero.

    
    Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
    


## <a name="create-a-hyper-v-virtual-switch"></a>Creare un commutatore virtuale Hyper-V

È possibile utilizzare questa sezione per creare un commutatore virtuale Hyper-V di \(vSwitch\) negli host Hyper-V.

L'illustrazione seguente mostra Hyper-V Host 1 con un vSwitch.

![Creare un commutatore virtuale Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


### <a name="create-an-external-hyper-v-virtual-switch"></a>Creare un commutatore virtuale Hyper-V esterno

È possibile utilizzare questa sezione per creare un vSwitch esterno in Hyper-V in Host Hyper-V A.

È possibile utilizzare il comando seguente per creare un commutatore denominato VMSTEST.

>[!NOTE]
>Il parametro **AllowManagementOS** nel comando seguente crea una scheda di rete Host che eredita l'indirizzo MAC e l'indirizzo IP della scheda di rete fisica.

    New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true

Di seguito sono risultati di esempio di questo comando.

|Nome |SwitchType |NetAdapterInterfaceDescription|
|---- |---------- |------------------------------|
|VMSTEST |Esterno |Scheda Ethernet Pro Mellanox ConnectX-3|

È possibile utilizzare il comando seguente per visualizzare le proprietà della scheda di rete.

    Get-NetAdapter | ft -AutoSize

Di seguito sono risultati di esempio di questo comando.

|Nome |InterfaceDescription | ifIndex |Stato |Indirizzo MAC |LinkSpeed|
|---- |-------------------- |-------| ------|----------|---------|
|
|vEthernet \(VMSTEST\) |Scheda Ethernet virtuale Hyper-V #2|27 |Backup |E4-1D-2D-07-40-71 |40 GB/s|


È possibile gestire una scheda di rete Host in due modi. È un metodo di **NetAdapter** visualizzazione, che opera in base al "vEthernet \(VMSTEST\)" nome.

L'altro metodo consiste di **VMNetworkAdapter** visualizzazione, che elimina il prefisso "vEthernet" e Usa semplicemente il nome del commutatore VM.

Il **VMNetworkAdapter** consente di visualizzare alcune proprietà di scheda di rete che non vengono visualizzate con il **NetAdapter** comando.

È possibile utilizzare il comando seguente per visualizzare i risultati del **VMNetworkAdapter** metodo.

    Get-VMNetworkAdapter –ManagementOS | ft -AutoSize

Di seguito sono risultati di esempio di questo comando.

|Nome |IsManagementOs |VMName |SwitchName |Indirizzo MAC |Stato |IPAddresses|
|----|-------------- |------ |----------|----------|------ |-----------|
|
|CORP-commutatore esterno |True |CORP-commutatore esterno| 001B785768AA |{Ok} |&nbsp;|
|VMSTEST |True |VMSTEST | E41D2D074071| {Ok} | &nbsp;| 

### <a name="test-the-connection"></a>Verificare la connessione

È possibile utilizzare il comando seguente per verificare la connessione e visualizzare i risultati.
    
    Test-NetConnection 192.168.1.5

    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
È possibile utilizzare i seguenti comandi di esempio per assegnare e visualizzare le impostazioni VLAN scheda di rete.
    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
    Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
    

|VMName |VMNetworkAdapterName |Modalità |VlanList|
|------ |-------------------- |---- |--------|
|
|&nbsp;|VMSTEST |Accesso |101     
 
### <a name="test-the-connection"></a>Verificare la connessione

Ricorda che la modifica potrebbe richiedere alcuni secondi completare prima di è possibile effettuare correttamente il ping l'altra scheda.

È possibile utilizzare il comando seguente per verificare la connessione e visualizzare i risultati.
    
    Test-NetConnection 192.168.1.5
     
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    

## <a name="test-hyper-v-virtual-switch-rdma-mode-2"></a>Testare il commutatore virtuale Hyper-V RDMA (modalità 2)

L'illustrazione seguente mostra lo stato corrente i host Hyper-V, inclusi il vSwitch su Hyper-V Host 1.

![Testare il commutatore virtuale Hyper-V](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


### <a name="set-priority-tagging-on-the-host-vnic"></a>Impostare la priorità di assegnazione di tag vNIC Host

È possibile utilizzare i seguenti comandi di esempio per impostare la priorità su vNIC Host la codifica e visualizzare i risultati delle azioni.
    
    Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
    
    Name: VMSTEST
    IeeePriorityTag : On
    
È possibile utilizzare il comando seguente per visualizzare la scheda di rete informazioni RDMA. Nei risultati, quando il parametro **abilitato** ha il valore **False**, significa che non sia abilitato RDMA.
    
    Get-NetAdapterRdma
    
|Nome |InterfaceDescription |Abilitato |
|---- |-------------------- |-------|
|
|vEthernet \(VMSTEST\)| Scheda Ethernet virtuale Hyper-V #2|False|

### <a name="enable-rdma-on-the-host-vnic"></a>Abilitare RDMA sulle vNIC Host

È possibile utilizzare i seguenti comandi di esempio per visualizzare le proprietà di scheda di rete, abilitare RDMA per la scheda e quindi visualizzare la scheda di rete informazioni RDMA.
    
    Get-NetAdapter
    
|Nome |InterfaceDescription |ifIndex |Stato |Indirizzo MAC |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|vEthernet (VMSTEST)|Scheda Ethernet virtuale Hyper-V #2|27|Backup|E4-1D-2D-07-40-71|40 GB/s|

    Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
    Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"

Nei risultati seguenti, quando il parametro **abilitato** ha il valore **True**, significa che è abilitato RDMA.

|Nome |InterfaceDescription |Abilitato |
|---- |-------------------- |-------|
|
|vEthernet \(VMSTEST\)| Scheda Ethernet virtuale Hyper-V #2|True|


    
    Get-NetAdapter 
    

|Nome |InterfaceDescription |ifIndex |Stato |Indirizzo MAC |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|vEthernet (VMSTEST)|Scheda Ethernet virtuale Hyper-V #2|27|Backup|E4-1D-2D-07-40-71|40 GB/s|

### <a name="perform-rdma-traffic-test-by-using-the-script"></a>Eseguire test di traffico RDMA mediante lo script

È possibile utilizzare il comando seguente per eseguire lo script che è stato scaricato e visualizzare i risultati.
    
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
    
La riga finale in questo output, "test traffico RDMA riuscite: traffico RDMA è stato inviato alla 192.168.1.5," Mostra che sia stato configurato correttamente convergente NIC dell'adattatore.

## <a name="all-topics-in-this-guide"></a>Tutti gli argomenti in questa Guida

Questa guida contiene gli argomenti seguenti.

- [Configurazione di rete convergente con una sola scheda di rete](cnic-single.md)
- [Configurazione di rete convergente le NIC combinate](cnic-datacenter.md)
- [Configurazione di commutatore fisico per scheda di rete convergente](cnic-app-switch-config.md)
- [Risoluzione dei problemi convergente configurazioni NIC](cnic-app-troubleshoot.md)
