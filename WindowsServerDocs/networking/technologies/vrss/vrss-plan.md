---
title: Pianificare l'uso di vRSS
description: È possibile utilizzare questo argomento per preparare la macchina virtuale e un host Hyper-V per l'uso di vRSS in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: e6558b00e87721d8ab81c84946a14745c4faa812
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850442"
---
# <a name="plan-the-use-of-vrss"></a>Pianificare l'uso di vRSS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In Windows Server 2016, vRSS è abilitato per impostazione predefinita, tuttavia, è necessario preparare l'ambiente per consentire di vRSS funzionare correttamente in una macchina virtuale \(VM\) o in una scheda virtuale host \(vNIC\). In Windows Server 2012 R2, vRSS è stata disabilitata per impostazione predefinita.

Quando si pianifica e prepara l'uso di vRSS, verificare quanto segue:

- Scheda di rete fisica è compatibile con coda macchine virtuali \(VMQ\) e ha una velocità di collegamento pari a 10 Gbps o più.
- Coda macchine Virtuali sono abilitata l'interfaccia di rete fisica e di Hyper\-porta del commutatore virtuale V
- Non vi è alcun Single Root Input\-Output Virtualization \(SR\-IOV\) configurato per la macchina virtuale.
- Gruppo NIC sia configurato correttamente.
- La macchina virtuale con più processori logici \(LPs\).

>[!NOTE]
>vRSS è abilitata per impostazione predefinita per qualsiasi Vnic dell'host che è abilitato RSS.

Di seguito è informazioni aggiuntive necessarie per completare questi passaggi di preparazione.
  
1. **Capacità della scheda di rete**. Verificare che la scheda di rete sia compatibile con coda macchine virtuali \(VMQ\) e ha una velocità di collegamento pari a 10 Gbps o più. Se la velocità di collegamento è inferiore a 10 Gbps, l'Hyper\-commutatore virtuale V disabilita la funzionalità coda macchine Virtuali per impostazione predefinita, anche se risulta ancora VMQ abilitato nei risultati del comando Windows PowerShell **Get-NetAdapterVmq**. Un metodo è possibile usare per verificare se coda macchine Virtuali sono abilitato o disabilitato consiste nell'usare il comando **Get-NetAdapterVmqQueue**.  Se tale funzionalità è disabilitata, i risultati di questo comando mostrano che non vi sia alcun ID coda assegnato alla scheda di rete virtuale host o macchina virtuale. 
  
2. **Abilitare VMQ**. Verificare che la funzionalità Coda macchine virtuali sia abilitata nel computer host. vRSS non funziona se l'host non la supporta. È possibile verificare che coda macchine Virtuali sia abilitata eseguendo **Get-VMSwitch** e trovando la scheda che utilizza il commutatore virtuale. Eseguire quindi **Get-NetAdapterVmq** e controllare che la scheda venga visualizzata nei risultati e abbia Coda macchine virtuali abilitata.
  
3. **Assenza di SR\-IOV**. Verificare che un solo nodo radice di Input\-Output Virtualization \(SR\-IOV\) funzione virtuale \(VF\) driver non è associato all'interfaccia di rete della macchina virtuale. È possibile verificarlo utilizzando il **Get-NetAdapterSriov** comando. Se viene caricato un driver VF, RSS utilizza le impostazioni di scalabilità di tale driver invece di quelle configurate da vRSS. Se il driver VF non supporta RSS, vRSS è disabilitato.
  
4. **Configurazione gruppo NIC**. Se si Usa gruppo NIC, è importante configurare correttamente VMQ per lavorare con le impostazioni del gruppo NIC. Per informazioni dettagliate sulla gestione e distribuzione di gruppo NIC, vedere [NIC Teaming](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

5. **Numero di LPs**. Verificare che la macchina virtuale ha più di un processore logico \(LP\). vRSS si basa su RSS nella VM o nell'host Hyper-V ricevuto bilanciare il carico per più LPs per l'elaborazione parallela. È possibile osservare LPs quanti dispone di una macchina virtuale eseguendo il comando di Windows PowerShell **Get-VMProcessor** nell'host. Dopo aver eseguito il comando, è possibile osservare la voce di colonna conteggio per il numero di LPs.

VNIC host può sempre accedere a tutti i processori fisici; Per configurare vNIC host per usare un numero specifico di processori, usare le impostazioni **- BaseProcessorNumber** e **- MaxProcessors** quando si esegue la **Set-NetAdapterRss** Comando di Windows PowerShell.

---