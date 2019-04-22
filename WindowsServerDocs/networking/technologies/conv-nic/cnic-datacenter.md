---
title: Scheda di rete convergente in una configurazione di interfaccia di rete raggruppate (datacenter)
description: In questo argomento, Microsoft fornisce le istruzioni per distribuire l'interfaccia di rete convergente in una configurazione di interfaccia di rete raggruppate con Switch Embedded Teaming (SET).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/17/2018
ms.openlocfilehash: 5f99600e24c62da9bdf674897dbadde9246b7bb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821032"
---
# <a name="converged-nic-in-a-teamed-nic-configuration-datacenter"></a>Scheda di rete convergente in una configurazione di interfaccia di rete raggruppate (datacenter)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento, Microsoft fornisce le istruzioni per distribuire l'interfaccia di rete convergente in una configurazione di interfaccia di rete raggruppate con Switch Embedded Teaming \(impostare\). 

La configurazione di esempio in questo argomento descrive due host Hyper-V **Hyper-V Host 1** e **Hyper-V Host 2**. Entrambi gli host hanno due schede di rete. In ogni host, una scheda è connessa alla subnet 192.168.1.x/24 e una scheda è connessa alla subnet 192.168.2.x/24.

![Host Hyper-V](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Passaggio 1. Testare la connettività tra origine e destinazione

Assicurarsi che fisico NIC possa connettersi all'host di destinazione.  Questo test dimostra la connettività con Livello3 \(L3\) -il livello IP - o, nonché il livello 2 \(L2\) virtual local area network \(VLAN\).

1. Consente di visualizzare la prima proprietà scheda di rete.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```
  
   _**Risultati:**_

   |Nome|InterfaceDescription|ifIndex|Stato|MacAddress|LinkSpeed|
   |-----|--------------------|-------|-----|----------|---------|
   |Test-40G-1|Scheda Ethernet Pro Mellanox ConnectX-3|11|Su|E4-1D-2D-07-43-D0|40 Gbps|
   ---
   
2. Visualizzare proprietà aggiuntive per la prima scheda, incluso l'indirizzo IP. 

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-1"
   Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```
   
   _**Risultati:**_

   |Parametro|Value|
   |---------|-----|
   |IPAddress| 192.168.1.3|
   |InterfaceIndex|11|
   |InterfaceAlias|Test-40G-1|
   |AddressFamily|IPv4|
   |Tipo| Unicast|
   |PrefixLength|24|
   ---

3. Consente di visualizzare la seconda proprietà scheda di rete.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize
   ```
   
   _**Risultati:**_

   |Nome |InterfaceDescription |ifIndex |Stato |MacAddress |LinkSpeed|
   |----|--------------------|-------|------|----------|---------|
   |TEST-40G-2 |Mellanox ConnectX-3 Pro Ethernet A...#2 |13 |Su |E4-1D-2D-07-40-70 |40 Gbps|
   ---
   
4. Visualizzare proprietà aggiuntive per la seconda scheda, incluso l'indirizzo IP.

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-2"
   Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```
   
   _**Risultati:**_

   |Parametro|Value|
   |---------|-----|
   |IPAddress|192.168.2.3|
   |InterfaceIndex|13|
   |InterfaceAlias|TEST-40G-2|
   |AddressFamily|IPv4|
   |Tipo|Unicast|
   |PrefixLength|24|
   ---

5. Verificare che altri Team NIC o SET di membri pNICs disponga di un indirizzo IP valido.<p>Usare una subnet separata, \(xxx.xxx. **2**xxx.xxx vs xxx. **1**xxx\), per agevolare l'invio da questo adattatore di destinazione. In caso contrario, se entrambi pNICs si trova nella stessa subnet, consente di bilanciare il carico dello stack TCP/IP di Windows tra le interfacce e una convalida semplice diventa più complessa.


## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Passaggio 2. Assicurarsi che possano comunicare l'origine e destinazione

In questo passaggio si usa la **Test-NetConnection** comando Windows PowerShell, ma se è possibile utilizzare il **ping** comando se si preferisce. 

1. Verificare la comunicazione bidirezionale.

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
   
   
4. Verificare la connettività per le schede NIC aggiuntive. Ripetere i passaggi precedenti per tutti i successivi pNICs incluso nel team di interfaccia di rete o un SET.

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```
   
   _**Risultati:**_

   |Parametro|Value|
   |---------|-----|
   |ComputerName|192.168.2.5|
   |RemoteAddress|192.168.2.5|
   |InterfaceAlias|Test-40G-2|
   |SourceAddress|192.168.2.3|
   |PingSucceeded|False|
   |PingReplyDetails \(RTT\)|0 ms|
   ---

## <a name="step-3-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Passaggio 3. Configurare gli ID VLAN per schede di rete installati negli host Hyper-V

Molte configurazioni di rete si avvalgono delle VLAN e se si prevede di usare reti VLAN in una rete, è necessario ripetere il test precedente con le VLAN configurate.

Per questo passaggio, le schede di rete presenti **accesso** modalità. Tuttavia, quando si crea un commutatore virtuale Hyper-V \(vSwitch\) più avanti in questa Guida, le proprietà VLAN vengono applicate a livello della porta commutatore virtuale. 

Poiché un interruttore può ospitare più VLAN, è necessario per il Top of Rack \(ToR\) commutatore fisico per avere la porta che l'host è connesso da configurati in modalità Trunk.

>[!NOTE]
>Per istruzioni su come configurare la modalità Trunk nel commutatore, consultare la documentazione del commutatore ToR.

L'immagine seguente mostra due host Hyper-V con due schede di rete ogni che dispongono di VLAN 101 e 102 VLAN configurato nella proprietà scheda di rete.

![Configurare le reti locali virtuali](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)


>[!TIP]
>In base dell'Institute of Electrical and Electronics Engineers \(IEEE\) standard, la qualità del servizio di rete \(QoS\) proprietà nella scheda NIC fisica agire sull'intestazione 802.1P incorporato all'interno di 802.1Q \(VLAN\) intestazione quando si configura il ID. VLAN

1. Configurare l'ID VLAN nella scheda di rete prima, Test-40G-1.

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**Risultati:**_   

   |Nome |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
   |----|-----------|------------|---------------|-------------|
   |TEST-40G-1|ID VLAN|101|VlanID|{101}|
   ---

2. Riavviare la scheda di rete per applicare il ID. VLAN

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-1"
   ```
   
3. Verificare che lo stato è **backup**.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```
   
   _**Risultati:**_

   |Nome|InterfaceDescription|ifIndex| Stato|MacAddress|LinkSpeed|
   |----|--------------------|-------|------|----------| ---------|
   |Test-40G-1|Ada Ethernet Pro Mellanox ConnectX-3...|11|Su|E4-1D-2D-07-43-D0|40 Gbps|
   ---
    
4. Configurare l'ID VLAN nella scheda di rete secondo, Test-40G-2.

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ``` 

   _**Risultati:**_

   |Nome |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
   |----|-----------|------------|---------------|-------------|
   |TEST-40G-2|ID VLAN|102|VlanID|{102}|
   ---
   
5. Riavviare la scheda di rete per applicare il ID. VLAN

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-2" 
   ```
   
6. Verificare che lo stato è **backup**.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```
   
   _**Risultati:**_

   |Nome|InterfaceDescription|ifIndex| Stato|MacAddress|LinkSpeed|
   |----|--------------------|-------|------|----------| ---------|
   |Test-40G-2 |Ada Ethernet Pro Mellanox ConnectX-3... |11 |Su |E4-1D-2D-07-43-D1 |40 Gbps|
   ---

   >[!IMPORTANT]
   >Potrebbe richiedere alcuni secondi per il dispositivo per riavviare e diventano disponibili nella rete. 
   
7. Verificare la connettività per la prima interfaccia di rete, Test-40G-1.<p>Se si verifica un errore di connettività, esaminare il commutatore partecipazione VLAN di configurazione o di destinazione nella stessa VLAN. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Risultati:**_   

   |Parametro|Value|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|Test-40G-1|
   |SourceAddress|192.168.1.5|
   |PingSucceeded|True|
   |PingReplyDetails \(RTT\)|0 ms|
   ---
   
8. Verificare la connettività per la prima interfaccia di rete, Test-40G-2.<p>Se si verifica un errore di connettività, esaminare il commutatore partecipazione VLAN di configurazione o di destinazione nella stessa VLAN.

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**Risultati:**_    

   |Parametro|Value|
   |---------|-----|
   |ComputerName|192.168.2.5|
   |RemoteAddress|192.168.2.5|
   |InterfaceAlias|Test-40G-2|
   |SourceAddress|192.168.2.3|
   |PingSucceeded|True|
   |PingReplyDetails \(RTT\)|0 ms|
   ---
   
   >[!IMPORTANT]
   >Non è insolito per un **Test-NetConnection** o un errore di verificarsi immediatamente dopo l'esecuzione di ping **Restart-NetAdapter**.  Quindi attendere che la scheda di rete inizializza sempre completamente e quindi riprovare.
   >
   >Se le connessioni di rete VLAN 101 funzionare, ma non le connessioni 102 VLAN, il problema potrebbe essere che l'opzione deve essere configurato per consentire il traffico di porta nella VLAN desiderate. È possibile controllare questo temporaneamente impostando gli adapter di errore alla VLAN 101 e ripetere il test della connettività.

   
   L'immagine seguente mostra gli host Hyper-V dopo aver configurato correttamente le VLAN.

   ![Configurare la qualità del servizio](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)
   
   
## <a name="step-4-configure-quality-of-service-qos"></a>Passaggio 4. Configurare la qualità del servizio \(QoS\)

>[!NOTE]
>È necessario eseguire tutti i passaggi di configurazione Data Center Bridging e QoS seguenti in tutti gli host che devono comunicare tra loro.

1. Installare Data Center Bridging \(DCB\) in ognuno degli host Hyper-V.

   - **Facoltativo** le configurazioni di rete che utilizzano iWarp.
   - **Obbligatorio** per le configurazioni di rete che usano RoCE \(qualsiasi versione\) per i servizi RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

   _**Risultati:**_

   |Riuscito |Riavvio necessario |Codice di chiusura|Risultato di funzionalità|
   |------- |-------------- |--------- |-------------- |
   |True |No |Riuscito| {Data Center Bridging}|
   ---
   
2. Impostare i criteri QoS per SMB diretto:

   - **Facoltativo** le configurazioni di rete che utilizzano iWarp.
   - **Obbligatorio** per le configurazioni di rete che usano RoCE \(qualsiasi versione\) per i servizi RDMA.
   
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
   |NetDirectPort|445
   |PriorityValue|3
   ---

3. Impostare altri criteri di QoS per il resto del traffico sull'interfaccia.   

   ```PowerShell
   New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
   ```

   _**Risultati:**_   

   |Parametro|Value|
   |---------|-----|
   |Nome | DEFAULT|
   |Proprietario|Criteri di gruppo \(macchina\)|
   |NetworkProfile|Tutte|
   |Precedenza|127|
   |Modello| Impostazione predefinita|
   |JobObject| &nbsp;|
   |PriorityValue|0|
   ---
   
4. Accendere **controllo di flusso priorità** per il traffico SMB, che non è necessaria per iWarp.

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
   |3 |True |Globale|&nbsp;|&nbsp;|
   |4 |False |Globale|&nbsp;|&nbsp;|
   |5 |False |Globale|&nbsp;|&nbsp;|
   |6 |False |Globale|&nbsp;|&nbsp;|
   |7 |False |Globale|&nbsp;|&nbsp;|
   ---
   
   >**IMPORTANTI** se i risultati non corrispondono a questi risultati perché elementi diversi dal 3 hanno un valore Enabled True, è necessario disabilitare **controllo di flusso** per queste classi.
   >
   >```PowerShell
   >Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
   >Get-NetQosFlowControl
   >```
   >Nelle configurazioni più complesse, le altre classi di traffico potrebbero richiedere il controllo di flusso, tuttavia questi scenari sono all'esterno dell'ambito di questa Guida.


5. Abilitare QoS per la prima interfaccia di rete, Test-40G-1.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
   Get-NetAdapterQos -Name "Test-40G-1"

   Name: TEST-40G-1 
   Enabled: True
   ```
   _**Funzionalità**:_   

   |Parametro|Hardware|Corrente|
   |---------|--------|-------|
   |MacSecBypass|NotSupported|NotSupported|
   |DcbxSupport|Nessuno|Nessuno|
   |NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
   ---
 
   _**OperationalTrafficClasses**:_    

   |TC|TSA|Bandwidth|Priorità|
   |----|-----|--------|-------|
   |0| Completa|&nbsp;|0-7|
   ---

   _**OperationalFlowControl**:_  

   Con priorità 3 abilitata  

   _**OperationalClassifications**:_  

   |Protocollo|Porta/tipo|Priority|
   |--------|---------|--------|
   |Impostazione predefinita|&nbsp;|0|
   |NetDirect| 445|3|
   ---
   
6. Abilitare QoS per la seconda NIC, Test-40G-2.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
   Get-NetAdapterQos -Name "Test-40G-2"

   Name: TEST-40G-2 
   Enabled: True 
   ```

   _**Funzionalità**:_ 

   |Parametro|Hardware|Corrente|
   |---------|--------|-------|
   |MacSecBypass|NotSupported|NotSupported|
   |DcbxSupport|Nessuno|Nessuno|
   |NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
   ---

   _**OperationalTrafficClasses**:_  

   |TC|TSA|Bandwidth|Priorità|
   |----|-----|--------|-------|
   |0| Completa|&nbsp;|0-7|
   ---
   
    _**OperationalFlowControl**:_  

    Con priorità 3 abilitata  
   
   _**OperationalClassifications**:_  

   |Protocollo|Porta/tipo|Priority|
   |--------|---------|--------|
   |Impostazione predefinita|&nbsp;|0|
   |NetDirect| 445|3|
   ---

   
7. Riservare la metà della larghezza di banda per SMB diretto \(RDMA\)

   ```PowerShell
   New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS
   ```
   
   _**Risultati:**_  
   
   |Nome|Algoritmo |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |SMB | ETS     | 50 |3 |Globale |&nbsp;|&nbsp;|   
   ---

8. Visualizzare le impostazioni di prenotazione della larghezza di banda:   

   ```PowerShell
   Get-NetQosTrafficClass | ft -AutoSize
   ```
   
   _**Risultati:**_  

   |Nome|Algoritmo |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |[Predefinito]| ETS|50 |0-2,4-7|  Globale|&nbsp;|&nbsp;| 
   |SMB |ETS|50 |3 |Globale|&nbsp;|&nbsp;| 
   ---
   
9. (Facoltativo) Creare due classi di traffico aggiuntivo per il traffico IP del tenant. 

   >[!TIP]
   >È possibile omettere i valori "IP1" e "IP2".

   ```PowerShell
   New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS
   ```
   
   _**Risultati:**_
   
   |Nome|Algoritmo |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |IP1 |ETS |10 |1 |Globale|&nbsp;|&nbsp;|
   ---
   
   ```PowerShell
   New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS
   ```
   
   _**Risultati:**_

   |Nome|Algoritmo |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |IP2 |ETS |10 |2 |Globale|&nbsp;|&nbsp;|
   ---
   
10. Consente di visualizzare le classi di traffico di QoS.

    ```PowerShell
    Get-NetQosTrafficClass | ft -AutoSize
    ```
    
    _**Risultati:**_

    |Nome|Algoritmo |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
    |----|---------| ------------ |--------| ---------|------- |------- |
    |[Predefinito] |ETS |30 |0,4-7 |Globale|&nbsp;|&nbsp;|
    |SMB |ETS |50 |3 |Globale|&nbsp;|&nbsp;|
    |IP1 |ETS |10 |1 |Globale|&nbsp;|&nbsp;|
    |IP2 |ETS |10 |2 |Globale|&nbsp;|&nbsp;|
    ---
   
11. (Facoltativo) Eseguire l'override del debugger.<p>Per impostazione predefinita, il debugger collegato Blocca NetQos. 

    ```PowerShell
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger
    ```

    _**Risultati:**_  

    ```
    AllowFlowControlUnderDebugger
    -----------------------------
    1
    ```

## <a name="step-5-verify-the-rdma-configuration-mode-1"></a>Passaggio 5. Verificare la configurazione di RDMA \(modalità 1\) 

Per assicurarsi che l'infrastruttura sia configurato correttamente prima della creazione di un commutatore virtuale e la transizione a RDMA \(modalità 2\).

L'immagine seguente mostra lo stato corrente degli host Hyper-V.

![Test RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)


1. Verificare la configurazione di RDMA.

   ```PowerShell
   Get-NetAdapterRdma | ft -AutoSize
   ```
   
   _**Risultati:**_

   |Nome |InterfaceDescription |Enabled|
   |----|--------------------|-------|
   |TEST-40G-1| Scheda Mellanox ConnectX-4 VPI #2 |True|
   |TEST-40G-2| Adapter VPI Mellanox ConnectX-4 |True|
   ---

2. Determinare il **ifIndex** valore delle schede di destinazione.<p>È consigliabile usare questo valore nei passaggi successivi quando si esegue lo script scaricato.   

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```
   
   _**Risultati:**_

   |InterfaceAlias |InterfaceIndex |Indirizzo IPv4|
   |-------------- |-------------- |-----------|
   |TEST-40G-1 |14 |{192.168.1.3}|
   |TEST-40G-2 | 13 |{192.168.2.3}|
   ---
   
3. Scaricare il [DiskSpd.exe utilità](https://aka.ms/diskspd) e decomprimerlo in c:\test.\.

4. Scaricare il [script di PowerShell Test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) in una cartella di test nell'unità locale, ad esempio, c:\test.\.

5. Eseguire la **Test-Rdma.ps1** script di PowerShell per passare il valore di ifIndex allo script, con l'indirizzo IP dell'adattatore prima remoto nella stessa VLAN.<p>In questo esempio, lo script passa il **ifIndex** pari a 14 sull'indirizzo IP della scheda di rete remoto 192.168.1.5.
   
   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Risultati:**_ 
   
   ```   
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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

6. Eseguire la **Test-Rdma.ps1** script di PowerShell per passare il valore di ifIndex allo script, con l'indirizzo IP della seconda scheda remota nella stessa VLAN.<p>In questo esempio, lo script passa il **ifIndex** valore pari a 13 sull'indirizzo IP della scheda di rete remoto 192.168.2.5.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Risultati:**_ 
   
   ```   
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter TEST-40G-2 is a physical adapter
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
   VERBOSE: Remote IP 192.168.2.5 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 541185606 RDMA bytes written per second
   VERBOSE: 34821478 RDMA bytes sent per second
   VERBOSE: 954717307 RDMA bytes written per second
   VERBOSE: 35040816 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
   ``` 

## <a name="step-6-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Passaggio 6. Creare un commutatore virtuale Hyper-V negli host Hyper-V


L'immagine seguente mostra Hyper-V Host 1 con un commutatore virtuale.

![Creare un commutatore virtuale Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

1. Creare un commutatore virtuale in modalità SET nell'host Hyper-V 1.

   ```PowerShell
   New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
   ```
   
   _**Risultato:**_

   |Nome |SwitchType |NetAdapterInterfaceDescription|
   |---- |---------- |------------------------------|
   |VMSTEST |Esterno |Interfaccia raggruppate|
   ---
   
2. Visualizzare il gruppo di schede fisiche nel SET.

   ```PowerShell
   Get-VMSwitchTeam -Name "VMSTEST" | fl
   ```
   
   _**Risultati:**_  
   
   ```
   Name: VMSTEST  
   Id: ad9bb542-dda2-4450-a00e-f96d44bdfbec  
   NetAdapterInterfaceDescription: {Mellanox ConnectX-3 Pro Ethernet Adapter, Mellanox ConnectX-3 Pro Ethernet Adapter #2}  
   TeamingMode: SwitchIndependent  
   LoadBalancingAlgorithm: Dynamic   
   ```
   
3. Visualizzare le due visualizzazioni di vNIC host

   ```PowerShell
    Get-NetAdapter
   ```
   
   _**Risultati:**_

   |Nome |InterfaceDescription |ifIndex |Stato |MacAddress |LinkSpeed|
   |---- |--------------------|-------|------|----------|---------|
   |vEthernet (VMSTEST)|Adattatore Ethernet virtuale Hyper-V #2 |28 |Su|E4-1D-2D-07-40-71|80 GB/s|
   ---
   
4. Visualizzare proprietà aggiuntive di vNIC host. 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**Risultati:**_

   |Nome |IsManagementOs |VMName |SwitchName |MacAddress |Stato |IPAddresses|
   |----|--------------|------|----------|----------|------|-----------|
   |VMSTEST|True |VMSTEST |E41D2D074071| {Ok}|&nbsp;|
   ---
   

5. Testare la connessione di rete alla scheda di rete VLAN 101 remota.

   ```PowerShell
   Test-NetConnection 192.168.1.5 
   ```
   
   _**Risultati:**_  
   
   ```
   WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable
    
   ComputerName   : 192.168.1.5
   RemoteAddress  : 192.168.1.5
   InterfaceAlias : vEthernet (CORP-External-Switch)
   SourceAddress  : 10.199.48.170
   PingSucceeded  : False
   PingReplyDetails (RTT) : 0 ms
   ```
   
## <a name="step-7-remove-the-access-vlan-setting"></a>Passaggio 7. Rimuovere l'impostazione di accesso VLAN

In questo passaggio si rimuovere l'impostazione di accesso VLAN dall'interfaccia di rete fisica e impostare VLANID tramite il commutatore virtuale.

È necessario rimuovere l'impostazione di accesso VLAN per evitare entrambi assegnazione automatica di tag il traffico in uscita con l'ID VLAN non corretto e dall'applicazione del filtro in ingresso del traffico che non corrisponde l'ID accesso VLAN.

1. Rimuovere l'impostazione.
    
   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
   ```

2. Impostare l'ID VLAN.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```
   
   _**Risultati:**_  
   
   ```
   VMName VMNetworkAdapterName Mode   VlanList
   ------ -------------------- ----   --------
          VMSTEST              Access 101     
   ```
   
   
3. Testare la connessione di rete.

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

   >**IMPORTANTE** se i risultati non sono simili a risultati dell'esempio e ping non riesce con il messaggio "avviso: Il comando ping per 192.168.1.5 non riuscita stato: "DestinationHostUnreachable, verificare che il"vEthernet (VMSTEST)"con indirizzo IP appropriato.
   >
   >```PowerShell
   >Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
   >```
   >
   >Se l'indirizzo IP non è impostata, correggere il problema.
   >
   >```PowerShell
   >New-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)" -IPAddress 192.168.1.3 -PrefixLength 24
   >
   >IPAddress : 192.168.1.3
   >InterfaceIndex: 37
   >InterfaceAlias: vEthernet (VMSTEST)
   >AddressFamily : IPv4
   >Type  : Unicast
   >PrefixLength  : 24
   >PrefixOrigin  : Manual
   >SuffixOrigin  : Manual
   >AddressState  : Tentative
   >ValidLifetime : Infinite ([TimeSpan]::MaxValue)
   >PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
   >SkipAsSource  : False
   >PolicyStore   : ActiveStore
   >```  
   

4. Rinominare la gestione di interfaccia di rete.

   ```PowerShell
   Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**Risultati:**_ 
   
   |Nome |IsManagementOs |VMName |SwitchName |MacAddress |Stato |IPAddresses
   |----|--------------|------|----------|----------|------|-----------|
   |CORP-External-opzione |True |&nbsp;|CORP-External-opzione |001B785768AA |{Ok}|&nbsp;|
   |MGT |True |&nbsp;|VMSTEST |E41D2D074071 |{Ok}|&nbsp;|
   ---
   
5. Visualizzare proprietà aggiuntive di interfaccia di rete.

   ```PowerShell
   Get-NetAdapter
   ```
   
   _**Risultati:**_

   |Nome |InterfaceDescription |ifIndex |Stato |MacAddress |LinkSpeed|
   |----|--------------------|------|----------|---------|------|
   |vEthernet (MGT) |Adattatore Ethernet virtuale Hyper-V #2 |28 |Su | E4-1D-2D-07-40-71 |80 GB/s|
   ---
   
## <a name="step-8-test-hyper-v-vswitch-rdma"></a>Passaggio 8. Commutatore virtuale Hyper-V test RDMA

L'immagine seguente mostra lo stato corrente degli host Hyper-V, tra cui il commutatore virtuale Hyper-V Host 1.

![Testare il commutatore virtuale Hyper-V](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

1. Impostare la priorità l'assegnazione di tag su vNIC Host per integrare le impostazioni VLAN precedenti.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
   ```
   
   _**Risultati:**_  
      
   nome: MGT  
   IeeePriorityTag:  Attivato  
    
2. Creare due Vnic dell'host per RDMA e connetterli trasmette il vswitch VMSTEST.

   ```PowerShell    
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
   ```
   
3. Visualizzare le proprietà di interfaccia di rete di gestione.

   ```PowerShell    
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**Risultati:**_ 

   |Nome |IsManagementOs |VMName |SwitchName |MacAddress |Stato |IPAddresses|
   |----|--------------|------|----------|----------|------|-----------|
   |CORP-External-opzione |True |CORP-External-opzione |001B785768AA|{Ok} |&nbsp;| 
   |Mgt |True |VMSTEST |E41D2D074071 |{Ok} |&nbsp;|
   |SMB1 |True |VMSTEST |00155D30AA00 |{Ok} |&nbsp;|
   |SMB2 |True |VMSTEST |00155D30AA01 |{Ok} |&nbsp;|
   ---
   
## <a name="step-9-assign-an-ip-address-to-the-smb-host-vnics-vethernet-smb1-and-vethernet-smb2"></a>Passaggio 9: Assegnare un indirizzo IP per il vEthernet Vnic Host SMB \(SMB1\) vEthernet e \(SMB2\)

Il TEST-40G-1 e 40G-TEST-2 schede fisiche abbiano un accesso VLAN di 101 e 102 configurato. Per questo motivo, le schede di codificare il traffico - e ping ha esito positivo. In precedenza, si configurati entrambi gli ID VLAN pNIC su zero, quindi impostare il vSwitch VMSTEST su VLAN 101. Successivamente, erano ancora in grado di effettuare il ping scheda VLAN 101 remota usando la scheda di rete virtuale di gestione, ma non sono attualmente presenti membri 102 VLAN.



1. Rimuovere l'impostazione di VLAN accesso dalla scheda NIC fisica per impedire che entrambi assegnazione automatica di tag il traffico in uscita con l'ID VLAN non corretto e per impedire che filtra il traffico in ingresso corrisponde l'ID accesso VLAN.

   ```PowerShell    
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
   ```

   _**Risultati:**_  

   ```   
   IPAddress : 192.168.2.111
   InterfaceIndex: 40
   InterfaceAlias: vEthernet (SMB1)
   AddressFamily : IPv4
   Type  : Unicast
   PrefixLength  : 24
   PrefixOrigin  : Manual
   SuffixOrigin  : Manual
   AddressState  : Invalid
   ValidLifetime : Infinite ([TimeSpan]::MaxValue)
   PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
   SkipAsSource  : False
   PolicyStore   : PersistentStore
   ```

2. Testare l'adattatore VLAN 102 remoto.
    
   ```PowerShell
   Test-NetConnection 192.168.2.5 
   ```
   
   _**Risultati:**_  
   
   ```
   ComputerName   : 192.168.2.5
   RemoteAddress  : 192.168.2.5
   InterfaceAlias : vEthernet (SMB1)
   SourceAddress  : 192.168.2.111
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```
    
3. Aggiungere un nuovo indirizzo IP per interfaccia vEthernet \(SMB2\).

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
   ```
   
   _**Risultati:**_ 
   
   ```
   IPAddress : 192.168.2.222
   InterfaceIndex: 44
   InterfaceAlias: vEthernet (SMB2)
   AddressFamily : IPv4
   Type  : Unicast
   PrefixLength  : 24
   PrefixOrigin  : Manual
   SuffixOrigin  : Manual
   AddressState  : Invalid
   ValidLifetime : Infinite ([TimeSpan]::MaxValue)
   PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
   SkipAsSource  : False
   PolicyStore   : PersistentStore
   ```
   
4. Verificare nuovamente la connessione.    


5. Posizionare le schede RDMA Host su 102 la VLAN preesistente.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS
    
   Get-VMNetworkAdapterVlan -ManagementOS
   ```

   _**Risultati:**_ 

   ```   
   VMName VMNetworkAdapterName Mode VlanList
   ------ -------------------- ---- --------
      SMB1 Access   102 
      Mgt  Access   101 
      SMB2 Access   102 
      CORP-External-Switch Untagged
   ```
   
6. Esaminare il mapping di SMB1 e SMB2 alle schede di rete fisiche sottostante nel commutatore virtuale impostato Team.<p>L'associazione di Host vNIC a schede NIC fisiche è casuale e soggetta a ribilanciamento durante la creazione e distruzione. In questo caso, è possibile utilizzare un meccanismo indiretto per controllare l'associazione corrente. Gli indirizzi MAC dei SMB1 e SMB2 sono associati al membro di interfaccia di rete Team TEST-40G-2. Ciò non è ideale perché non ha una scheda di rete virtuale Host SMB associato, Test-40G-1 e non consentirà un utilizzo del traffico RDMA tramite il collegamento fino a quando non viene eseguito il mapping di una scheda di rete virtuale Host SMB a esso.

   ```PowerShell    
   Get-NetAdapterVPort (Preferred)
    
   Get-NetAdapterVmqQueue
   ```
   
   _**Risultati:**_ 
   
   ```
   Name   QueueID MacAddressVlanID Processor VmFriendlyName
   ----   ------- ---------------- --------- --------------
   TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
   TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
   TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
   ```

7. Visualizzare le proprietà scheda di rete della macchina virtuale.

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**Risultati:**_ 
   
   ```
   Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
   ---- -------------- ------ ----------   ----------   ------ -----------
   CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
   Mgt  True  VMSTEST  E41D2D074071 {Ok}  
   SMB1 True  VMSTEST  00155D30AA00 {Ok}  
   SMB2 True  VMSTEST  00155D30AA01 {Ok}  
   ```

8. Consente di visualizzare il mapping di rete della scheda team.<p>I risultati non devono restituire informazioni perché non ancora eseguito il mapping.
    
   ```PowerShell
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
   ```
   
   
9. Eseguire il mapping SMB1 e SMB2 per separare i membri del team NIC fisici e per visualizzare i risultati delle proprie azioni.

   >[!IMPORTANT]
   >Assicurarsi di aver completato questo passaggio prima di procedere, o l'implementazione avrà esito negativo.
    
   ```PowerShell
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"
    
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
   ```

   _**Risultati:**_ 
   
   ```   
   NetAdapterName : Test-40G-1
   NetAdapterDeviceId : {BAA9A00F-A844-4740-AA93-6BD838F8CFBA}
   ParentAdapter  : VMInternalNetworkAdapter, Name = 'SMB1'
   IsTemplate : False
   CimSession : CimSession: .
   ComputerName   : 27-3145G0803
   IsDeleted  : False
    
   NetAdapterName : Test-40G-2
   NetAdapterDeviceId : {B7AB5BB3-8ACB-444B-8B7E-BC882935EBC8}
   ParentAdapter  : VMInternalNetworkAdapter, Name = 'SMB2'
   IsTemplate : False
   CimSession : CimSession: .
   ComputerName   : 27-3145G0803
   IsDeleted  : False
   ```
   
10. Verificare che le associazioni di MAC create in precedenza.

    ```PowerShell    
    Get-NetAdapterVmqQueue
    ```

    _**Risultati:**_ 
   
    ```   
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    ```


11. Testare la connessione dal sistema remoto, perché entrambi Vnic dell'Host si trovano nella stessa subnet e hanno lo stesso ID VLAN \(102\).

    ```PowerShell 
    Test-NetConnection 192.168.2.111
    ```

    _**Risultati:**_   

    ```
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    ```
    
    ```PowerShell   
    Test-NetConnection 192.168.2.222
    ```

    _**Risultati:**_   

    ```
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    ```
12. Impostare il nome, nome del commutatore e i tag di priorità.   

    ```PowerShell
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    ```

    _**Risultati:**_   
    
    ```
    Name: SMB1
    SwitchName  : VMSTEST
    IeeePriorityTag : On 
    Status  : {Ok}
   
    Name: SMB2
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    ```

13. Visualizzare le proprietà dell'adapter vEthernet rete.
    
    ```PowerShell
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    ```

    _**Risultati:**_   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    ```

14. Abilita le schede di rete vEthernet.  
    
    ```PowerShell
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    ```

    _**Risultati:**_   
    
    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

## <a name="step-10-validate-the-rdma-functionality"></a>Passaggio 10. Convalidare la funzionalità RDMA.

Si desidera convalidare la funzionalità RDMA dal sistema remoto al sistema locale, che dispone di un commutatore virtuale, a entrambi i membri del team SET di commutatore virtuale.<p>Poiché entrambi Vnic dell'Host \(SMB1 e SMB2\) vengono assegnati a 102 VLAN, è possibile selezionare la scheda di rete VLAN 102 nel sistema remoto. <p>In questo esempio, l'interfaccia di rete Test-40G-2 esegue RDMA per SMB1 (192.168.2.111) e SMB2 (192.168.2.222).

>[!TIP]
>Potrebbe essere necessario disabilitare il Firewall su questo sistema.  Per dettagli, consultare i criteri dell'infrastruttura.
>
>```PowerShell
>Set-NetFirewallProfile -All -Enabled False
>    
>Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
>```
>
>_**Risultati:**_ 
>   
>```
>Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
>----  -----------------------   --------------- -------------  
> .
> .
>Test-40G-2VLAN ID102VlanID  {102} 
>```
    
1. Visualizzare le proprietà scheda di rete.

   ```PowerShell
   Get-NetAdapter
   ```
    
   _**Risultati:**_ 
    
   ```
   Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
   ----  --------------------------- ------   ---------- ---------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
   ```
   
2. Consente di visualizzare la scheda di rete informazioni RDMA.

   ```PowerShell
   Get-NetAdapterRdma
   ```
    
   _**Risultati:**_  
    
   ```
   Name  InterfaceDescription Enabled
   ----  -------------------- -------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
   ```

3. Eseguire il test di traffico RDMA nella prima scheda fisica.

   ```PowerShell 
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**Risultati:**_ 
    
   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter Test-40G-2 is a physical adapter
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.111, is reachable.
   VERBOSE: Remote IP 192.168.2.111 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 34251744 RDMA bytes sent per second
   VERBOSE: 967346308 RDMA bytes written per second
   VERBOSE: 35698177 RDMA bytes sent per second
   VERBOSE: 976601842 RDMA bytes written per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.111
   ```

4. Eseguire il test di traffico RDMA sulla seconda scheda fisica.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**Risultati:**_ 
    
   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter Test-40G-2 is a physical adapter
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.222, is reachable.
   VERBOSE: Remote IP 192.168.2.222 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 485137693 RDMA bytes written per second
   VERBOSE: 35200268 RDMA bytes sent per second
   VERBOSE: 939044611 RDMA bytes written per second
   VERBOSE: 34880901 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.222
   ```
    
5. Test per il traffico RDMA da locale al computer remoto.

    ```PowerShell
    Get-NetAdapter | ft –AutoSize
    ```
    
    _**Risultati:**_ 
    
    ```
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    ```

6. Eseguire il test di traffico RDMA nella prima scheda virtuale.    
    
   ```
   C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**Risultati:**_ 
    
   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter vEthernet (SMB1) is a virtual adapter
   VERBOSE: Retrieving vSwitch bound to the virtual adapter
   VERBOSE: Found vSwitch: VMSTEST
   VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1, TEST-40G-2
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
   VERBOSE: Remote IP 192.168.2.5 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 15250197 RDMA bytes sent per second
   VERBOSE: 896320913 RDMA bytes written per second
   VERBOSE: 33947559 RDMA bytes sent per second
   VERBOSE: 912160540 RDMA bytes written per second
   VERBOSE: 34091930 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
   ```

7. Eseguire il test di traffico RDMA sulla seconda scheda virtuale.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**Risultati:**_ 
    
   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter vEthernet (SMB2) is a virtual adapter
   VERBOSE: Retrieving vSwitch bound to the virtual adapter
   VERBOSE: Found vSwitch: VMSTEST
   VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1, TEST-40G-2
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
   VERBOSE: Remote IP 192.168.2.5 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 385169487 RDMA bytes written per second
   VERBOSE: 33902277 RDMA bytes sent per second
   VERBOSE: 907354685 RDMA bytes written per second
   VERBOSE: 33923662 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
   ```
    
L'ultima riga nell'output di questo "test di traffico RDMA riuscito: Il traffico RDMA è stato inviato a 192.168.2.5,"Mostra che è stato configurato correttamente interfaccia di rete convergente sulla propria scheda.

## <a name="related-topics"></a>Argomenti correlati 

- [Configurazione scheda di rete convergente con una sola scheda di rete](cnic-single.md)
- [Configurazione di commutatore fisico per la scheda di rete convergente](cnic-app-switch-config.md)
- [Risoluzione dei problemi di configurazioni di interfaccia di rete convergente](cnic-app-troubleshoot.md)
