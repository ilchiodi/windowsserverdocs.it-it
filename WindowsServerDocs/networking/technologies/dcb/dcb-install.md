---
title: Installare Data Center Bridging (DCB) in Windows Server o client
description: Questo argomento fornisce istruzioni su come installare Data Center Bridging in Windows Server o client Windows.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ecb6ef072dd2328a0a45d57d181dca9c2928a30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405790"
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>Installare Data Center Bridging \(DCB @ no__t-1 in Windows Server 2016 o Windows 10

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come installare DCB in Windows Server 2016 o Windows 10.

## <a name="prerequisites-for-using-dcb"></a>Prerequisiti per l'uso di DCB

Di seguito sono riportati i prerequisiti per la configurazione e la gestione di DCB.

### <a name="install-a-compatible-operating-system"></a>Installare un sistema operativo compatibile

È possibile usare i comandi di DCB di questa guida nei sistemi operativi seguenti.

- Windows Server (Canale semestrale)
- Windows Server 2016
- Windows 10 \(tutti Versions @ no__t-1

I sistemi operativi seguenti includono versioni precedenti di DCB che non sono compatibili con i comandi usati nella documentazione di DCB per Windows Server 2016 e Windows 10.

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>Requisiti hardware

Di seguito è riportato un elenco dei requisiti hardware per DCB.

- DCB @ no__t-0capable Ethernet Network Adapter @ no__t-1S @ no__t-2 deve essere installato nei computer che forniscono Windows Server 2016 DCB.
- DCB @ no__t-i commutatori hardware 0capable devono essere distribuiti nella rete.


## <a name="install-dcb-in-windows-server-2016"></a>Installare DCB in Windows Server 2016

È possibile usare le sezioni seguenti per installare DCB in un computer che esegue Windows Server 2016.

**Credenziali amministrative**

Per eseguire queste procedure, è necessario essere un membro di **Administrators**.

### <a name="install-dcb-using-windows-powershell"></a>Installare DCB con Windows PowerShell

È possibile utilizzare la procedura seguente per installare DCB tramite Windows PowerShell.

1. In un computer che esegue Windows Server 2016, fare clic sul pulsante **Start**, quindi fare clic con il pulsante destro del mouse sull'icona di Windows PowerShell. Viene visualizzato un menu. Nel menu fare clic su **altro**e quindi su **Esegui come amministratore**. Se richiesto, digitare le credenziali di un account con privilegi di amministratore per il computer. Windows PowerShell viene aperto con privilegi di amministratore.
2. Digitare il comando seguente e quindi premere INVIO.

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>Installare DCB usando Server Manager

È possibile utilizzare la procedura seguente per installare DCB utilizzando Server Manager.

>[!NOTE]
>Dopo aver eseguito il primo passaggio di questa procedura, la pagina **prima di iniziare** dell'aggiunta guidata ruoli e funzionalità non viene visualizzata se in precedenza è stata selezionata l'opzione **Ignora questa pagina per impostazione predefinita** durante l'aggiunta guidata ruoli e funzionalità. Se non viene visualizzata la pagina **prima di iniziare** , passare dal passaggio 1 al passaggio 3.

1. In Server Manager, in DC1 fare clic su **Gestisci**, quindi su **Aggiungi ruoli e funzionalità**. Viene avviata l'Aggiunta guidata ruoli e funzionalità.
2. In **Prima di iniziare** fare clic su **Avanti**.
3. In **Select Installation Type**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.
4. In **Selezione server di destinazione** assicurarsi che sia selezionata l'opzione **Selezionare un server dal pool di server**. Assicurarsi che in **Pool di server** sia selezionato il computer locale. Fare clic su **Avanti**.
5. In **Selezione ruoli server**, fare clic su **Avanti**.
6. In **Selezione funzionalità**, in **funzionalità**, fare clic su **Data Center Bridging**. Verrà visualizzata una finestra di dialogo in cui viene chiesto se si desidera aggiungere le funzionalità richieste di DCB. Fare clic su **Aggiungi funzionalità**.
7. In **Selezionare le funzionalità**, fare clic su **Avanti**. 
8. 7.In **confermare le selezioni**per l'installazione, fare clic su **Installa**. Il **lo stato dell'installazione** pagina Visualizza stato durante il processo di installazione. Quando viene visualizzato il messaggio indicante che l'installazione è riuscita, fare clic su **Chiudi**.

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>Configurare il debugger del kernel per consentire QoS \(Optional @ no__t-1

 Per impostazione predefinita, i debugger del kernel bloccano NetQos. Indipendentemente dal metodo usato per installare DCB, se nel computer è installato un debugger del kernel, è necessario configurare il debugger in modo da consentire l'abilitazione e la configurazione di QoS eseguendo il comando seguente.

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>Installare DCB in Windows 10

È possibile eseguire la procedura seguente in un computer Windows 10.

Per eseguire questa procedura, è necessario essere un membro di **Administrators**.

### <a name="install-dcb"></a>Installare DCB

1. Fare clic sul pulsante **Start**, quindi scorrere verso il basso e fare clic su **sistema Windows**.
2. Fare clic su **Pannello di controllo**. Verrà visualizzata la finestra di dialogo **Pannello di controllo** .
3. Nel **Pannello di controllo**fare clic su **Visualizza per**e quindi su icone **grandi** o **icone piccole**.
4. Fare clic su **programmi e funzionalità**. Verrà visualizzata la finestra di dialogo programmi e funzionalità.
5. In **programmi e funzionalità**, nel riquadro sinistro, fare clic **su attivazione o disattivazione delle funzionalità Windows**. Verrà visualizzata la finestra di dialogo **funzionalità Windows** .
6. In **funzionalità di Windows**fare clic su **Data Center Bridging**, quindi fare clic su **OK**.

![Finestra di dialogo Attivazione o disattivazione delle funzionalità Windows](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


