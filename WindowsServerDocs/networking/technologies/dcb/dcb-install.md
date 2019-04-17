---
title: Installazione di Data Center Bridging (DCB) in Windows Server o Client
description: In questo argomento vengono fornite istruzioni su come installare il Data Center Bridging in Windows Server o Client Windows.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 491bdeb1a7458be1f991be68724e7a7b51f67ecf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>Installazione di Data Center Bridging \(DCB\) in Windows Server 2016 o Windows 10

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come installare DCB in Windows Server 2016 o Windows 10.

## <a name="prerequisites-for-using-dcb"></a>Prerequisiti per l'utilizzo di Data Center Bridging

Di seguito sono i prerequisiti per la configurazione e la gestione DCB.

### <a name="install-a-compatible-operating-system"></a>Installare un sistema operativo compatibile

È possibile utilizzare i comandi DCB da questa Guida nei seguenti sistemi operativi.

- Windows Server (canale annuale e virgola)
- Windows Server 2016
- Windows 10 \(all versions\)

I sistemi operativi seguenti includono le versioni precedenti di DCB che non sono compatibili con i comandi che vengono utilizzati nella documentazione di Data Center Bridging per Windows Server 2016 e Windows 10.

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>Requisiti hardware

Ecco un elenco di requisiti hardware per DCB.

- Adapter\(s\) di rete Ethernet che supportano DCB\ deve essere installato in computer che forniscono DCB di Windows Server 2016.
- Commutatori hardware che supportano DCB\ devono essere distribuiti nella rete.


## <a name="install-dcb-in-windows-server-2016"></a>Installare Data Center Bridging in Windows Server 2016

È possibile utilizzare le sezioni seguenti per installare Data Center Bridging in un computer che esegue Windows Server 2016.

**Credenziali amministrative**

Per eseguire queste procedure, è necessario essere un membro del **amministratori**.

### <a name="install-dcb-using-windows-powershell"></a>Installare DCB utilizzando Windows PowerShell

È possibile utilizzare la procedura seguente per installare DCB tramite Windows PowerShell.

1. In un computer che esegue Windows Server 2016, fare clic su **Start**, quindi fare doppio clic sull'icona di Windows PowerShell. Viene visualizzato un menu. Nel menu, fare clic su **più**, quindi fare clic su **Esegui come amministratore**. Se richiesto, digitare le credenziali per un account che dispone di privilegi di amministratore sul computer. Verrà visualizzata la finestra di Windows PowerShell con privilegi di amministratore.
2. Digitare il comando seguente e quindi premere INVIO.

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>Installare DCB utilizzando Server Manager

È possibile utilizzare la procedura seguente per installare DCB tramite Server Manager.

>[!NOTE]
>Dopo aver eseguito il primo passaggio di questa procedura, il **prima di iniziare** pagina l'aggiunta guidata ruoli e funzionalità non viene visualizzato se è stato selezionato in precedenza **Ignora questa pagina per impostazione predefinita** quando è stato eseguito l'aggiunta guidata ruoli e funzionalità. Se il **prima di iniziare** pagina non viene visualizzata, andare al passaggio 1 al passaggio 3.

1. Su DC1, in Server Manager fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Verrà avviata l'aggiunta guidata ruoli e funzionalità.
2. In **prima di iniziare**, fare clic su **Avanti**.
3. In **Selezione tipo di installazione**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.
4. In **server di destinazione**, assicurarsi che **selezionare un server dal pool di server** sia selezionata. In **Pool di Server**, verificare che sia selezionato il computer locale. Fare clic su **Avanti**.
5. In **Selezione ruoli server**, fare clic su **Avanti**.
6. In **selezionare le funzionalità**, in **funzionalità**, fare clic su **Data Center Bridging**. Verrà visualizzata una finestra di dialogo per richiedere se vuoi aggiungere funzionalità DCB necessari. Fare clic su **aggiungere funzionalità**.
7. In **selezionare le funzionalità**, fare clic su **Avanti**. 
8. 7. In **Conferma selezioni per l'installazione**, fare clic su **installare**. Il **lo stato dell'installazione** pagina Visualizza stato durante il processo di installazione. Dopo che viene visualizzato il messaggio indicante che l'installazione ha avuto esito positivo, fare clic su **Chiudi**.

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>Configurare il debugger del kernel per consentire \(Optional\) QoS

 Per impostazione predefinita, il debugger del kernel Blocca NetQos. Indipendentemente dal metodo utilizzato per installare DCB, se si dispone di un debugger del kernel installato nel computer, è necessario configurare il debugger per consentire di QoS di essere abilitato e configurato eseguendo il comando seguente.

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>Installare Data Center Bridging in Windows 10

È possibile eseguire la procedura seguente in un computer Windows 10.

Per eseguire questa procedura, è necessario essere un membro del **amministratori **.

### <a name="install-dcb"></a>Installare Data Center Bridging

1. Fare clic su **Start**, quindi scorrere verso il basso e fare clic su **sistema Windows **.
2. Fare clic su **Pannello di controllo **. Il **Pannello di controllo** apre la finestra di dialogo.
3. In **Pannello di controllo**, fare clic su **visualizzare**, quindi fare clic su **icone grandi** o **icone piccole **.
4. Fare clic su **programmi e funzionalità **. Apre la finestra di dialogo programmi e funzionalità.
5. In **programmi e funzionalità**, nel riquadro a sinistra, fare clic su **o disattivazione delle funzionalità Windows attivare **. Il **funzionalità di Windows** apre la finestra di dialogo.
6. In **funzionalità di Windows**, fare clic su **Data Center Bridging**, quindi fare clic su **OK **.

![Attiva o disattiva la finestra di dialogo funzionalità Windows](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


