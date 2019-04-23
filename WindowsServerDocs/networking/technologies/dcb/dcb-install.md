---
title: Installazione di Data Center Bridging (DCB) in Windows Server o Client Windows
description: In questo argomento vengono fornite istruzioni su come installare il Data Center Bridging in Windows Server o Client Windows.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7c20ef027279780181ff176afa39a19f2976c4c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845442"
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>Installazione Data Center Bridging \(DCB\) in Windows Server 2016 o Windows 10

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come installare i Data Center Bridging in Windows Server 2016 o Windows 10.

## <a name="prerequisites-for-using-dcb"></a>Prerequisiti per l'uso di Data Center Bridging

Di seguito sono i prerequisiti per la configurazione e la gestione DCB.

### <a name="install-a-compatible-operating-system"></a>Installare un sistema operativo compatibile

È possibile usare i comandi DCB da questa Guida in sistemi operativi seguenti.

- Windows Server (Canale semestrale)
- Windows Server 2016
- Windows 10 \(tutte le versioni\)

I sistemi operativi seguenti includono le versioni precedenti di DCB che non sono compatibili con i comandi che vengono usati nella documentazione di Data Center Bridging per Windows Server 2016 e Windows 10.

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>Requisiti hardware

Seguito è riportato un elenco di requisiti hardware per DCB.

- DCB\-scheda di rete Ethernet che supportano\(s\) deve essere installato nel computer che forniscono DCB di Windows Server 2016.
- DCB\-commutatori hardware che supportano devono essere distribuiti nella rete.


## <a name="install-dcb-in-windows-server-2016"></a>Installare i Data Center Bridging in Windows Server 2016

È possibile utilizzare le sezioni seguenti per installare i Data Center Bridging in un computer che esegue Windows Server 2016.

**Credenziali amministrative**

Per eseguire queste procedure, è necessario essere un membro del **gli amministratori**.

### <a name="install-dcb-using-windows-powershell"></a>Installare DCB tramite Windows PowerShell

È possibile utilizzare la procedura seguente per installare DCB tramite Windows PowerShell.

1. In un computer che esegue Windows Server 2016, fare clic su **avviare**, quindi fare doppio clic sull'icona di Windows PowerShell. Viene visualizzato un menu. Nel menu, fare clic su **altre**, quindi fare clic su **Esegui come amministratore**. Se richiesto, digitare le credenziali per un account con privilegi di amministratore nel computer. Verrà visualizzata la finestra di Windows PowerShell con privilegi di amministratore.
2. Digitare il comando seguente e quindi premere INVIO.

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>Installare DCB utilizzando Server Manager

È possibile utilizzare la procedura seguente per installare i Data Center Bridging tramite Server Manager.

>[!NOTE]
>Dopo aver eseguito il primo passaggio in questa procedura, il **prima di iniziare** pagina l'aggiunta guidata ruoli e funzionalità non viene visualizzata se si è scelto in precedenza **Ignora questa pagina per impostazione predefinita** quando Aggiungi È stato eseguito guidata ruoli e funzionalità. Se il **prima di iniziare** pagina non viene visualizzata, andare nel passaggio 1 al passaggio 3.

1. Su DC1, in Server Manager fare clic su **Manage**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Viene avviata l'Aggiunta guidata ruoli e funzionalità.
2. In **Prima di iniziare** fare clic su **Avanti**.
3. In **Select Installation Type**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.
4. In **Selezione server di destinazione** assicurarsi che sia selezionata l'opzione **Selezionare un server dal pool di server**. Assicurarsi che in **Pool di server** sia selezionato il computer locale. Fare clic su **Avanti**.
5. In **Selezione ruoli server**, fare clic su **Avanti**.
6. Nelle **Selezione funzionalità**, nel **funzionalità**, fare clic su **Data Center Bridging**. Consente di aprire una finestra di dialogo per chiedere se si desidera aggiungere funzionalità di Data Center Bridging necessari. Fare clic su **aggiungere funzionalità**.
7. In **Selezionare le funzionalità**, fare clic su **Avanti**. 
8. 7. in **Conferma selezioni per l'installazione**, fare clic su **installare**. Il **lo stato dell'installazione** pagina Visualizza stato durante il processo di installazione. Dopo che viene visualizzato il messaggio che segnala che l'installazione ha avuto esito positivo, fare clic su **Chiudi**.

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>Configurare il debugger del kernel per consentire QoS \(facoltativo\)

 Per impostazione predefinita, il debugger del kernel Blocca NetQos. Indipendentemente dal metodo usato per installare i Data Center Bridging, se si dispone di un debugger del kernel installato nel computer, è necessario configurare il debugger per consentire di QoS essere abilitato e configurato per eseguire il comando seguente.

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>Installare i Data Center Bridging in Windows 10

È possibile eseguire la procedura seguente in un computer Windows 10.

Per eseguire questa procedura, è necessario essere un membro del **gli amministratori**.

### <a name="install-dcb"></a>Installare DCB

1. Fare clic su **avviare**, quindi scorrere verso il basso e fare clic su **sistema Windows**.
2. Fare clic su **Pannello di controllo**. Il **Pannello di controllo** verrà visualizzata la finestra di dialogo.
3. In **Pannello di controllo**, fare clic su **visualizzare**, quindi fare clic su **icone grandi** o **icone piccole**.
4. Fare clic su **programmi e funzionalità**. Verrà visualizzata la finestra di dialogo programmi e funzionalità.
5. Nelle **programmi e funzionalità**, nel riquadro sinistro, fare clic su **o disattivazione delle funzionalità Windows attivare**. Il **funzionalità di Windows** verrà visualizzata la finestra di dialogo.
6. Nelle **funzionalità di Windows**, fare clic su **Data Center Bridging**, quindi fare clic su **OK**.

![Attivare le funzionalità di Windows o disattivare la finestra di dialogo](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


