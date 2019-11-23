---
title: Pianificare l'uso di vRSS
description: È possibile utilizzare questo argomento per preparare la macchina virtuale e l'host Hyper-V per l'utilizzo di vRSS in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: 3addb500a654ff9f23c56388fec3ef2c3855a1da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395815"
---
# <a name="plan-the-use-of-vrss"></a>Pianificare l'uso di vRSS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In Windows Server 2016, vRSS è abilitato per impostazione predefinita, tuttavia è necessario preparare l'ambiente per consentire il corretto funzionamento di vRSS in una macchina virtuale \(VM\) o in una scheda virtuale host \(vNIC\). In Windows Server 2012 R2, vRSS è stato disabilitato per impostazione predefinita.

Quando si pianifica e si prepara l'uso di vRSS, verificare quanto segue:

- La scheda di rete fisica è compatibile con Coda macchine virtuali \(VMQ\) e ha una velocità di collegamento pari o superiore a 10 Gbps.
- VMQ è abilitato sulla scheda di interfaccia di rete fisica e sulla porta del Commuter virtuale Hyper\-V
- Non esiste un singolo input radice\-output Virtualization \(SR\-IOV\) configurato per la macchina virtuale.
- Gruppo NIC configurato correttamente.
- La macchina virtuale ha più processori logici \(LPs\).

>[!NOTE]
>vRSS è abilitato per impostazione predefinita anche per qualsiasi schede host in cui è abilitato RSS.

Di seguito sono riportate informazioni aggiuntive necessarie per completare questi passaggi di preparazione.
  
1. **Capacità della scheda di rete**. Verificare che la scheda di rete sia compatibile con Coda macchine virtuali \(VMQ\) e abbia una velocità di collegamento pari o superiore a 10 Gbps. Se la velocità del collegamento è inferiore a 10 Gbps, il Commuter virtuale Hyper\-V Disabilita VMQ per impostazione predefinita, anche se Mostra ancora VMQ come abilitato nei risultati del comando di Windows PowerShell **Get-NetAdapterVmq**. Un metodo che è possibile usare per verificare che VMQ sia abilitato o disabilitato consiste nell'usare il comando **Get-NetAdapterVmqQueue**.  Se VMQ è disabilitato, i risultati di questo comando indicano che non è stato assegnato alcun QueueID alla macchina virtuale o alla scheda di rete virtuale dell'host. 
  
2. **Abilitare VMQ**. Verificare che la funzionalità Coda macchine virtuali sia abilitata nel computer host. vRSS non funziona se l'host non supporta VMQ. È possibile verificare che VMQ sia abilitato eseguendo **Get-VMSwitch** e individuando la scheda utilizzata dal commutire virtuale. Eseguire quindi **Get-NetAdapterVmq** e controllare che la scheda venga visualizzata nei risultati e abbia Coda macchine virtuali abilitata.
  
3. **Assenza di SR\-IOV**. Verificare che un singolo input radice\-output Virtualization \(SR\-IOV\) Virtual Function \(VF\) driver non sia collegato all'interfaccia di rete VM. Per verificarlo, è possibile usare il comando **Get-NetAdapterSriov** . Se viene caricato un driver VF, RSS usa le impostazioni di scalabilità di questo driver invece di quelle configurate da vRSS. Se il driver VF non supporta RSS, vRSS è disabilitato.
  
4. **Configurazione gruppo NIC**. Se si utilizza il gruppo NIC, è importante configurare correttamente VMQ per l'utilizzo con le impostazioni gruppo NIC. Per informazioni dettagliate sulla distribuzione e la gestione del gruppo NIC, vedere [Gruppo NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

5. **Numero di LPS**. Verificare che nella macchina virtuale sia presente più di un processore logico \(LP\). vRSS si basa su RSS nella macchina virtuale o sull'host Hyper-V per bilanciare il carico del traffico ricevuto a più LPs per l'elaborazione parallela. È possibile osservare il numero di LPs della macchina virtuale eseguendo il comando di Windows PowerShell **Get-VMProcessor** nell'host. Dopo aver eseguito il comando, è possibile osservare la voce della colonna Count per il numero di LPs.

L'host vNIC ha sempre accesso a tutti i processori fisici. per configurare l'host vNIC per l'uso di un numero specifico di processori, usare le impostazioni **-BaseProcessorNumber** e **-MaxProcessors** quando si esegue il comando **set-NetAdapterRss** di Windows PowerShell.

---