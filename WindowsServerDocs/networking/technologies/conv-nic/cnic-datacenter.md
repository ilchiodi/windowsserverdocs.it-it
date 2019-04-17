---
title: Configurazione di rete convergente le NIC combinate
description: In questo argomento vengono fornite istruzioni su come configurare una scheda di rete convergente in una configurazione di Data Center in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ac6c2915301b1cf64486f24c197ebbf22bb5e2e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="converged-nic-teamed-nic-configuration"></a>Configurazione di rete convergente le NIC combinate

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Le sezioni seguenti forniscono istruzioni per la distribuzione convergente NIC in una configurazione NIC raggruppata con Switch Embedded Teaming \(SET\). La configurazione di esempio in questa guida illustra due host Hyper-V, Hyper-V Host 1 e 2 Host Hyper-V.

## <a name="test-connectivity-between-source-and-destination"></a>Verificare la connettività tra origine e di destinazione

In questa sezione include i passaggi necessari per testare la connettività tra gli host Hyper-V di origine e di destinazione.

L'illustrazione seguente mostra due host Hyper-V, **Hyper-V Host 1** e **Hyper-V Host 2**. 

Entrambi gli host Hyper-V sono due schede di rete. In ogni host, una sola scheda connessa alla subnet 192.168.1.x/24 e una sola scheda connessa alla subnet 192.168.2.x/24.

![Host Hyper-V](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

### <a name="test-nic-connectivity-to-the-hyper-v-virtual-switch"></a>Testare la connettività di rete al commutatore virtuale Hyper-V

Con questo passaggio, è possibile verificare che la scheda NIC fisica, per cui è in un secondo momento verrà creato un commutatore virtuale Hyper-V, è possibile connettersi all'host di destinazione. 

Questo test dimostra la connettività tramite \(L3\) livello 3 - o - livello IP nonché \(L2\) livello 2 virtual local area network \(VLAN\).

È possibile utilizzare il comando di Windows PowerShell seguente per ottenere le proprietà della scheda di rete.

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
     
Di seguito sono risultati di esempio di questo comando.

|Nome|InterfaceDescription|ifIndex|Stato|Indirizzo MAC|LinkSpeed|
|-----|--------------------|-------|-----|----------|---------|
|
|Test-40G-1|Scheda Ethernet Pro Mellanox ConnectX-3|11|Backup|E4-1D-2D-07-43-D0|40 GB/s|

È possibile utilizzare uno dei comandi seguenti per ottenere le proprietà delle schede aggiuntive, inclusi l'indirizzo IP.

    Get-NetIPAddress -InterfaceAlias "Test-40G-1"

    Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
    
Di seguito sono risultati di esempio di questi comandi.

|Parametro|Valore|
|---------|-----|
|
|IPAddress| 192.168.1.3|
|InterfaceIndex|11|
|InterfaceAlias|Test-40G-1|
|AddressFamily|IPv4|
|Tipo| Unicast|
|LunghezzaPrefisso dove|24|

### <a name="verify-that-nic-team-member-has-a-valid-ip-address"></a>Verificare che disponga di un indirizzo IP valido membro del Team NIC

È possibile utilizzare questo passaggio per verificare che gli altri membri del Team NIC o Switch Embedded Team \(SET\) membro fisico NIC \(pNICs\) anche avere un indirizzo IP valido.

Per questo passaggio, è possibile utilizzare una subnet distinta, \ (xxx.xxx.** 2**xxx vs xxx.xxx. **1**. xxx\), per agevolare l'invio da questa scheda nella destinazione. 

In caso contrario, se è disponibile nella stessa subnet, entrambi pNICs bilancia il carico dello stack TCP/IP di Windows tra le interfacce e convalida semplice diventa più complessa.

È possibile utilizzare il comando di Windows PowerShell seguente per ottenere le proprietà della scheda di rete seconda.

    Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize

Di seguito sono risultati di esempio di questo comando.

|Nome |InterfaceDescription |ifIndex |Stato |Indirizzo MAC |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|TEST-40G-2 |Ethernet Pro Mellanox ConnectX-3 r.... \ n. 2 |13 |Backup |E4-1D-2D-07-40-70 |40 GB/s|

È possibile utilizzare uno dei comandi seguenti per ottenere le proprietà delle schede aggiuntive, inclusi l'indirizzo IP.

    Get-NetIPAddress -InterfaceAlias "Test-40G-2"

    Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$\_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress

Di seguito sono risultati di esempio di questi comandi.

|Parametro|Valore|
|---------|-----|
|
|IPAddress|192.168.2.3|
|InterfaceIndex|13|
|InterfaceAlias|TEST-40G-2|
|AddressFamily|IPv4|
|Tipo|Unicast|
|LunghezzaPrefisso dove|24|

### <a name="verify-that-additional-nics-have-valid-ip-addresses"></a>Verificare che altre NIC dispongano di indirizzi IP validi

È possibile utilizzare questo passaggio per assicurarsi che la scheda NIC di altri disponga di un indirizzo IP valido.

Per questo passaggio, è possibile utilizzare una subnet distinta, \ (xxx.xxx.** 2**xxx vs xxx.xxx. **1**. xxx\), per agevolare l'invio da questa scheda nella destinazione. 

In caso contrario, se è disponibile nella stessa subnet, entrambi pNICs bilancia il carico dello stack TCP/IP di Windows tra le interfacce e convalida semplice diventa più complessa.

È possibile utilizzare il comando di Windows PowerShell seguente per ottenere le proprietà della scheda di rete seconda.

    Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize

Di seguito sono risultati di esempio di questo comando.

|Nome |InterfaceDescription |ifIndex |Stato |Indirizzo MAC |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|TEST-40G-2 |Ethernet Pro Mellanox ConnectX-3 r.... \ n. 2 |13 |Backup |E4-1D-2D-07-40-70 |40 GB/s|

È possibile utilizzare uno dei comandi seguenti per ottenere le proprietà delle schede aggiuntive, inclusi l'indirizzo IP.
    
    Get-NetIPAddress -InterfaceAlias "Test-40G-2"

    Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress

Di seguito sono risultati di esempio di questi comandi.

|Parametro|Valore|
|---------|-----|
|
|IPAddress|192.168.2.3|
|InterfaceIndex|13|
|InterfaceAlias|TEST-40G-2|
|AddressFamily|IPv4|
|Tipo|Unicast|
|LunghezzaPrefisso dove|24|

### <a name="ensure-that-source-and-destination-can-communicate"></a>Assicurarsi che possa comunicare l'origine e destinazione

È possibile utilizzare questo passaggio per verificare la comunicazione bidirezionale \ (ping dall'origine alla destinazione e viceversa in entrambi systems\).  Nell'esempio seguente, il **Test NetConnection** viene utilizzato il comando di Windows PowerShell, ma se preferisci che è possibile utilizzare il **ping** comando. 

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

### <a name="verify-connectivity-for-additional-nics"></a>Verificare la connettività per schede di rete aggiuntivi

Con questo passaggio, è possibile ripetere i passaggi precedenti per tutti i successivi pNICs inclusi nel gruppo NIC o un SET.

È possibile utilizzare il comando seguente per verificare la connessione.
    
    Test-NetConnection 192.168.2.5

Di seguito sono risultati di esempio di questo comando.

|Parametro|Valore|
|---------|-----|
|
|ComputerName|192.168.2.5|
|RemoteAddress|192.168.2.5|
|InterfaceAlias|Test-40G-2|
|IndirizzoOrigine|192.168.2.3|
|PingSucceeded|False|
|PingReplyDetails \(RTT\)|0 ms|


## <a name="configure-vlans"></a>Configurare le VLAN

Per questo passaggio, le schede NIC sono **accesso** modalità. Tuttavia, quando si crea un commutatore virtuale Hyper-V di \(vSwitch\) più avanti in questa Guida, le proprietà VLAN vengono applicate a livello di porta vSwitch. 

Poiché un commutatore può ospitare più VLAN, è necessario per il commutatore fisico \(ToR\) Top of Rack affinché la porta configurata in modalità Trunk.

Per istruzioni su come configurare la modalità Trunk nel commutatore, consultare la documentazione del commutatore ToR.

L'illustrazione seguente mostra due host Hyper-V con due schede di rete ogni che hanno VLAN 101 e 102 VLAN configurato nelle proprietà della scheda di rete.

![Configurare reti locali virtuali](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)

### <a name="configure-the-vlan-ids-for-both-nics"></a>Configurare gli ID VLAN per entrambe le schede NIC

Per configurare gli ID VLAN per le schede NIC installata nell'host Hyper-V, è possibile utilizzare questo passaggio.

In base di Institute of Electrical and Electronics Engineers \(IEEE\) degli standard di rete, le proprietà di \(QoS\) Quality of Service nella scheda NIC agire testata 802.1 p è incorporato all'interno di 802.1 q \(VLAN\) intestazione quando si configura il ID. VLAN

#### <a name="configure-nic-test-40g-1"></a>Configurare NIC Test-40G-1

Con i comandi seguenti, configurare l'ID VLAN per la prima scheda di rete, Test-40G-1, quindi visualizzare la configurazione risultante.

    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
    Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
    
Di seguito sono risultati di esempio di questo comando.

|Nome |DisplayName| Disponibili DisplayValue| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|TEST-40G-1|ID VLAN|101|VlanID|{101}|


Assicurarsi che l'ID VLAN ha effetto indipendente dell'implementazione di scheda di rete utilizzando il comando seguente per riavviare la scheda di rete.

    Restart-NetAdapter -Name "Test-40G-1"

È possibile utilizzare il comando seguente per verificare che lo stato di scheda di rete sia **backup** prima di procedere.

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize

Di seguito sono risultati di esempio di questo comando.

|Nome|InterfaceDescription|ifIndex| Stato|Indirizzo MAC|LinkSpeed|
|----|--------------------|-------|------|----------| ---------|
|
|Test-40G-1|Ada Ethernet Pro Mellanox ConnectX-3...|11|Backup|E4-1D-2D-07-43-D0|40 GB/s|

#### <a name="configure-nic-test-40g-2"></a>Configurare Test-40G-2 NIC

Con i comandi seguenti, configurare l'ID VLAN per la seconda scheda di rete, Test-40G-2, quindi visualizzare la configurazione risultante.

    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
    Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
    

Di seguito sono risultati di esempio di questo comando.

|Nome |DisplayName| Disponibili DisplayValue| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|TEST-40G-2|ID VLAN|102|VlanID|{102}|

Assicurarsi che l'ID VLAN ha effetto indipendente dell'implementazione di scheda di rete utilizzando il comando seguente per riavviare la scheda di rete.

`Restart-NetAdapter -Name "Test-40G-2"  `              

È possibile utilizzare il comando seguente per verificare che lo stato di scheda di rete sia **backup** prima di procedere.

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize

Di seguito sono risultati di esempio di questo comando.

|Nome|InterfaceDescription|ifIndex| Stato|Indirizzo MAC|LinkSpeed|
|----|--------------------|-------|------|----------| ---------|
|
|Test-40G-2 |Ada Ethernet Pro Mellanox ConnectX-3... |11 |Backup |E4-1D-2D-07-43-D1 |40 GB/s|


### <a name="verify-connectivity"></a>Verificare la connettività

È possibile utilizzare questa sezione per verificare la connettività dopo le schede di rete vengono riavviate. È possibile verificare la connettività dopo l'applicazione di tag VLAN a entrambe le schede. Se si verifica un errore di connettività, è possibile esaminare il commutatore partecipazione VLAN di configurazione o di destinazione nella stessa VLAN. 

>[!IMPORTANT]
>Dopo aver eseguito i passaggi nella sezione precedente, potrebbe richiedere alcuni secondi per il dispositivo per riavviare e diventano disponibili nella rete.

#### <a name="verify-connectivity-for-nic-test-40g-1"></a>Verificare la connettività per NIC Test-40G-1

Per verificare la connettività per la prima scheda di rete, è possibile eseguire il comando seguente.

    Test-NetConnection 192.168.1.5
    
Di seguito sono risultati di esempio di questo comando.

|Parametro|Valore|
|---------|-----|
|
|ComputerName|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|Test-40G-1|
|IndirizzoOrigine|192.168.1.5|
|PingSucceeded|True|
|PingReplyDetails \(RTT\)|0 ms|

#### <a name="verify-connectivity-for-nic-test-40g-2"></a>Verificare la connettività per Test-40G-2 NIC

È possibile utilizzare questa sezione per testare la connettività per Test-40G-2 NIC.

È possibile utilizzare il comando seguente per testare la connettività per la seconda scheda di rete.
    
    Test-NetConnection 192.168.2.5
    
Di seguito sono risultati di esempio di questo comando.

|Parametro|Valore|
|---------|-----|
|
|ComputerName|192.168.2.5|
|RemoteAddress|192.168.2.5|
|InterfaceAlias|Test-40G-2|
|IndirizzoOrigine|192.168.2.3|
|PingSucceeded|True|
|PingReplyDetails \(RTT\)|0 ms|

Un **Test NetConnection** errore o errore del ping che si verifica immediatamente dopo l'esecuzione **riavvio-NetAdapter** non è insolito, quindi attendere che la scheda di rete inizializzare completamente e quindi riprovare.

Se le connessioni di rete VLAN 101 funzionare, ma le connessioni 102 VLAN non funzionano, il problema potrebbe essere che il commutatore deve essere configurato per consentire il traffico della porta sulla VLAN desiderato. È possibile verificarlo temporaneamente impostando le schede di errori su VLAN 101 e ripetere il test della connettività.

## <a name="configure-quality-of-service-qos"></a>Configurare qualità del servizio \(QoS\)

Il passaggio successivo consiste nel configurare \(QoS\) Quality of Service, che è necessario innanzitutto installare la funzionalità di Windows Server 2016 \(DCB\) Data Center Bridging.

La figura seguente illustra gli host Hyper-V dopo la corretta configurazione VLAN nel passaggio precedente.

![Configurare qualità del servizio](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)

### <a name="install-data-center-bridging-dcb"></a>Installazione di Data Center Bridging \(DCB\)

Per installare e abilitare DCB, è possibile utilizzare questo passaggio. Nella maggior parte dei casi, questo passaggio è facoltativo per iWarp implementazioni, ma è necessario scala dell'infrastruttura, ad esempio per gli scenari di cross-rack.

È possibile utilizzare il comando seguente per installare DCB in tutti gli host Hyper-V. 

    Install-WindowsFeature Data-Center-Bridging

Di seguito sono risultati di esempio di questo comando.

|Operazione riuscita |Riavvio necessario |Codice di uscita|Risultato di funzionalità|
|------- |-------------- |--------- |-------------- |
|
|True |No |Operazione riuscita| {Data Center Bridging}|

### <a name="set-the-qos-policies-for-smb-direct"></a>Impostare i criteri QoS per SMB diretto 

È possibile utilizzare il comando seguente per configurare i criteri QoS per SMB diretto.

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

### <a name="set-policies-for-other-traffic-on-the-interface"></a>Impostare i criteri per altro traffico sull'interfaccia 

È possibile utilizzare il comando seguente per impostare i criteri QoS aggiuntivi.

    New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
 
Di seguito sono risultati di esempio di questo comando.

|Parametro|Valore|
|---------|-----|
|
|Nome | IMPOSTAZIONE PREDEFINITA|
|Proprietario|\(Machine\) criteri di gruppo|
|NetworkProfile|Tutti|
|Precedenza|127|
|Modello| Impostazione predefinita|
|JobObject| &nbsp;|
|PriorityValue|0|

### <a name="turn-on-flow-control-for-smb"></a>Attivare il controllo di flusso per SMB 

È possibile utilizzare i comandi seguenti per abilitare il controllo di flusso SMB e visualizzare i risultati.

    Enable-NetQosFlowControl -priority 3
    Get-NetQosFlowControl

Di seguito sono risultati di esempio di questo comando.

|Priorità|Abilitato|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |False |Globale|&nbsp;|&nbsp;|
|1 |False |Globale|&nbsp;|&nbsp;|
|2 |False |Globale|&nbsp;|&nbsp;|
|3 |True |Globale|&nbsp;|&nbsp;|
|4 |False |Globale|&nbsp;|&nbsp;|
|5 |False |Globale|&nbsp;|&nbsp;|
|6 |False |Globale|&nbsp;|&nbsp;|
|7 |False |Globale|&nbsp;|&nbsp;|

Se i risultati non corrispondono a questi risultati perché gli elementi diversi da 3 dispongono di un valore Enabled su True, è necessario eseguire il passaggio successivo. Se i risultati corrispondono a questi risultati di esempio e unico elemento 3 ha un valore True abilitato, non è necessaria eseguire il successivo passaggio e ignorare verso il basso **abilitare QoS per le schede di rete di destinazione e di destinazione**.

### <a name="ensure-flow-control-is-disabled-for-other-traffic-classes-optional"></a>Assicurarsi di che controllo di flusso è disabilitato per \(Optional\) altre classi di traffico

Non è necessario eseguire questo passaggio se **controllo di flusso** non è abilitato per le classi di traffico 0,1,2,4,5,6 e 7.

Se **controllo di flusso** è già abilitato per le classi di qualsiasi tipo di traffico diversi da 3 \ (0,1,2,4,5,6 e 7\), è necessario disabilitare **controllo di flusso** per queste classi. 

>[!NOTE]
>In configurazioni più complesse, le classi di traffico potrebbero richiedere il controllo di flusso, tuttavia, questi scenari non rientrano nell'ambito di questa Guida.

È possibile utilizzare i comandi seguenti per disabilitare il controllo di flusso SMB per le classi di traffico 0,1,2,4,5,6 e 7 e per visualizzare i risultati.

    Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
    Get-NetQosFlowControl

Di seguito sono risultati di esempio di questo comando.

|Priorità|Abilitato|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |False |Globale|&nbsp;|&nbsp;|
|1 |False |Globale|&nbsp;|&nbsp;|
|2 |False |Globale|&nbsp;|&nbsp;|
|3 |True |Globale|&nbsp;|&nbsp;|
|4 |False |Globale|&nbsp;|&nbsp;|
|5 |False |Globale|&nbsp;|&nbsp;|
|6 |False |Globale|&nbsp;|&nbsp;|
|7 |False |Globale|&nbsp;|&nbsp;|

### <a name="enable-qos-for-the-local-and-destination-network-adapters"></a>Abilitare QoS per le schede di rete locali e di destinazione 

Con questo passaggio è possibile abilitare QoS per schede di rete locali e di destinazione. Assicurarsi di eseguire questi comandi in entrambi i host Hyper-V.

#### <a name="enable-qos-for-nic-test-40g-1"></a>Abilitare QoS per NIC Test-40G-1

È possibile utilizzare i comandi seguenti per abilitare QoS e visualizzare i risultati della configurazione.

    Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
    Get-NetAdapterQos -Name "Test-40G-1"

Di seguito sono risultati di esempio di questo comando.

**Nome**: TEST-40G-1 **abilitato**: True **funzionalità**:   

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
|0| Strict|&nbsp;|0-7|

**OperationalFlowControl**: priorità 3 Enabled **OperationalClassifications**:

|Protocollo|Tipo di porta /|Priorità|
|--------|---------|--------|
|
|Impostazione predefinita|&nbsp;|0|
|NetDirect| 445|3|

#### <a name="enable-qos-for-nic-test-40g-2"></a>Abilitare QoS per Test-40G-2 NIC

È possibile utilizzare i comandi seguenti per abilitare QoS e visualizzare i risultati della configurazione.

    Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
    Get-NetAdapterQos -Name "Test-40G-2"

 
Di seguito sono risultati di esempio di questo comando.

**Nome**: TEST-40G-2 **abilitato**: True **funzionalità**:   

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
|0| Strict|&nbsp;|0-7|

**OperationalFlowControl**: priorità 3 Enabled **OperationalClassifications**:

|Protocollo|Tipo di porta /|Priorità|
|--------|---------|--------|
|
|Impostazione predefinita|&nbsp;|0|
|NetDirect| 445|3|


### <a name="allocate-50-of-the-bandwidth-reservation-to-smb-direct-rdma"></a>Allocare 50% della prenotazione della larghezza di banda a SMB diretto \(RDMA\)

È possibile utilizzare i comandi seguenti per assegnare la metà della prenotazione della larghezza di banda a SMB diretto.

    New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS

Di seguito sono risultati di esempio di questo comando.

|Nome|Algoritmo |Bandwidth(%)| Priorità |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|SMB | ETS     | 50 |3 |Globale |&nbsp;|&nbsp;|                                      
 
È possibile utilizzare il comando seguente per visualizzare le informazioni di prenotazione della larghezza di banda.

    Get-NetQosTrafficClass | ft -AutoSize

Di seguito sono risultati di esempio di questo comando.
 
|Nome|Algoritmo |Bandwidth(%)| Priorità |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[Predefinito]| ETS|50 |0-2,4-7|  Globale|&nbsp;|&nbsp;| 
SMB |ETS|50 |3 |Globale|&nbsp;|&nbsp;| 

### <a name="create-traffic-classes-for-tenant-ip-traffic-optional"></a>Creare classi di traffico per il traffico Tenant IP \(optional\)

È possibile utilizzare questo passaggio per creare due classi di traffico aggiuntivo per il traffico IP tenant. 

Quando si eseguono i comandi di esempio seguente, è possibile omettere i valori "IP1" e "IP2" Se si preferisce.

    New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS

Di seguito sono risultati di esempio di questo comando.

|Nome|Algoritmo |Bandwidth(%)| Priorità |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|IP1 |ETS |10 |1 |Globale|&nbsp;|&nbsp;|


    New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS

Di seguito sono risultati di esempio di questo comando.

|Nome|Algoritmo |Bandwidth(%)| Priorità |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|IP2 |ETS |10 |2 |Globale|&nbsp;|&nbsp;|

È possibile utilizzare il comando seguente per visualizzare le classi di traffico QoS.

    Get-NetQosTrafficClass | ft -AutoSize

Di seguito sono risultati di esempio di questo comando.

|Nome|Algoritmo |Bandwidth(%)| Priorità |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[Predefinito] |ETS |30 |0,4-7 |Globale|&nbsp;|&nbsp;|
|SMB |ETS |50 |3 |Globale|&nbsp;|&nbsp;|
|IP1 |ETS |10 |1 |Globale|&nbsp;|&nbsp;|
|IP2 |ETS |10 |2 |Globale|&nbsp;|&nbsp;|

## <a name="configure-debugger-optional"></a>Configurare \(Optional\) Debugger

Questo passaggio è possibile utilizzare per configurare il debugger.

Per impostazione predefinita, il debugger collegato Blocca NetQos. È possibile utilizzare i comandi seguenti per sostituire il debugger e visualizzare i risultati.

    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger

Di seguito sono risultati di esempio di questo comando.

    AllowFlowControlUnderDebugger
    -----------------------------
    1

## <a name="test-rdma-mode-1"></a>Testare \(Mode 1\) RDMA

È possibile utilizzare questo passaggio per assicurarsi che l'infrastruttura sia configurato correttamente prima della creazione di un vSwitch e passando a \(Mode 2\) RDMA.

L'illustrazione seguente mostra lo stato corrente degli host Hyper-V.

![Test RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)

Per verificare la configurazione di RDMA, è possibile eseguire il comando seguente.

    Get-NetAdapterRdma | ft -AutoSize

Di seguito sono risultati di esempio di questo comando.

|Nome |InterfaceDescription |Abilitato|
|----|--------------------|-------|
|
|TEST-40G-1| Scheda Mellanox ConnectX-4 VPI #2 |True|
|TEST-40G-2| Scheda Mellanox ConnectX-4 VPI |True|


### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>Determinare il valore ifIndex destinazione della scheda di rete

È possibile utilizzare il comando seguente per individuare il valore ifIndex della scheda di destinazione.

    Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

Di seguito sono risultati di esempio di questo comando.

|InterfaceAlias |InterfaceIndex |IPv4Address|
|-------------- |-------------- |-----------|
|TEST-40G-1 |14 |{192.168.1.3}|
|TEST-40G-2 | 13 |{192.168.2.3}|

### <a name="download-diskspdexe-and-a-powershell-script"></a>Download di DiskSpd.exe e uno Script di PowerShell

Per continuare, è necessario prima scaricare gli elementi seguenti.

- Scaricare l'utilità DiskSpd.exe ed estrarre l'utilità in C:\TEST\[http://tinyurl.com/z68h3rc](http://tinyurl.com/z68h3rc)

- Scaricare lo script di powershell Test-RDMA a C:\TEST\[https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

### <a name="run-the-powershell-script"></a>Eseguire lo script PowerShell

Quando si esegue il ps1 Test-Rdma script di Windows PowerShell, è possibile passare il valore ifIndex dello script, con l'indirizzo IP della scheda remota nella stessa VLAN.

È possibile utilizzare il comando seguente per eseguire lo script con un ifIndex del 14 nella scheda di rete 192.168.1.5.
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter TEST-40G-1 is a physical adapter
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

### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>Determinare il valore ifIndex destinazione della scheda di rete

È possibile utilizzare il comando seguente per individuare il valore ifIndex della scheda di destinazione.

    Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

Di seguito sono risultati di esempio di questo comando.

|InterfaceAlias |InterfaceIndex |IPv4Address|
|-------------- |-------------- |-----------|
|TEST-40G-1 |14 |{192.168.1.3}|
|TEST-40G-2 | 13 |{192.168.2.3}|

È possibile utilizzare il comando seguente per eseguire lo script con un ifIndex di 13 anni nella scheda di rete 192.168.2.5.

    
    C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    

## <a name="create-a-hyper-v-virtual-switch"></a>Creare un commutatore virtuale Hyper-V

È possibile utilizzare questa sezione per creare un commutatore virtuale Hyper-V di \(vSwitch\) negli host Hyper-V.

L'illustrazione seguente mostra Hyper-V Host 1 con un vSwitch.

![Creare un commutatore virtuale Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

### <a name="create-a-vswitch-in-switch-embedded-teaming-set-mode"></a>Creare un vSwitch in modalità Switch Embedded Teaming \(SET\)

È possibile utilizzare il comando seguente per creare un gruppo di indipendenti commutatore SET denominato **VMSTEST** su Hyper-V Host 1. Entrambe le schede di rete su questo computer, TEST-40G-1 e 2, 40G TEST vengono aggiunti al team SET con questo comando.
    
    New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
    
Di seguito sono risultati di esempio di questo comando.

|Nome |SwitchType |NetAdapterInterfaceDescription|
|---- |---------- |------------------------------|
|VMSTEST |Esterno |Interfaccia raggruppate|

È possibile utilizzare questo comando per visualizzare il gruppo di schede fisiche nel SET.

    Get-VMSwitchTeam -Name "VMSTEST" | fl

Di seguito sono risultati di esempio di questo comando.

**Nome**: VMSTEST **Id**: ad9bb542-dda2-4450-a00e-f96d44bdfbec **NetAdapterInterfaceDescription**: {scheda Ethernet Pro Mellanox ConnectX-3, la scheda Ethernet Pro Mellanox ConnectX-3 #2} **TeamingMode**: SwitchIndependent **LoadBalancingAlgorithm**: dinamico

#### <a name="display-two-views-of-the-host-vnic"></a>Visualizzare due visualizzazioni dei vNIC Host

È possibile utilizzare i due comandi seguenti per due diverse visualizzazioni dello vNIC Host.

È possibile utilizzare questo comando per visualizzare le proprietà di vNIC Host.

    Get-NetAdapter

Di seguito sono risultati di esempio di questo comando.

| Nome | InterfaceDescription & ifIndex | Stato | MacAddress | LinkSpeed & &------|---|---|---|---| & & vEthernet (VMSTEST) & Hyper-V Scheda Ethernet virtuale #2 | 28 & Backup | E4-1D-2D-07-40-71|80 Gbps &

È possibile utilizzare questo comando per visualizzare le proprietà aggiuntive di vNIC Host.

    Get-VMNetworkAdapter -ManagementOS

Di seguito sono risultati di esempio di questo comando.

|Nome |IsManagementOs |VMName |SwitchName |Indirizzo MAC |Stato |IPAddresses|
|----|--------------|------|----------|----------|------|-----------|
|
|VMSTEST|True |VMSTEST |E41D2D074071| {Ok}|&nbsp;|

#### <a name="test-the-network-connection-to-the-remote-vlan-101-adapter"></a>Verificare la connessione di rete alla scheda di rete VLAN 101 remota

È possibile utilizzare questo comando per verificare la connessione alla scheda 101 VLAN remota.

`Test-NetConnection 192.168.1.5` 

Di seguito sono risultati di esempio di questo comando.

    WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable
    
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 10.199.48.170
    PingSucceeded  : False
    PingReplyDetails (RTT) : 0 ms
    
### <a name="reconfigure-vlans"></a>Riconfigurare le VLAN

È possibile utilizzare questo passaggio consente di rimuovere l'impostazione di accesso VLAN dalla scheda NIC fisica e impostare VLANID utilizzando il vSwitch.

È necessario rimuovere l'impostazione di accesso VLAN per evitare entrambi assegnazione automatica di tag il traffico in uscita con l'ID VLAN non corretto e il traffico dal filtro di ingresso che non corrisponde l'ID della VLAN accesso.

È possibile utilizzare i comandi seguenti per rimuovere l'impostazione di accesso VLAN.
    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
    Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
    

È possibile utilizzare i comandi seguenti per impostare VLANID utilizzando i comandi di Windows PowerShell vSwitch specifici e per visualizzare i risultati di questa azione.

    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
    Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
    

Di seguito sono risultati di esempio di questo comando.

VMName VMNetworkAdapterName modalità VlanList
------ -------------------- ----   --------
       VMSTEST              Access 101     

È possibile utilizzare il comando seguente per testare la connessione di rete e visualizzare i risultati.

    Test-NetConnection 192.168.1.5
    
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    

Se i risultati non sono simili ai risultati di esempio e ping ha esito negativo con il messaggio "avviso: il comando Ping per 192.168.1.5 non riuscita - stato: DestinationHostUnreachable," verificare che il "vEthernet (VMSTEST)" è l'indirizzo IP appropriato eseguendo il comando seguente.

    
    Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
    

Se non è impostato l'indirizzo IP, è possibile utilizzare il comando seguente per correggere il problema e visualizzare i risultati delle azioni.

    
    New-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)" -IPAddress 192.168.1.3 -PrefixLength 24
    
    IPAddress : 192.168.1.3
    InterfaceIndex: 37
    InterfaceAlias: vEthernet (VMSTEST)
    AddressFamily : IPv4
    Type  : Unicast
    PrefixLength  : 24
    PrefixOrigin  : Manual
    SuffixOrigin  : Manual
    AddressState  : Tentative
    ValidLifetime : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource  : False
    PolicyStore   : ActiveStore
    
#### <a name="rename-the-management-nic"></a>Rinominare la scheda NIC di gestione

È possibile utilizzare questa sezione per rinominare la scheda NIC di gestione e successivamente utilizzare istanze separate di vNIC Host per RDMA.

È possibile eseguire i comandi seguenti per rinominare la scheda NIC di gestione e visualizzazione di alcune proprietà NIC.

    Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
    Get-VMNetworkAdapter -ManagementOS

Di seguito sono risultati di esempio di questo comando.

|Nome |IsManagementOs |VMName |SwitchName |Indirizzo MAC |Stato |IPAddresses
|----|--------------|------|----------|----------|------|-----------|
|
|CORP-commutatore esterno |True |&nbsp;|CORP-commutatore esterno |001B785768AA |{Ok}|&nbsp;|
|ENERGIA |True |&nbsp;|VMSTEST |E41D2D074071 |{Ok}|&nbsp;|

È possibile eseguire il comando seguente per visualizzare alcune proprietà aggiuntive NIC.

    Get-NetAdapter

Di seguito sono risultati di esempio di questo comando.

|Nome |InterfaceDescription |ifIndex |Stato |Indirizzo MAC |LinkSpeed|
|----|--------------------|------|----------|---------|------|
|
|vEthernet (energia) |Scheda Ethernet virtuale Hyper-V #2 |28 |Backup | E4-1D-2D-07-40-71 |80 GB/s|

## <a name="test-hyper-v-virtual-switch-rdma"></a>Testare il commutatore virtuale Hyper-V RDMA

L'illustrazione seguente mostra lo stato corrente i host Hyper-V, inclusi il vSwitch su Hyper-V Host 1.

![Testare il commutatore virtuale Hyper-V](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

### <a name="set-priority-tagging-on-the-host-vnic-to-complement-the-previous-vlan-settings"></a>Impostare la priorità di assegnazione di tag vNIC Host per integrare le impostazioni VLAN precedenti

È possibile utilizzare i seguenti comandi di esempio per impostare la priorità su vNIC Host la codifica e visualizzare i risultati delle azioni.
    
    Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
    

    Name: MGT
    IeeePriorityTag : On
    
### <a name="create-two-host-vnics-for-rdma"></a>Creare due Host Vnic RDMA

È possibile utilizzare i seguenti comandi di esempio per creare due host Vnic RDMA e collegarli alla VMSTEST vSwitch.
    
    Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
    Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
    
È possibile utilizzare il comando seguente per visualizzare le proprietà di scheda NIC di gestione.
    
    Get-VMNetworkAdapter -ManagementOS
    
Di seguito sono risultati di esempio di questo comando.

|Nome |IsManagementOs |VMName |SwitchName |Indirizzo MAC |Stato |IPAddresses|
|----|--------------|------|----------|----------|------|-----------|
|
|CORP-commutatore esterno |True |CORP-commutatore esterno |001B785768AA|{Ok} |&nbsp;| 
|Energia |True |VMSTEST |E41D2D074071 |{Ok} |&nbsp;|
|SMB1 |True |VMSTEST |00155D30AA00 |{Ok} |&nbsp;|
|SMB2 |True |VMSTEST |00155D30AA01 |{Ok} |&nbsp;|

### <a name="assign-an-ip-address-to-the-smb-host-vnics"></a>Assegnare un indirizzo IP per il Vnic dell'Host di SMB

È possibile utilizzare questa sezione per assegnare il vEthernet vNICs Host SMB addressrd IP \(SMB1\) e vEthernet \(SMB2\).

Il TEST 40G-1 e il TEST-40G-2 schede fisiche ancora VLAN un accesso di 101 e 102 configurato. Per questo motivo, le schede codificare il traffico - e ping ha esito positivo.

In precedenza, è configurato entrambi gli ID VLAN pNIC su zero, quindi impostare il vSwitch VMSTEST 101 VLAN. In seguito, sono stati ancora in grado di eseguire il ping scheda 101 VLAN remota utilizzando la scheda di rete di energia, ma non sono attualmente presenti membri 102 VLAN.

È ora possibile utilizzare il comando seguente per rimuovere l'impostazione di accesso VLAN dalla scheda NIC fisica per impedire che venga entrambi assegnazione automatica di tag il traffico in uscita con l'ID VLAN non corretto e di impedire che filtrano il traffico in ingresso che non corrisponde l'ID della VLAN accesso.

È possibile utilizzare il comando seguente per raggiungere questi obiettivi per l'aggiunta di un nuovo indirizzo IP di interfaccia vEthernet (SMB1) e visualizzare i risultati.

    
    New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
    
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
    

È possibile utilizzare il comando seguente per testare la scheda 102 VLAN remota e visualizzare i risultati.
    
    Test-NetConnection 192.168.2.5 
    
    ComputerName   : 192.168.2.5
    RemoteAddress  : 192.168.2.5
    InterfaceAlias : vEthernet (SMB1)
    SourceAddress  : 192.168.2.111
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
È possibile utilizzare il comando seguente per aggiungere un nuovo indirizzo IP per interfaccia vEthernet \(SMB2\) e visualizzare i risultati.
    
    New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
    
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
    
Non devi verificare nuovamente la connessione in quanto viene stabilita la comunicazione.

### <a name="place-the-rdma-host-vnics-on-vlan-102"></a>Posizionare il Vnic dell'Host RDMA in 102 VLAN

È possibile utilizzare questa sezione per assegnare il Vnic dell'Host RDMA a 102 VLAN.

Poiché la scheda di rete "Energia" Host si trova in 101 VLAN, è possibile utilizzare i seguenti comandi di esempio per inserire la 102 VLAN preesistente scheda Vnic RDMA Host e visualizzare i risultati delle azioni.

    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS
    
    Get-VMNetworkAdapterVlan -ManagementOS
    
    VMName VMNetworkAdapterName Mode VlanList
    ------ -------------------- ---- --------
       SMB1 Access   102 
       Mgt  Access   101 
       SMB2 Access   102 
       CORP-External-Switch Untagged
    
### <a name="inspect-the-mapping-of-smb1-and-smb2-to-the-underlying-physical-nics"></a>Esaminare il Mapping di SMB1 e per le schede NIC fisiche sottostanti SMB2

È possibile utilizzare questa sezione per esaminare il mapping di SMB1 e SMB2 per le schede NIC fisiche sottostanti in vSwitch impostare Team.  L'associazione di Host vNIC a schede NIC fisiche è casuale e soggetti a ribilanciamento durante la creazione e l'eliminazione.

In questo caso è possibile utilizzare un meccanismo indiretto per verificare l'associazione corrente.

Gli indirizzi MAC di SMB1 e SMB2 sono associati al membro del gruppo NIC TEST-40G-2. Questo non è ideale perché non dispone di una scheda di rete SMB Host associato, Test-40G-1 e non potrà per l'utilizzo di traffico RDMA tramite il collegamento fino a quando non viene mappato un vNIC Host SMB.

È possibile utilizzare i seguenti comandi di esempio per visualizzare queste informazioni.

    
    Get-NetAdapterVPort (Preferred)
    
    Get-NetAdapterVmqQueue
    
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
    
    Get-VMNetworkAdapter -ManagementOS
    
    Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
    ---- -------------- ------ ----------   ----------   ------ -----------
    CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
    Mgt  True  VMSTEST  E41D2D074071 {Ok}  
    SMB1 True  VMSTEST  00155D30AA00 {Ok}  
    SMB2 True  VMSTEST  00155D30AA01 {Ok}  
    

Entrambi i comandi riportati di seguito non devono restituire alcuna informazione perché non hanno ancora eseguito il mapping.
    
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
    
È possibile utilizzare i seguenti comandi di esempio per eseguire il mapping SMB1 e SMB2 per separare i membri del gruppo NIC fisici e per visualizzare i risultati delle azioni.

>[!IMPORTANT]
>Assicurarsi di aver completato questo passaggio prima di procedere oppure l'implementazione avrà esito negativo.
    
    Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
    Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"
    
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
    
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
    
### <a name="confirm-the-mac-associations"></a>Verificare le associazioni MAC

È possibile utilizzare i seguenti comandi di esempio per verificare le associazioni MAC creato in precedenza.
    
    Get-NetAdapterVmqQueue
    
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    

Poiché entrambe Vnic dell'Host si trovano nella stessa subnet e avere la stessa \(102\) ID VLAN, è possibile convalidare la comunicazione dal sistema remoto e visualizzare i risultati delle azioni.

>[!IMPORTANT]
>Eseguire i comandi seguenti nel computer remoto.

    
    Test-NetConnection 192.168.2.111
    
    
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
    Test-NetConnection 192.168.2.222
    
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    
        
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    
    Name: SMB1
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    
    Name: SMB2
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    

    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    
    
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
      

### <a name="validate-rdma-functionality-from-the-remote-system"></a>Convalidare le funzionalità RDMA dal sistema remoto.

È possibile utilizzare questa sezione per convalidare la funzionalità RDMA dal sistema remoto al sistema locale, che dispone di un vSwitch, in tal modo test entrambe le schede che sono membri del team SET di vSwitch.

Poiché entrambe Vnic dell'Host \ (SMB1 e SMB2\) vengono assegnati a 102 VLAN, è possibile selezionare la scheda di rete VLAN 102 nel sistema remoto.  

In questo processo, Test-40G-2 NIC non RDMA per SMB1 (192.168.2.111) e SMB2 (192.168.2.222).

Facoltativo: Potrebbe essere necessario disabilitare il Firewall nel sistema.  Per ulteriori informazioni, consultare i criteri dell'infrastruttura.

    
    Set-NetFirewallProfile -All -Enabled False
    
    Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
    
    Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
    ----  -----------------------   --------------- -------------  
    .
    .
    Test-40G-2VLAN ID102VlanID  {102} 
    
    Get-NetAdapter
    
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
    
    
    Get-NetAdapterRdma
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    
### <a name="test-for-rdma-traffic-from-the-local-to-the-remote-computer"></a>Test per il traffico RDMA da locale al computer remoto

In questa sezione è possibile verificare che il traffico RDMA viene inviato dal computer locale al computer remoto.

È possibile utilizzare i seguenti comandi di esempio per testare e visualizzare il flusso di traffico.

    
    Get-NetAdapter | ft –AutoSize
    
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    
La riga finale in questo output, "test traffico RDMA riuscite: traffico RDMA è stato inviato alla 192.168.2.5," Mostra che sia stato configurato correttamente convergente NIC dell'adattatore.

## <a name="all-topics-in-this-guide"></a>Tutti gli argomenti in questa Guida

Questa guida contiene gli argomenti seguenti.

- [Configurazione di rete convergente con una sola scheda di rete](cnic-single.md)
- [Configurazione di rete convergente le NIC combinate](cnic-datacenter.md)
- [Configurazione di commutatore fisico per scheda di rete convergente](cnic-app-switch-config.md)
- [Risoluzione dei problemi convergente configurazioni NIC](cnic-app-troubleshoot.md)
