---
title: NIC convergente in una configurazione NIC in gruppo (Datacenter)
description: In questo argomento vengono fornite le istruzioni per distribuire una scheda di interfaccia di rete convergente in una configurazione NIC in gruppo con switch Embedded Teaming (SET).
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/17/2018
ms.openlocfilehash: e4c305a7c8c4c4618b0df1e1b2a646356d8f821f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356117"
---
# <a name="converged-nic-in-a-teamed-nic-configuration-datacenter"></a>NIC convergente in una configurazione NIC in gruppo (Datacenter)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono fornite le istruzioni per distribuire una scheda di interfaccia di rete convergente in una configurazione di nic in gruppo con switch \(Embedded Teaming set\). 

La configurazione di esempio in questo argomento descrive due host Hyper-V, **Hyper-v host 1** e **Hyper-v host 2**. Entrambi gli host dispongono di due schede di rete. In ogni host, una scheda è connessa alla subnet 192.168.1. x/24 e una scheda è connessa alla subnet 192.168.2. x/24.

![Host Hyper-V](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Passaggio 1. Testare la connettività tra l'origine e la destinazione

Verificare che la scheda di interfaccia di rete fisica sia in grado di connettersi all'host di destinazione.  Questo test illustra la connettività usando il livello \(3\) L3 o il livello IP, nonché la VLAN\)delle reti \(locali \(virtuali\) di livello 2 L2.

1. Visualizzare le proprietà della prima scheda di rete.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**Risultati**_


   |    Nome    |           InterfaceDescription           | ifIndex | Stato |    macAddress     | LinkSpeed |
   |------------|------------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-1 | Mellanox ConnectX-3 scheda Ethernet Pro |   11    |   Su   | E4-1D-2D-07-43-D0 |  40 Gbps  |

   ---

2. Visualizzare le proprietà aggiuntive per il primo adapter, incluso l'indirizzo IP. 

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-1"
   Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**Risultati**_


   |   Parametro    |    Value    |
   |----------------|-------------|
   |   IPAddress    | 192.168.1.3 |
   | IndiceInterfaccia |     11      |
   | InterfaceAlias | Test-40G-1  |
   | AddressFamily  |    IPv4     |
   |      Type      |   Unicast   |
   |  LunghezzaPrefisso  |     24      |

   ---

3. Visualizzare le proprietà della seconda scheda di rete.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize
   ```

   _**Risultati**_


   |    Nome    |          InterfaceDescription           | ifIndex | Stato |    macAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | TEST-40G-2 | Mellanox ConnectX-3 Pro Ethernet A... #2 |   13    |   Su   | E4-1D-2D-07-40-70 |  40 Gbps  |

   ---

4. Visualizzare le proprietà aggiuntive per la seconda scheda, incluso l'indirizzo IP.

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-2"
   Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**Risultati**_


   |   Parametro    |    Value    |
   |----------------|-------------|
   |   IPAddress    | 192.168.2.3 |
   | IndiceInterfaccia |     13      |
   | InterfaceAlias | TEST-40G-2  |
   | AddressFamily  |    IPv4     |
   |      Type      |   Unicast   |
   |  LunghezzaPrefisso  |     24      |

   ---

5. Verificare che un altro gruppo NIC o membro SET pNICs disponga di un indirizzo IP valido.<p>Usare una subnet separata, \(xxx.xxx. **2**. xxx rispetto a xxx.xxx. **1**. xxx\), per facilitare l'invio da questo adapter alla destinazione. In caso contrario, se si individuano entrambi pNICs sulla stessa subnet, il bilanciamento del carico dello stack TCP/IP di Windows tra le interfacce e la convalida semplice diventa più complessa.


## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Passaggio 2. Verificare che l'origine e la destinazione siano in grado di comunicare

In questo passaggio viene usato il comando **test-NetConnection** di Windows PowerShell, ma se si preferisce è possibile usare il comando **ping** . 

1. Verificare la comunicazione bidirezionale.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Risultati**_


   |        Parametro         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | RTT \(PingReplyDetails\) |    0 ms     |

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


   |        Parametro         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | RTT \(PingReplyDetails\) |    0 ms     |

   ---


4. Verificare la connettività per altre schede di rete. Ripetere i passaggi precedenti per tutti i pNICs successivi inclusi nella scheda di interfaccia di rete o impostare il team.

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**Risultati**_


   |        Parametro         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | Test-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    False    |
   | RTT \(PingReplyDetails\) |    0 ms     |

   ---

## <a name="step-3-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Passaggio 3. Configurare gli ID VLAN per le schede di rete installate negli host Hyper-V

Molte configurazioni di rete usano VLAN e, se si prevede di usare VLAN nella rete, è necessario ripetere il test precedente con VLAN configurate.

Per questo passaggio, le schede di rete sono in modalità di **accesso** . Tuttavia, quando si crea un Commuter \(virtuale Hyper-V vswitch\) più avanti in questa guida, le proprietà VLAN vengono applicate a livello di porta vswitch. 

Poiché un'opzione può ospitare più VLAN, è necessario che il commutatore fisico \(Top\) del rack Tor abbia la porta a cui l'host è connesso configurato in modalità trunk.

>[!NOTE]
>Per istruzioni su come configurare la modalità trunk nel commutatore, vedere la documentazione del commutatore ToR.

Nella figura seguente vengono illustrati due host Hyper-V con due schede di rete ognuna con VLAN 101 e VLAN 102 configurate nelle proprietà della scheda di rete.

![Configurare reti locali virtuali](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)


>[!TIP]
>Secondo gli \(standard di rete IEEE\) Institute of Electrical and Electronics Engineers, la qualità del \(servizio\) proprietà QoS nella scheda di interfaccia di rete fisica agisce sull'intestazione 802.1 p incorporata all'interno dell'intestazione \(VLAN\) 802.1 q quando si configura l'ID VLAN.

1. Configurare l'ID VLAN nella prima scheda di interfaccia di rete, test-40G-1.

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**Risultati**_   


   |    Nome    | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------------|-------------|--------------|-----------------|---------------|
   | TEST-40G-1 |   ID VLAN   |     101      |     VlanID      |     {101}     |

   ---

2. Riavviare la scheda di rete per applicare l'ID VLAN.

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-1"
   ```

3. Verificare che lo stato sia **attivo**.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**Risultati**_


   |    Nome    |          InterfaceDescription           | ifIndex | Stato |    macAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-1 | Mellanox ConnectX-3 Pro Ethernet Ada... |   11    |   Su   | E4-1D-2D-07-43-D0 |  40 Gbps  |

   ---

4. Configurare l'ID VLAN nella seconda scheda di interfaccia di rete, test-40G-2.

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ``` 

   _**Risultati**_


   |    Nome    | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------------|-------------|--------------|-----------------|---------------|
   | TEST-40G-2 |   ID VLAN   |     102      |     VlanID      |     {102}     |

   ---

5. Riavviare la scheda di rete per applicare l'ID VLAN.

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-2" 
   ```

6. Verificare che lo stato sia **attivo**.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**Risultati**_


   |    Nome    |          InterfaceDescription           | ifIndex | Stato |    macAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-2 | Mellanox ConnectX-3 Pro Ethernet Ada... |   11    |   Su   | E4-1D-2D-07-43-D1 |  40 Gbps  |

   ---

   >[!IMPORTANT]
   >Potrebbe essere necessario attendere alcuni secondi prima che il dispositivo venga riavviato e reso disponibile sulla rete. 

7. Verificare la connettività per la prima scheda di interfaccia di rete, test-40G-1.<p>Se la connettività non riesce, controllare la configurazione della VLAN del Commuter o la partecipazione alla destinazione nella stessa VLAN. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Risultati**_   


   |        Parametro         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.5 |
   |      PingSucceeded       |    True     |
   | RTT \(PingReplyDetails\) |    0 ms     |

   ---

8. Verificare la connettività per la prima scheda di interfaccia di rete, test-40G-2.<p>Se la connettività non riesce, controllare la configurazione della VLAN del Commuter o la partecipazione alla destinazione nella stessa VLAN.

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**Risultati**_    


   |        Parametro         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | Test-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    True     |
   | RTT \(PingReplyDetails\) |    0 ms     |

   ---

   >[!IMPORTANT]
   >Non è insolito che si verifichi un errore di **test-NetConnection** o ping subito dopo l'esecuzione di **Restart-NetAdapter**.  Attendere quindi che la scheda di rete venga inizializzata completamente, quindi riprovare.
   >
   >Se le connessioni VLAN 101 funzionano, ma le connessioni VLAN 102 non lo sono, il problema potrebbe essere che il commutire deve essere configurato per consentire il traffico della porta sulla VLAN desiderata. È possibile verificarlo impostando temporaneamente gli adapter non riusciti sulla VLAN 101 e ripetendo il test di connettività.


   La figura seguente Mostra gli host Hyper-V dopo aver configurato correttamente le VLAN.

   ![Configurare la qualità del servizio](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)


## <a name="step-4-configure-quality-of-service-qos"></a>Passaggio 4. Configurare la qualità del \(servizio QoS\)

>[!NOTE]
>È necessario eseguire tutti i passaggi di configurazione DCB e QoS seguenti in tutti gli host che hanno lo scopo di comunicare tra loro.

1. Installare Data Center Bridging \(DCB\) in ogni host Hyper-V.

   - **Facoltativo** per le configurazioni di rete che usano iWarp.
   - **Obbligatorio** per le configurazioni di rete che \(usano roce\) qualsiasi versione per i servizi RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

   _**Risultati**_


   | Riuscito | Riavvio necessario | Codice di chiusura |     Risultato della funzionalità     |
   |---------|----------------|-----------|------------------------|
   |  True   |       No       |  Riuscito  | {Data Center Bridging} |

   ---

2. Impostare i criteri QoS per SMB-Direct:

   - **Facoltativo** per le configurazioni di rete che usano iWarp.
   - **Obbligatorio** per le configurazioni di rete che \(usano roce\) qualsiasi versione per i servizi RDMA.

   Nel comando di esempio seguente il valore "3" è arbitrario. È possibile usare qualsiasi valore compreso tra 1 e 7, purché si usi costantemente lo stesso valore per tutta la configurazione dei criteri QoS.

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**Risultati**_


   |   Parametro    |          Value           |
   |----------------|--------------------------|
   |      Nome      |           SMB            |
   |     Proprietario      | Computer \(criteri di gruppo\) |
   | NetworkProfile |           Tutte            |
   |   Precedenza   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

   ---

3. Impostare criteri QoS aggiuntivi per altro traffico sull'interfaccia.   

   ```PowerShell
   New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
   ```

   _**Risultati**_   


   |   Parametro    |          Value           |
   |----------------|--------------------------|
   |      Nome      |         DEFAULT          |
   |     Proprietario      | Computer \(criteri di gruppo\) |
   | NetworkProfile |           Tutte            |
   |   Precedenza   |           127            |
   |    Modello    |         Predefinito          |
   |   JobObject    |          &nbsp;          |
   | PriorityValue  |            0             |

   ---

4. Attivare il **controllo del flusso di priorità** per il traffico SMB, che non è obbligatorio per iWarp.

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**Risultati**_


   | Priority | Enabled | PolicySet | ifIndex | IfAlias |
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

   >**Importante** Se i risultati non corrispondono a questi risultati perché gli elementi diversi da 3 hanno un valore abilitato true, è necessario disabilitare **FlowControl** per queste classi.
   >
   >```PowerShell
   >Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
   >Get-NetQosFlowControl
   >```
   >In configurazioni più complesse, le altre classi di traffico potrebbero richiedere il controllo di flusso, ma questi scenari non rientrano nell'ambito di questa guida.


5. Abilitare QoS per la prima scheda di interfaccia di rete, test-40G-1.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
   Get-NetAdapterQos -Name "Test-40G-1"

   Name: TEST-40G-1 
   Enabled: True
   ```
   _**Funzionalità**:_   


   |      Parametro      |   Hardware   |   Corrente    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     Nessuno     |     Nessuno     |
   | NumTCs (max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses**:_    


   | TC |  RICONOSCIMENTO TIMESTAMP   | Banda | Priorità |
   |----|--------|-----------|------------|
   | 0  | Completa |  &nbsp;   |    0-7     |

   ---

   _**OperationalFlowControl**:_  

   Priorità 3 abilitata  

   _**OperationalClassifications**:_  


   | Protocol  | Porta/tipo | Priority |
   |-----------|-----------|----------|
   |  Predefinito  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

6. Abilitare QoS per la seconda scheda di interfaccia di rete, test-40G-2.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
   Get-NetAdapterQos -Name "Test-40G-2"

   Name: TEST-40G-2 
   Enabled: True 
   ```

   _**Funzionalità**:_ 


   |      Parametro      |   Hardware   |   Corrente    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     Nessuno     |     Nessuno     |
   | NumTCs (max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses**:_  


   | TC |  RICONOSCIMENTO TIMESTAMP   | Banda | Priorità |
   |----|--------|-----------|------------|
   | 0  | Completa |  &nbsp;   |    0-7     |

   ---

    _**OperationalFlowControl**:_  

    Priorità 3 abilitata  

   _**OperationalClassifications**:_  


   | Protocol  | Porta/tipo | Priority |
   |-----------|-----------|----------|
   |  Predefinito  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---


7. Riservare metà della larghezza di banda \(a SMB diretto RDMA\)

   ```PowerShell
   New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS
   ```

   _**Risultati**_  


   | Nome | Algoritmo | Larghezza di banda (%) | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      50      |    3     |  Globale   | &nbsp;  | &nbsp;  |

   ---

8. Visualizzare le impostazioni di prenotazione della larghezza di banda:   

   ```PowerShell
   Get-NetQosTrafficClass | ft -AutoSize
   ```

   _**Risultati**_  


   |   Nome    | Algoritmo | Larghezza di banda (%) | Priority | PolicySet | ifIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | Predefinita |    ETS    |      50      | 0-2, 4-7  |  Globale   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      50      |    3     |  Globale   | &nbsp;  | &nbsp;  |

   ---

9. Opzionale Creare due classi di traffico aggiuntive per il traffico IP del tenant. 

   >[!TIP]
   >È possibile omettere i valori "IP1" e "IP2".

   ```PowerShell
   New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**Risultati**_


   | Nome | Algoritmo | Larghezza di banda (%) | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP1  |    ETS    |      10      |    1     |  Globale   | &nbsp;  | &nbsp;  |

   ---

   ```PowerShell
   New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**Risultati**_


   | Nome | Algoritmo | Larghezza di banda (%) | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP2  |    ETS    |      10      |    2     |  Globale   | &nbsp;  | &nbsp;  |

   ---

10. Visualizzare le classi di traffico QoS.

    ```PowerShell
    Get-NetQosTrafficClass | ft -AutoSize
    ```

    _**Risultati**_


    |   Nome    | Algoritmo | Larghezza di banda (%) | Priority | PolicySet | ifIndex | IfAlias |
    |-----------|-----------|--------------|----------|-----------|---------|---------|
    | Predefinita |    ETS    |      30      |  0, 4-7   |  Globale   | &nbsp;  | &nbsp;  |
    |    SMB    |    ETS    |      50      |    3     |  Globale   | &nbsp;  | &nbsp;  |
    |    IP1    |    ETS    |      10      |    1     |  Globale   | &nbsp;  | &nbsp;  |
    |    IP2    |    ETS    |      10      |    2     |  Globale   | &nbsp;  | &nbsp;  |

    ---

11. Opzionale Eseguire l'override del debugger.<p>Per impostazione predefinita, il debugger collegato blocca NetQos. 

    ```PowerShell
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger
    ```

    _**Risultati**_  

    ```
    AllowFlowControlUnderDebugger
    -----------------------------
    1
    ```

## <a name="step-5-verify-the-rdma-configuration-mode-1"></a>Passaggio 5. Verificare la modalità di \(configurazione RDMA 1\) 

Si vuole verificare che l'infrastruttura sia configurata correttamente prima di creare un vswitch e passare alla modalità \(RDMA 2\).

La figura seguente mostra lo stato corrente degli host Hyper-V.

![Test RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)


1. Verificare la configurazione RDMA.

   ```PowerShell
   Get-NetAdapterRdma | ft -AutoSize
   ```

   _**Risultati**_


   |    Nome    |        InterfaceDescription        | Enabled |
   |------------|------------------------------------|---------|
   | TEST-40G-1 | #2 Mellanox ConnectX-4 adapter VPI |  True   |
   | TEST-40G-2 |  Mellanox ConnectX-4 VPI adapter   |  True   |

   ---

2. Determinare il valore **ifindex** degli adattatori di destinazione.<p>Questo valore viene usato nei passaggi successivi quando si esegue lo script scaricato.   

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**Risultati**_


   | InterfaceAlias | IndiceInterfaccia |  Indirizzo IPv4  |
   |----------------|----------------|---------------|
   |   TEST-40G-1   |       14       | 192.168.1.3 |
   |   TEST-40G-2   |       13       | {192.168.2.3} |

   ---

3. Scaricare l' [utilità DiskSpd. exe](https://aka.ms/diskspd) ed estrarla in C:\test\.

4. Scaricare lo [script di PowerShell test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) in una cartella di test nell'unità locale, ad esempio C:\test\.

5. Eseguire lo script di PowerShell **test-RDMA. ps1** per passare il valore ifindex allo script, insieme all'indirizzo IP della prima scheda remota nella stessa VLAN.<p>In questo esempio lo script passa il valore **ifindex** 14 nell'indirizzo IP della scheda di rete remota 192.168.1.5.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Risultati**_ 

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
   >Se il traffico RDMA ha esito negativo, per il caso RoCE, consultare la configurazione del commutatore ToR per le impostazioni appropriate di PFC/ETS che devono corrispondere alle impostazioni host. Per i valori di riferimento, vedere la sezione QoS in questo documento.

6. Eseguire lo script di PowerShell **test-RDMA. ps1** per passare il valore ifindex allo script, insieme all'indirizzo IP della seconda scheda remota nella stessa VLAN.<p>In questo esempio lo script passa il valore **ifindex** 13 nell'indirizzo IP della scheda di rete remota 192.168.2.5.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Risultati**_ 

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

## <a name="step-6-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Passaggio 6. Creare un vSwitch Hyper-V negli host Hyper-V


La figura seguente Mostra Hyper-V host 1 con vSwitch.

![Creazione di un Commuter virtuale Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

1. Creare un vSwitch in modalità SET in Hyper-V host 1.

   ```PowerShell
   New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
   ```

   _**Risultato**_


   |  Nome   | SwitchType | Parametro netadapterinterfacedescription |
   |---------|------------|--------------------------------|
   | VMSTEST |  Esterno  |        Gruppo-interfaccia        |

   ---

2. Visualizzare il gruppo di schede fisiche nel SET.

   ```PowerShell
   Get-VMSwitchTeam -Name "VMSTEST" | fl
   ```

   _**Risultati**_  

   ```
   Name: VMSTEST  
   Id: ad9bb542-dda2-4450-a00e-f96d44bdfbec  
   NetAdapterInterfaceDescription: {Mellanox ConnectX-3 Pro Ethernet Adapter, Mellanox ConnectX-3 Pro Ethernet Adapter #2}  
   TeamingMode: SwitchIndependent  
   LoadBalancingAlgorithm: Dynamic   
   ```

3. Visualizza due visualizzazioni dell'host vNIC

   ```PowerShell
    Get-NetAdapter
   ```

   _**Risultati**_


   |        Nome         |        InterfaceDescription         | ifIndex | Stato |    macAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (VMSTEST) | Scheda Ethernet virtuale Hyper-V #2 |   28    |   Su   | E4-1D-2D-07-40-71 |  80 Gbps  |

   ---

4. Visualizzare le proprietà aggiuntive dell'host vNIC. 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Risultati**_


   |  Nome   | Gestione dei | VMName  |  SwitchName  | macAddress | Stato | IPAddresses |
   |---------|----------------|---------|--------------|------------|--------|-------------|
   | VMSTEST |      True      | VMSTEST | E41D2D074071 |    OK    | &nbsp; |             |

   ---


5. Testare la connessione di rete alla scheda VLAN 101 remota.

   ```PowerShell
   Test-NetConnection 192.168.1.5 
   ```

   _**Risultati**_  

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

In questo passaggio viene rimossa l'impostazione della VLAN di accesso dalla scheda di interfaccia di rete fisica e viene impostato il VLANID usando vSwitch.

È necessario rimuovere l'impostazione della VLAN di accesso per impedire che il traffico in uscita venga taggato automaticamente con l'ID VLAN errato e che il filtro in ingresso non corrisponda all'ID VLAN di accesso.

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

   _**Risultati**_  

   ```
   VMName VMNetworkAdapterName Mode   VlanList
   ------ -------------------- ----   --------
          VMSTEST              Access 101     
   ```


3. Testare la connessione di rete.

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

   >**Importante** Se i risultati non sono simili a quelli dell'esempio, il ping ha esito negativo e viene visualizzato il messaggio "avviso: Il ping per 192.168.1.5 non è riuscito. stato: DestinationHostUnreachable, "verificare che il" vEthernet (VMSTEST) "disponga dell'indirizzo IP appropriato.
   >
   >```PowerShell
   >Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
   >```
   >
   >Se l'indirizzo IP non è impostato, risolvere il problema.
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


4. Rinominare la scheda di interfaccia di rete di gestione.

   ```PowerShell
   Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Risultati**_ 


   |         Nome         | Gestione dei | VMName |      SwitchName      |  macAddress  | Stato | IPAddresses |
   |----------------------|----------------|--------|----------------------|--------------|--------|-------------|
   | CORP-External-switch |      True      | &nbsp; | CORP-External-switch | 001B785768AA |  OK  |   &nbsp;    |
   |         ENERGIA          |      True      | &nbsp; |       VMSTEST        | E41D2D074071 |  OK  |   &nbsp;    |

   ---

5. Visualizzare le proprietà aggiuntive della scheda di interfaccia di rete.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Risultati**_


   |      Nome       |        InterfaceDescription         | ifIndex | Stato |    macAddress     | LinkSpeed |
   |-----------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (gest) | Scheda Ethernet virtuale Hyper-V #2 |   28    |   Su   | E4-1D-2D-07-40-71 |  80 Gbps  |

   ---

## <a name="step-8-test-hyper-v-vswitch-rdma"></a>Passaggio 8. Test di Hyper-V vSwitch RDMA

La figura seguente mostra lo stato corrente degli host Hyper-V, incluso vSwitch in Hyper-V host 1.

![Testare il Commuter virtuale Hyper-V](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

1. Impostare l'assegnazione di tag di priorità nell'host vNIC per integrare le impostazioni VLAN precedenti.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
   ```

   _**Risultati**_  

   nome ENERGIA  
   IeeePriorityTag :  Attivato  

2. Creare due host schede per RDMA e connetterli a vSwitch VMSTEST.

   ```PowerShell    
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
   ```

3. Visualizzare le proprietà della scheda di interfaccia di rete di gestione.

   ```PowerShell    
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Risultati**_ 


   |         Nome         | Gestione dei |        VMName        |  SwitchName  | macAddress | Stato | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-External-switch |      True      | CORP-External-switch | 001B785768AA |    OK    | &nbsp; |             |
   |         Energia          |      True      |       VMSTEST        | E41D2D074071 |    OK    | &nbsp; |             |
   |         SMB1         |      True      |       VMSTEST        | 00155D30AA00 |    OK    | &nbsp; |             |
   |         SMB2         |      True      |       VMSTEST        | 00155D30AA01 |    OK    | &nbsp; |             |

   ---

## <a name="step-9-assign-an-ip-address-to-the-smb-host-vnics-vethernet-smb1-and-vethernet-smb2"></a>Passaggio 9: Assegnare un indirizzo IP all'host SMB schede vEthernet \(SMB1\) e vEthernet \(SMB2\)

Per le schede fisiche TEST-40G-1 e TEST-40G-2 è ancora stata configurata una VLAN di accesso di 101 e 102. Per questo motivo, gli adapter contrassegnano il traffico e il ping ha esito positivo. In precedenza, sono stati configurati entrambi gli ID VLAN installare su zero, quindi la vSwitch VMSTEST è stata impostata su VLAN 101. Successivamente, è ancora possibile eseguire il ping della scheda VLAN 101 remota usando il vNIC di gestione del computer, ma attualmente non ci sono membri VLAN 102.



1. Rimuovere l'impostazione della VLAN di accesso dalla scheda di interfaccia di rete fisica per evitare che sia stata contrassegnata automaticamente il traffico in uscita con l'ID VLAN errato ed evitare di filtrare il traffico in ingresso che non corrisponde all'ID VLAN di accesso.

   ```PowerShell    
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
   ```

   _**Risultati**_  

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

2. Testare la scheda VLAN 102 remota.

   ```PowerShell
   Test-NetConnection 192.168.2.5 
   ```

   _**Risultati**_  

   ```
   ComputerName   : 192.168.2.5
   RemoteAddress  : 192.168.2.5
   InterfaceAlias : vEthernet (SMB1)
   SourceAddress  : 192.168.2.111
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```

3. Aggiungere un nuovo indirizzo IP per l'interfaccia \(vEthernet\)SMB2.

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
   ```

   _**Risultati**_ 

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

4. Testare di nuovo la connessione.    


5. Inserire il schede host RDMA nella VLAN 102 esistente.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS

   Get-VMNetworkAdapterVlan -ManagementOS
   ```

   _**Risultati**_ 

   ```   
   VMName VMNetworkAdapterName Mode VlanList
   ------ -------------------- ---- --------
      SMB1 Access   102 
      Mgt  Access   101 
      SMB2 Access   102 
      CORP-External-Switch Untagged
   ```

6. Esaminare il mapping tra SMB1 e SMB2 e le schede di rete fisiche sottostanti nel gruppo vSwitch SET.<p>L'associazione di vNIC host a schede di rete fisiche è casuale e soggetta a ribilanciamento durante la creazione e la distruzione. In questa circostanza, è possibile utilizzare un meccanismo indiretto per controllare l'associazione corrente. Gli indirizzi MAC di SMB1 e SMB2 sono associati al membro del team NIC TEST-40G-2. Questa situazione non è ideale perché test-40G-1 non ha un vNIC host SMB associato e non consente l'utilizzo del traffico RDMA sul collegamento fino a quando non viene eseguito il mapping di un host SMB vNIC.

   ```PowerShell    
   Get-NetAdapterVPort (Preferred)

   Get-NetAdapterVmqQueue
   ```

   _**Risultati**_ 

   ```
   Name   QueueID MacAddressVlanID Processor VmFriendlyName
   ----   ------- ---------------- --------- --------------
   TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
   TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
   TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
   ```

7. Visualizzare le proprietà della scheda di rete della macchina virtuale.

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Risultati**_ 

   ```
   Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
   ---- -------------- ------ ----------   ----------   ------ -----------
   CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
   Mgt  True  VMSTEST  E41D2D074071 {Ok}  
   SMB1 True  VMSTEST  00155D30AA00 {Ok}  
   SMB2 True  VMSTEST  00155D30AA01 {Ok}  
   ```

8. Visualizzare il mapping del gruppo di schede di rete.<p>I risultati non devono restituire informazioni perché non è ancora stato eseguito il mapping.

   ```PowerShell
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
   ```


9. Eseguire il mapping di SMB1 e SMB2 per separare i membri del gruppo NIC fisico e visualizzare i risultati delle azioni.

   >[!IMPORTANT]
   >Assicurarsi di completare questo passaggio prima di procedere. in alternativa, l'implementazione non riesce.

   ```PowerShell
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"

   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
   ```

   _**Risultati**_ 

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

10. Confermare le associazioni MAC create in precedenza.

    ```PowerShell    
    Get-NetAdapterVmqQueue
    ```

    _**Risultati**_ 

    ```   
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    ```


11. Testare la connessione dal sistema remoto perché entrambi schede host si trovano nella stessa subnet e hanno lo stesso ID \(VLAN 102.\)

    ```PowerShell 
    Test-NetConnection 192.168.2.111
    ```

    _**Risultati**_   

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

    _**Risultati**_   

    ```
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    ```
12. Impostare il nome, il nome dell'opzione e i tag di priorità.   

    ```PowerShell
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    ```

    _**Risultati**_   

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

13. Visualizzare le proprietà della scheda di rete vEthernet.

    ```PowerShell
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    ```

    _**Risultati**_   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    ```

14. Abilitare le schede di rete vEthernet.  

    ```PowerShell
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    ```

    _**Risultati**_   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

## <a name="step-10-validate-the-rdma-functionality"></a>Passaggio 10. Convalidare la funzionalità RDMA.

Si vuole convalidare la funzionalità RDMA dal sistema remoto al sistema locale, che ha un vSwitch, a entrambi i membri del team vSwitch SET.<p>Poiché entrambi gli host \(schede SMB1 e\) SMB2 sono assegnati alla VLAN 102, è possibile selezionare la scheda VLAN 102 nel sistema remoto. <p>In questo esempio, la scheda di interfaccia di rete test-40G-2 esegue RDMA in SMB1 (192.168.2.111) e SMB2 (192.168.2.222).

>[!TIP]
>Potrebbe essere necessario disabilitare il firewall in questo sistema.  Per informazioni dettagliate, vedere i criteri di infrastruttura.
>
>```PowerShell
>Set-NetFirewallProfile -All -Enabled False
>    
>Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
>```
>
>_**Risultati**_ 
>   
>```
>Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
>----  -----------------------   --------------- -------------  
> .
> .
>Test-40G-2VLAN ID102VlanID  {102} 
>```

1. Visualizzare le proprietà della scheda di rete.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Risultati**_ 

   ```
   Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
   ----  --------------------------- ------   ---------- ---------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
   ```

2. Visualizzare le informazioni di RDMA sulla scheda di rete.

   ```PowerShell
   Get-NetAdapterRdma
   ```

   _**Risultati**_  

   ```
   Name  InterfaceDescription Enabled
   ----  -------------------- -------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
   ```

3. Eseguire il test del traffico RDMA sulla prima scheda fisica.

   ```PowerShell 
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Risultati**_ 

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

4. Eseguire il test del traffico RDMA sulla seconda scheda fisica.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Risultati**_ 

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

5. Testare il traffico RDMA dal computer locale al computer remoto.

    ```PowerShell
    Get-NetAdapter | ft –AutoSize
    ```

    _**Risultati**_ 

    ```
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    ```

6. Eseguire il test del traffico RDMA sulla prima scheda virtuale.    

   ```
   C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Risultati**_ 

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

7. Eseguire il test del traffico RDMA sulla seconda scheda virtuale.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Risultati**_ 

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

La riga finale in questo output "RDMA Traffic test ha ESITo positivo: Il traffico RDMA è stato inviato a 192.168.2.5 "indica che è stata configurata correttamente la scheda di interfaccia di rete convergente nella scheda.

## <a name="related-topics"></a>Argomenti correlati 

- [Configurazione NIC convergente con una singola scheda di rete](cnic-single.md)
- [Configurazione del Commuter fisico per NIC convergente](cnic-app-switch-config.md)
- [Risoluzione dei problemi relativi alle configurazioni NIC convergenti](cnic-app-troubleshoot.md)
