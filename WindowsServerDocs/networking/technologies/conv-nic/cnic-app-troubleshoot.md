---
title: Risoluzione dei problemi relativi alle configurazioni NIC convergenti
description: Questo argomento fa parte della Guida alla configurazione della NIC convergente per Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c20c21c39e44d7eb3da812bbe71f175d0688d6c0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309650"
---
# <a name="troubleshooting-converged-nic-configurations"></a>Risoluzione dei problemi relativi alle configurazioni NIC convergenti

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare lo script seguente per verificare se la configurazione RDMA è corretta nell'host Hyper-V.

- [Scaricare lo script Test-Rdma. ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

È anche possibile usare i comandi di Windows PowerShell seguenti per risolvere i problemi e verificare la configurazione delle NIC convergenti.

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

Per verificare la configurazione della scheda di rete RDMA, eseguire il comando di Windows PowerShell seguente nel server Hyper-V.

    
    Get-NetAdapterRdma | fl *
    

È possibile utilizzare i risultati previsti e imprevisti seguenti per identificare e risolvere i problemi dopo l'esecuzione di questo comando nell'host Hyper-V.

### <a name="get-netadapterrdma-expected-results"></a>Risultati previsti per Get-NetAdapterRdma

VNIC host e la scheda di interfaccia di rete fisica mostrano funzionalità RDMA diverse da zero.

![Risultati previsti di Windows PowerShell](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Risultati imprevisti di Get-NetAdapterRdma

Quando si esegue il comando **Get-NetAdapterRdma** , seguire questa procedura se si ricevono risultati imprevisti.

1. Verificare che i driver MLNX miniport e MLNX bus siano più recenti. Per Mellanox, usare almeno drop 42. 
2. Verificare che i driver MLNX miniport e bus corrispondano controllando la versione del driver tramite Device Manager. Il driver bus si trova nei dispositivi di sistema. Il nome deve iniziare con Mellanox Connect-X 3 PRO VPI, come illustrato nello screenshot seguente delle proprietà della scheda di rete.

![Proprietà delle schede di rete](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. Verificare che la rete diretta (RDMA) sia abilitata sia nella scheda di interfaccia di rete fisica che nell'host vNIC.
5. Verificare che vSwitch sia stato creato sulla scheda fisica corretta controllando le funzionalità di RDMA.
6. Controllare il registro di sistema di EventViewer e filtrare per origine "Hyper-V-VmSwitch".

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Come passaggio aggiuntivo per verificare la configurazione di RDMA, eseguire il comando di Windows PowerShell seguente nel server Hyper-V.


    Get-SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Risultati previsti per Get-SmbClientNetworkInterface

L'host vNIC dovrebbe apparire come RDMA in grado di supportare anche la prospettiva SMB.

![Proprietà delle schede di rete](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Risultati imprevisti di Get-SmbClientNetworkInterface

1. Verificare che i driver MLNX miniport e MLNX bus siano più recenti. Per Mellanox, usare almeno drop 42. 
2. Verificare che i driver MLNX miniport e bus corrispondano controllando la versione del driver tramite Device Manager. Il driver bus si trova nei dispositivi di sistema. Il nome deve iniziare con Mellanox Connect-X 3 PRO VPI, come illustrato nello screenshot seguente delle proprietà della scheda di rete.
3. Verificare che la rete diretta (RDMA) sia abilitata sia nella scheda di interfaccia di rete fisica che nell'host vNIC.
4. Verificare che il commutire virtuale Hyper-V sia stato creato sulla scheda fisica corretta controllando le relative funzionalità RDMA.
5. Controllare i registri di EventViewer per "client SMB" in **Application and Services | Microsoft | Windows**.

--- 

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

È possibile visualizzare la qualità del servizio della scheda di rete \(la configurazione QoS\) eseguendo il comando di Windows PowerShell seguente.

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Risultati previsti per Get-NetAdapterQos

Le priorità e le classi di traffico devono essere visualizzate in base al primo passaggio di configurazione eseguito con questa guida.

![Priorità e classi di qualità del servizio](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Risultati imprevisti di Get-NetAdapterQos

Se i risultati sono imprevisti, seguire questa procedura.

1. Verificare che la scheda di rete fisica supporti Data Center Bridging \(DCB\) e QoS
2. Verificare che i driver della scheda di rete siano aggiornati.

--- 

## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

È possibile utilizzare il comando di Windows PowerShell seguente per verificare che l'indirizzo IP del nodo remoto sia RDMA\-in grado di supportare.

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Risultati previsti per Get-SmbMultiChannelConnection

L'indirizzo IP del nodo remoto viene visualizzato come RDMA in grado di supportare.

![Indirizzo IP del nodo remoto compatibile con RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Risultati imprevisti di Get-SmbMultiChannelConnection

Se i risultati sono imprevisti, seguire questa procedura.

1. Assicurarsi che ping funzioni in entrambi i modi.
2. Verificare che il firewall non blocchi l'avvio della connessione SMB. In particolare, abilitare la regola del firewall per la porta SMB diretta 5445 per iWARP e 445 per ROCE.

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

È possibile usare il comando seguente per verificare che la scheda di interfaccia di rete virtuale abilitata per RDMA sia indicata come RDMA\-in grado di supportare SMB.

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Risultati previsti per Get-SmbClientNetworkInterface

La scheda di interfaccia di rete virtuale abilitata per RDMA deve essere considerata RDMA in grado di supportare SMB.

![SMB segnala che le schede di rete sono compatibili con RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Risultati imprevisti di Get-SmbClientNetworkInterface

Se i risultati sono imprevisti, seguire questa procedura.

1. Assicurarsi che ping funzioni in entrambi i modi.
2. Verificare che il firewall non blocchi l'avvio della connessione SMB.

--- 

## <a name="vstat-mellanox-specific"></a>vstat \(\) specifico di Mellanox

Se si usano schede di rete Mellanox, è possibile usare il comando **vstat** per verificare la versione di RDMA over Converged Ethernet \(roce\) nei nodi Hyper-V.

### <a name="vstat-expected-results"></a>risultati previsti vstat

La versione di RoCE in entrambi i nodi deve essere la stessa. Questo è anche un modo efficace per verificare che la versione del firmware in entrambi i nodi sia la più recente.

![Esempi di risultati del controllo della versione di RoCE](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>risultati imprevisti di vstat

Se i risultati sono imprevisti, seguire questa procedura.

1. Impostare la versione corretta di RoCE con set-MlnxDriverCoreSetting
2. Installare il firmware più recente dal sito Web Mellanox.

--- 

## <a name="perfmon-counters"></a>Contatori PerfMon

È possibile esaminare i contatori in Performance Monitor per verificare l'attività RDMA della configurazione.

![Esempi di risultati di performance monitor](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

--- 

## <a name="related-topics"></a>Argomenti correlati

- [Configurazione NIC convergente con una singola scheda di rete](cnic-single.md)
- [Configurazione NIC convergente con gruppo NIC](cnic-datacenter.md)
- [Configurazione del Commuter fisico per NIC convergente](cnic-app-switch-config.md)

---
