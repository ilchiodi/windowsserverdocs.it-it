---
title: Risoluzione dei problemi convergente configurazioni NIC
description: In questo argomento fa parte di convergenza NIC Configuration Guide per Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 373ecd9b9fff62aaabd8caa176ff091ec98ad81c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshooting-converged-nic-configurations"></a>Risoluzione dei problemi convergente configurazioni NIC

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare lo script seguente per verificare se la configurazione di RDMA è corretta nell'host Hyper-V.

- [Scaricare i file con estensione ps1 Test-Rdma script](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

È anche possibile utilizzare i seguenti comandi di Windows PowerShell per risolvere i problemi e verificare la configurazione del tuo schede di rete convergente.

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

Per verificare la configurazione della scheda di rete RDMA, eseguire il comando seguente di Windows PowerShell nel server Hyper-V.

    
    Get-NetAdapterRdma | fl *
    

Per identificare e risolvere i problemi dopo aver eseguito questo comando nell'host Hyper-V, è possibile utilizzare i seguenti previsto e risultati imprevisti.

### <a name="get-netadapterrdma-expected-results"></a>Risultati previsti Get-NetAdapterRdma

Scheda di rete host e la scheda NIC fisica Mostra funzionalità RDMA diverso da zero.

![Risultati di Windows PowerShell previsto](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma risultati imprevisti

Eseguire i passaggi seguenti se si ricevano risultati imprevisti quando si esegue il **Get-NetAdapterRdma** comando.

1. Assicurarsi che il miniport Mlnx e driver bus Mlnx sono più recenti. Per Mellanox, utilizzare drop almeno 42. 
2. Verificare che i driver miniport e bus Mlnx corrispondano controllando la versione del driver tramite Gestione dispositivi. Il driver del bus è reperibile in dispositivi di sistema. Il nome deve iniziare con Mellanox Connect-X 3 PRO VPI, come illustrato nella schermata seguente di proprietà delle schede di rete.

![Proprietà delle schede di rete](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. Assicurati di rete diretto (RDMA) è abilitato sul NIC fisica e vNIC host.
5. Assicurarsi che vSwitch viene creato tramite la scheda fisica adeguati controllando le funzionalità RDMA.
6. Controllare il Registro di sistema Viewer e filtrare in base a origine "Hyper-V-VmSwitch".

## <a name="get-smbclientnetworkinterface"></a>Ottenere SmbClientNetworkInterface

Come passaggio aggiuntivo per verificare la configurazione di RDMA, eseguire il comando seguente di Windows PowerShell nel server Hyper-V.


    Get SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Ottenere risultati SmbClientNetworkInterface previsto

VNIC host dovrebbe essere visualizzato come in grado di supportare RDMA dal punto di vista di SMB anche.

![Proprietà delle schede di rete](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>SmbClientNetworkInterface di ottenere risultati imprevisti

1. Assicurarsi che il miniport Mlnx e driver bus Mlnx sono più recenti. Per Mellanox, utilizzare drop almeno 42. 
2. Verificare che i driver miniport e bus Mlnx corrispondano controllando la versione del driver tramite Gestione dispositivi. Il driver del bus è reperibile in dispositivi di sistema. Il nome deve iniziare con Mellanox Connect-X 3 PRO VPI, come illustrato nella schermata seguente di proprietà delle schede di rete.
3. Assicurati di rete diretto (RDMA) è abilitato sul NIC fisica e vNIC host.
4. Assicurarsi che il commutatore virtuale Hyper-V viene creato tramite la scheda fisica adeguati controllando le funzionalità RDMA.
5. Controllare i registri Viewer "SMB Client" in **servizi e applicazioni | Microsoft | Windows**.

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

È possibile visualizzare la qualità della scheda di rete di configurazione del servizio \(QoS\) eseguendo il comando seguente di Windows PowerShell.

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Risultati previsti Get-NetAdapterQos

Priorità e classi di traffico devono essere visualizzate in base al primo passaggio di configurazione che è eseguita utilizzando questa Guida.

![Qualità del servizio priorità e classi](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-NetAdapterQos risultati imprevisti

Se i risultati imprevisti, eseguire i passaggi seguenti.

1. Assicurarsi che la scheda di rete fisica supporta \(DCB\) Data Center Bridging e QoS
2. Assicurarsi che i driver di scheda di rete siano aggiornati.


## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

È possibile utilizzare il comando di Windows PowerShell seguente per verificare che l'indirizzo IP del nodo remoto sia in grado di supportare RDMA\.

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Risultati previsti Get-SmbMultiChannelConnection

Indirizzo IP del nodo remoto viene visualizzato come RDMA in grado di supportare.

![Indirizzo IP di nodo remoto in grado di supportare RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Get-SmbMultiChannelConnection risultati imprevisti

Se i risultati imprevisti, eseguire i passaggi seguenti.

1. Assicurarsi che il comando ping funziona in entrambe le direzioni.
2. Assicurarsi che il firewall non blocca l'avvio della connessione SMB. In particolare, attivare la regola del firewall per la porta SMB diretto 5445 per iWARP e 445 per ROCE.

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

È possibile utilizzare il comando seguente per verificare che la scheda di rete virtuale è abilitato per RDMA è segnalato come in grado di supportare RDMA\ da SMB.

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Risultati previsti Get-SmbClientNetworkInterface

NIC virtuale che è stato abilitato per RDMA devono essere considerate come in grado di supportare RDMA da SMB.

![SMB segnala che le schede NIC sono in grado di supportare RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface risultati imprevisti

Se i risultati imprevisti, eseguire i passaggi seguenti.

1. Assicurarsi che il comando ping funziona in entrambe le direzioni.
2. Assicurarsi che i firewall non blocca l'avvio della connessione SMB.

## <a name="vstat-mellanox-specific"></a>Vstat \(Mellanox specific\)

Se si utilizzano schede di rete Mellanox, è possibile utilizzare il **vstat** comando per verificare il RDMA sulla versione \(RoCE\) Converged Ethernet sui nodi Hyper-V.

### <a name="vstat-expected-results"></a>Risultati vstat previsto

La versione RoCE in entrambi i nodi deve essere lo stesso. Questo è un buon modo per verificare che la versione del firmware in entrambi i nodi sia più recente.

![Esempi di risultato RoCE versione controllo](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>Vstat risultati imprevisti

Se i risultati imprevisti, eseguire i passaggi seguenti.

1. Versione RoCE corretta set utilizzando Set-MlnxDriverCoreSetting
2. Installare il firmware più recente dal sito Web Mellanox.


## <a name="perfmon-counters"></a>Contatori Perfmon

È possibile esaminare i contatori in Performance Monitor per verificare l'attività RDMA della configurazione.

![Esempi di risultato di monitoraggio delle prestazioni](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

## <a name="all-topics-in-this-guide"></a>Tutti gli argomenti in questa Guida

Questa guida contiene gli argomenti seguenti.

- [Configurazione di rete convergente con una sola scheda di rete](cnic-single.md)
- [Configurazione di rete convergente le NIC combinate](cnic-datacenter.md)
- [Configurazione di commutatore fisico per scheda di rete convergente](cnic-app-switch-config.md)
- [Risoluzione dei problemi convergente configurazioni NIC](cnic-app-troubleshoot.md)
