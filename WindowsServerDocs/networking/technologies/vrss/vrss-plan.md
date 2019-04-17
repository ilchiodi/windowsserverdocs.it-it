---
title: Pianificare l'uso di vRSS
description: È possibile utilizzare questo argomento per preparare la macchina virtuale e l'host Hyper-V per uso vRSS in Windows Server 2016.
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133387"
---
# Pianificare l'uso di vRSS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In Windows Server 2016, vRSS è abilitata per impostazione predefinita, tuttavia devi preparare l'ambiente per consentire vRSS a funzionare correttamente in una macchina virtuale \(VM\) o su un \(vNIC\) scheda virtuale host. In Windows Server 2012 R2, vRSS è stata disabilitata per impostazione predefinita.

Quando si pianifica e prepara l'uso di vRSS, verifica che:

- La scheda di rete fisica è compatibile con \(VMQ\) coda di macchina virtuale e ha una velocità di collegamento di 10 Gbps o più.
- VMQ è abilitato nella scheda di rete fisica e sulla porta commutatore virtuale Hyper-V
- Non esiste alcun \(SR\-IOV\) singolo radice Input\-Output virtualizzazione configurati per la macchina virtuale.
- GRUPPO NIC è configurato correttamente.
- La macchina virtuale ha più processori logici \(LPs\).

>[!NOTE]
>vRSS anche è abilitata per impostazione predefinita per qualsiasi vnic dell'host che hanno RSS abilitata.

Di seguito è informazioni aggiuntive necessarie per completare questi passaggi di preparazione.
  
1. **Capacità della scheda di rete**. Verifica che la scheda di rete è compatibile con \(VMQ\) coda di macchina virtuale e ha una velocità di collegamento di 10 Gbps o più. Se la velocità del collegamento è minore di 10 Gbps, il commutatore virtuale Hyper-V Disabilita VMQ per impostazione predefinita, anche se viene ancora VMQ abilitato nei risultati del comando Windows PowerShell **Get-NetAdapterVmq**. Un metodo che puoi usare per verificare che VMQ è abilitato o disabilitato consiste nell'usare il comando **Get-NetAdapterVmqQueue**.  Se VMQ è disabilitato, i risultati di questo comando indicano che non vi sia alcun QueueID assegnato alla scheda di rete virtuale della macchina virtuale o host. 
  
2. **Abilitare VMQ**. Verificare che VMQ sia abilitata nel computer host. vRSS non funziona se l'host non supporta VMQ. È possibile verificare che VMQ è abilitata per l'esecuzione di **Get-VMSwitch** e trovare la scheda che utilizza il commutatore virtuale. Successivamente, eseguire **Get-NetAdapterVmq** e garantire che la scheda viene visualizzata nei risultati e ha abilitato VMQ.
  
3. **Assenza di SR\-IOV**. Verificare che un singolo radice Input\-Output virtualizzazione \(SR\-IOV\) \(VF\) funzione virtuale driver non è collegato all'interfaccia di rete della macchina virtuale. È possibile verificare ciò usando il comando di **Get-NetAdapterSriov** . Se un driver VF viene caricato, RSS Usa le impostazioni di ridimensionamento da questo driver invece di quelli configurato tramite vRSS. Se il driver VF non supporta RSS, vRSS è disabilitato.
  
4. **Gruppo NIC configurazione**. Se usi gruppo NIC, è importante che configurare correttamente VMQ per lavorare con le impostazioni di gruppo NIC. Per informazioni dettagliate sulla gestione e distribuzione gruppo NIC, vedi [Gruppo NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

5. **Numero di analisi periodica limitata**. Verificare che la macchina virtuale ha più processori logici \(LP\). vRSS si basa su RSS nella macchina virtuale o nell'host Hyper-V per caricare il traffico saldo ricevuto più logici per l'elaborazione parallela. È possibile osservare quante analisi periodica limitata la macchina virtuale ha eseguendo il comando Windows PowerShell **Get-VMProcessor** nell'host. Dopo aver eseguito il comando, è possibile osservare il movimento di colonna conteggio del numero di analisi periodica limitata.

VNIC host sempre l'accesso a tutti i processori fisici; Per configurare vNIC host per l'uso di un determinato numero di processori, Usa le impostazioni **- BaseProcessorNumber** e **-MaxProcessors** quando Esegui il comando Windows PowerShell **Set-NetAdapterRss** .

---