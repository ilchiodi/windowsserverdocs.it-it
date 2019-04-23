---
title: Risoluzione dei problemi di configurazioni di interfaccia di rete convergente
description: Questo argomento fa parte di convergente NIC Configuration Guide per Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3004ff9d6fe874410c24d174755a6d26f99f8f2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876482"
---
# <a name="troubleshooting-converged-nic-configurations"></a>Risoluzione dei problemi di configurazioni di interfaccia di rete convergente

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile usare lo script seguente per verificare se la configurazione di RDMA è corretta nell'host Hyper-V.

- [Scaricare script di Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

È anche possibile usare i comandi di Windows PowerShell seguenti per risolvere i problemi e verificare la configurazione delle schede di rete convergente.

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

Per verificare la configurazione di RDMA scheda di rete, eseguire il comando seguente di Windows PowerShell nel server Hyper-V.

    
    Get-NetAdapterRdma | fl *
    

Per identificare e risolvere i problemi dopo aver eseguito questo comando nell'host Hyper-V, è possibile utilizzare la seguente previsto e risultati imprevisti.

### <a name="get-netadapterrdma-expected-results"></a>Risultati di Get-NetAdapterRdma previsto

VNIC host e l'interfaccia di rete fisica mostrano le funzionalità RDMA diverso da zero.

![Risultati di Windows PowerShell previsto](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma risultati imprevisti

Eseguire la procedura seguente se si ricevono risultati imprevisti quando si esegue la **Get-NetAdapterRdma** comando.

1. Assicurarsi che il Mlnx miniport e driver bus Mlnx sono più recenti. Per Mellanox, usare drop almeno 42. 
2. Verificare che i driver miniport e bus Mlnx corrispondano controllando la versione del driver tramite Gestione dispositivi. Il driver di bus è reperibile in dispositivi di sistema. Il nome deve iniziare con Mellanox Connect-X 3 PRO VPI, come illustrato nella schermata riportata di seguito di proprietà scheda di rete.

![Proprietà delle schede di rete](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. Assicurarsi che rete diretto (RDMA) è abilitato nel vNIC host sia interfaccia di rete fisica.
5. Assicurarsi che sia stata creata tramite l'adapter fisico adeguati vSwitch controllando le funzionalità RDMA.
6. Controllare il Registro di sistema di Visualizzatore eventi e filtrare in base origine "Hyper-V-VmSwitch".

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Come passaggio aggiuntivo per verificare la configurazione di RDMA, eseguire il comando seguente di Windows PowerShell nel server Hyper-V.


    Get-SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Risultati di Get-SmbClientNetworkInterface previsto

Dal punto di vista del SMB anche, vNIC host compariranno come supporto per RDMA.

![Proprietà delle schede di rete](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface risultati imprevisti

1. Assicurarsi che il Mlnx miniport e driver bus Mlnx sono più recenti. Per Mellanox, usare drop almeno 42. 
2. Verificare che i driver miniport e bus Mlnx corrispondano controllando la versione del driver tramite Gestione dispositivi. Il driver di bus è reperibile in dispositivi di sistema. Il nome deve iniziare con Mellanox Connect-X 3 PRO VPI, come illustrato nella schermata riportata di seguito di proprietà scheda di rete.
3. Assicurarsi che rete diretto (RDMA) è abilitato nel vNIC host sia interfaccia di rete fisica.
4. Assicurarsi che il commutatore virtuale Hyper-V viene creato tramite l'adapter fisico adeguati controllando le funzionalità RDMA.
5. Controllare i log di Visualizzatore eventi "SMB Client" in **servizi e le applicazioni | Microsoft | Windows**.

--- 

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

È possibile visualizzare la qualità di scheda di rete del servizio \(QoS\) configurazione eseguendo il comando seguente di Windows PowerShell.

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Risultati di Get-NetAdapterQos previsto

Le priorità e classi di traffico dovrebbero essere visualizzate in base al primo passaggio di configurazione eseguite utilizzando questa Guida.

![Qualità delle classi e le priorità del servizio](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-NetAdapterQos risultati imprevisti

Se i risultati imprevisti, eseguire la procedura seguente.

1. Assicurarsi che la scheda di rete fisica supporti Data Center Bridging \(DCB\) e QoS
2. Assicurarsi che il driver di schede di rete siano aggiornati.

--- 

## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

È possibile usare il comando Windows PowerShell seguente per verificare che l'indirizzo IP del nodo remoto sia RDMA\-in grado di supportare.

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Risultati di Get-SmbMultiChannelConnection previsto

Indirizzo IP del nodo remoto viene visualizzato come RDMA in grado di supportare.

![Indirizzo IP nodo remoto che supporta RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Get-SmbMultiChannelConnection risultati imprevisti

Se i risultati imprevisti, eseguire la procedura seguente.

1. Assicurarsi che il comando ping funziona in entrambe le direzioni.
2. Assicurarsi che il firewall non stia bloccando avvio della connessione SMB. In particolare, abilitare la regola del firewall per la porta SMB diretto 5445 per iWARP e 445 per ROCE.

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

È possibile usare il comando seguente per verificare che l'interfaccia di rete virtuale è abilitata per RDMA è segnalato come RDMA\-in grado di supportare per SMB.

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Risultati di Get-SmbClientNetworkInterface previsto

Scheda di rete virtuale che è stata abilitata per RDMA debba essere considerata come supporto per RDMA da SMB.

![SMB segnala che le schede NIC sono che supporta RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface risultati imprevisti

Se i risultati imprevisti, eseguire la procedura seguente.

1. Assicurarsi che il comando ping funziona in entrambe le direzioni.
2. Assicurarsi che i firewall non blocchi l'avvio della connessione SMB.

--- 

## <a name="vstat-mellanox-specific"></a>vstat \(Mellanox specifici\)

Se si utilizzano schede di rete Mellanox, è possibile usare la **vstat** comando per verificare il RDMA over Converged Ethernet \(RoCE\) versione sui nodi Hyper-V.

### <a name="vstat-expected-results"></a>risultati vstat previsto

La versione RoCE in entrambi i nodi deve essere lo stesso. Questo è un buon metodo per verificare che la versione del firmware in entrambi i nodi sia più recente.

![Esempi di risultati RoCE versione controllo](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>vstat risultati imprevisti

Se i risultati imprevisti, eseguire la procedura seguente.

1. Impostare versione RoCE corretta tramite Set-MlnxDriverCoreSetting
2. Installare il firmware più recente dal sito Web Mellanox.

--- 

## <a name="perfmon-counters"></a>Contatori Perfmon

È possibile esaminare i contatori in Performance Monitor per verificare l'attività RDMA della configurazione.

![Esempi di risultato di monitoraggio delle prestazioni](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

--- 

## <a name="related-topics"></a>Argomenti correlati

- [Configurazione scheda di rete convergente con una sola scheda di rete](cnic-single.md)
- [Configurazione di interfaccia di rete convergente le NIC](cnic-datacenter.md)
- [Configurazione di commutatore fisico per la scheda di rete convergente](cnic-app-switch-config.md)

---
